# Task: generate-image

## Purpose

Executes saved image prompts from `assets/prompts/` via the browser — in two modes:

- **Single mode** (`/generate-image {slug}`) — execute one specific prompt file directly
- **Batch mode** (`/generate-image` without argument) — show mode selection, then process all pending prompts

To generate and save prompts first, use `/create-image`.

Requires the **chrome-devtools MCP server** to be active and Chrome running with remote debugging.

## Inputs

- **Single mode:** `assets/prompts/image-{slug}.md`
- **Batch mode:** all `assets/prompts/image-*.md` files (skips slugs that already have a matching image)
- Chrome DevTools MCP availability (checked at task start)
- Course language from `docs/context.md` (safety-net: appended to prompt if no language instruction is already present)

## Output

- Downloaded images saved to `assets/images/{slug}.png` (or fallback path)
- Confirmation per image; batch summary at the end

## MCP Setup (required)

```bash
google-chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-debug
```

The `chrome-devtools` MCP server must be configured in VS Code's `mcp.json`.

---

## Phase 1: Entry Point

1. Check if `mcp_chrome-devtoo_*` tools are available.
   - **Not available** → explain setup, stop. Suggest `/create-image` for prompt-only mode.

2. Resize the browser viewport: use `mcp_chrome-devtoo_resize_page` → width: 1280, height: 900.
   This ensures stop-button and send-button are rendered (they may be hidden on narrow viewports).

3. Determine mode:
   - **Slug provided** → skip to [Single Mode (Phase 2a)](#phase-2a-single-mode).
   - **No argument** → 🎛️ ask with structured question (single choice):
     - **Single** — enter a slug to execute one prompt
     - **Sequential batch** — process all pending prompts one by one, agent controls each step
     - **Automated batch** — inject a JS loop into the browser, runs fully unattended

---

## Phase 2a: Single Mode

3. Read `assets/prompts/image-{slug}.md`.
4. Extract the `Complete Prompt:` section (the quoted string).
5. Execute (Phase 3 → 4 → 5 below), then stop.

---

## Phase 2b: Batch Mode — Collect Queue

3. Scan `assets/prompts/` for all files matching `image-*.md`.
4. Derive slug per file (e.g. `image-fox-samurai.md` → `fox-samurai`).
5. Check if `assets/images/{slug}.png` already exists:
   - **Exists** → mark `⏭ skipped`
   - **Missing** → add to queue
6. Print queue:
   ```
   Batch queue: {N} to process, {M} skipped (already done)
   - fox-samurai  → assets/images/fox-samurai.png
   - whale-astronaut → assets/images/whale-astronaut.png
   ```
7. 🎛️ Confirm: **Start / Cancel**

### Sequential Batch

Process each item using Phase 3 → 4 → 5 in order.
After each image: log result (`✅ done` / `❌ failed`), continue to next.

### Automated Batch

8. Read all pending prompt files, extract `Complete Prompt:` strings. Read course language from `docs/context.md`. For each prompt, if it does not already contain an in-image language instruction, append: `"All text visible in the image (labels, headings, UI elements) must be written in {language}."`
9. Inject the following self-contained JS loop into the browser:

   ```js
   const queue = [
     { slug: 'fox-samurai',     prompt: '...' },
     { slug: 'whale-astronaut', prompt: '...' },
     // one entry per pending prompt
   ];

   async function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

   async function waitForGenerationDone(timeout = 150000) {
     const start = Date.now();

     const urlsBefore = new Set(
       [...document.querySelectorAll('img')]
         .map(i => i.src)
         .filter(s => s.includes('file_'))
     );

     // Phase 1: wait for stop-button to appear
     while (Date.now() - start < 20000) {
       if (document.querySelector('[data-testid="stop-button"]')) break;
       await sleep(500);
     }

     // Phase 2: wait for stop-button gone AND new image URL in DOM
     while (Date.now() - start < timeout) {
       const stopGone = !document.querySelector('[data-testid="stop-button"]');
       const newImg = [...document.querySelectorAll('img')]
         .map(i => i.src)
         .filter(s => s.includes('file_'))
         .some(s => !urlsBefore.has(s));
       if (stopGone && newImg) {
         await sleep(2000); // grace period for full-resolution render
         return true;
       }
       await sleep(1500);
     }
     return false; // timeout
   }

   function getGeneratedImageUrl() {
     const seen = new Set();
     const imgs = [...document.querySelectorAll('img')]
       .map(i => i.src)
       .filter(s => s.includes('chatgpt.com') && s.includes('file_'))
       .filter(s => {
         const match = s.match(/file_[^&?]+/);
         const id = match ? match[0] : s;
         if (seen.has(id)) return false;
         seen.add(id);
         return true;
       });
     return imgs.length > 0 ? imgs[imgs.length - 1] : null;
   }

   async function downloadBlob(url, filename) {
     const blob = await fetch(url).then(r => r.blob());
     const a = document.createElement('a');
     a.href = URL.createObjectURL(blob);
     a.download = filename;
     document.body.appendChild(a); a.click(); document.body.removeChild(a);
     return blob.size;
   }

   async function runBatch() {
     // ChatGPT must already be open — no navigation here (window.location.href kills the script context)
     for (const { slug, prompt } of queue) {
       console.log(`[batch] Starting: ${slug}`);
       const tb = document.getElementById('prompt-textarea');
       tb.focus();
       tb.textContent = prompt;
       tb.dispatchEvent(new InputEvent('input', { bubbles: true, inputType: 'insertText', data: prompt }));
       // poll for send-button (only rendered when textarea has content)
       let sendBtn;
       const deadline = Date.now() + 10000;
       while (Date.now() < deadline) {
         sendBtn = document.querySelector('[data-testid="send-button"]');
         if (sendBtn) break;
         await sleep(200);
       }
       if (!sendBtn) { console.warn(`[batch] ❌ Send button not found: ${slug}`); continue; }
       sendBtn.click();
       const done = await waitForGenerationDone();
       if (!done) { console.warn(`[batch] ❌ Timeout: ${slug}`); continue; }
       const imgUrl = getGeneratedImageUrl();
       if (!imgUrl) { console.warn(`[batch] ❌ No image found: ${slug}`); continue; }
       const size = await downloadBlob(imgUrl, `${slug}.png`);
       console.log(`[batch] ✅ Done: ${slug} (${Math.round(size/1024)} KB)`);
       await sleep(1000);
     }
     console.log('[batch] All done.');
   }

   runBatch();
   ```

10. Monitor browser console for `[batch] ✅ / ❌` logs.
11. After completion, collect results from console output.

---

## Phase 3: Submit to ChatGPT *(single + sequential batch)*

- **First image only:** Navigate to `https://chatgpt.com/`. For subsequent images in sequential batch, stay on the same page — just insert the next prompt.
- **Language safety-net:** Read course language from `docs/context.md`. If the prompt does not already contain a language instruction for in-image text (i.e., does not mention "text visible in the image"), append to the prompt:  
  `"All text visible in the image (labels, headings, UI elements) must be written in {language}."`
- Insert prompt and submit — poll for send-button at 200ms intervals (it only renders when textarea has content):
  ```js
  const tb = document.getElementById('prompt-textarea');
  tb.focus();
  tb.textContent = prompt;
  tb.dispatchEvent(new InputEvent('input', { bubbles: true, inputType: 'insertText', data: prompt }));
  // poll for send-button (only rendered when textarea has content)
  let sendBtn;
  const deadline = Date.now() + 10000;
  while (Date.now() < deadline) {
    sendBtn = document.querySelector('[data-testid="send-button"]');
    if (sendBtn) break;
    await sleep(200);
  }
  if (!sendBtn) throw new Error('Send button not found after 10s');
  sendBtn.click();
  ```

## Phase 4: Wait for Image *(single + sequential batch)*

- Use a dual-check: snapshot URLs before click, wait for stop-button gone **and** a new `file_` URL in the DOM:
  ```js
  async function waitForGenerationDone(timeout = 150000) {
    const start = Date.now();

    // Snapshot of image URLs present BEFORE this prompt was submitted
    const urlsBefore = new Set(
      [...document.querySelectorAll('img')]
        .map(i => i.src)
        .filter(s => s.includes('file_'))
    );

    // Phase 1: wait for stop-button to appear (confirms generation started)
    while (Date.now() - start < 20000) {
      if (document.querySelector('[data-testid="stop-button"]')) break;
      await sleep(500);
    }

    // Phase 2: wait for stop-button gone AND new image URL in DOM
    while (Date.now() - start < timeout) {
      const stopGone = !document.querySelector('[data-testid="stop-button"]');
      const newImg = [...document.querySelectorAll('img')]
        .map(i => i.src)
        .filter(s => s.includes('file_'))
        .some(s => !urlsBefore.has(s));

      if (stopGone && newImg) {
        await sleep(2000); // grace period for full-resolution render
        return true;
      }
      await sleep(1500);
    }
    return false;
  }
  ```
  - Do **not** rely solely on button state — `[data-testid="send-button"]` is only rendered when the textarea has content; after generation the textarea is empty → button absent → the old approach always timed out.
  - The `urlsBefore` snapshot is taken at function entry, immediately after the click. Image generation takes several seconds, so the race condition risk is negligible.
  - After `waitForGenerationDone()` returns `true`, fetch the last matching image URL from the DOM. Filter: `s.includes('chatgpt.com') && s.includes('file_')`. Note: the `file_` ID is in the query parameter, not the path. Deduplicate by `file_` ID to avoid counting the same image multiple times.
  - Timeout (150s) → report `❌ failed`, stop (single) or continue (batch).

## Phase 5: Download and Save *(single + sequential batch)*

- Determine save path:
  - `assets/images/` exists → `assets/images/{slug}.png`
  - `assets/` exists → `assets/{slug}.png`
  - Neither → `~/Downloads/{slug}.png` (inform instructor)
- Download via Blob URL:
  ```js
  fetch(url).then(r => r.blob()).then(blob => {
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = '{slug}.png';
    document.body.appendChild(a); a.click(); document.body.removeChild(a);
  });
  ```
- Confirm: `"✅ {slug}.png saved ({size} KB) → {path}"`

---

## Phase 6: Summary *(batch modes only)*

```
Batch complete.
✅  3 images generated and saved to assets/images/
⏭   1 skipped (already existed)
❌  0 failed
```
If any failures: list slugs, suggest `/generate-image {slug}` to retry individually.

---

## Relation to /create-image

| Feature                  | `/create-image` | `/generate-image {slug}` | `/generate-image` (batch) |
|--------------------------|-----------------|--------------------------|---------------------------|
| Generate prompt          | ✅              | ❌ (reads saved)          | ❌ (reads saved)           |
| Save prompt to file      | ✅ always        | ❌                         | ❌                         |
| Submit to ChatGPT        | ❌              | ✅ single                 | ✅ all pending             |
| Download image           | ❌              | ✅                         | ✅                         |
| Save to project folder   | ❌              | ✅                         | ✅                         |
| Requires chrome-devtools | ❌              | ✅                         | ✅                         |
