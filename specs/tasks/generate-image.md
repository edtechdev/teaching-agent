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

2. Determine mode:
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

8. Read all pending prompt files, extract `Complete Prompt:` strings.
9. Inject the following self-contained JS loop into the browser:

   ```js
   const queue = [
     { slug: 'fox-samurai',     prompt: '...' },
     { slug: 'whale-astronaut', prompt: '...' },
     // one entry per pending prompt
   ];

   async function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

   async function waitForGenerationComplete(timeout = 120000) {
     const start = Date.now();
     while (Date.now() - start < timeout) {
       const stopBtn = document.querySelector('[data-testid="stop-button"]');
       const sendBtn = document.querySelector('[data-testid="send-button"]');
       if (!stopBtn && sendBtn) return true; // generation finished
       await sleep(2000);
     }
     return false; // timeout
   }

   function getGeneratedImageUrl() {
     const imgs = [...document.querySelectorAll('img')]
       .map(i => i.src)
       .filter(s => s.includes('estuary') && s.includes('file_'));
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
     // Navigate once before the first image
     window.location.href = 'https://chatgpt.com/';
     await sleep(3000);

     for (const { slug, prompt } of queue) {
       console.log(`[batch] Starting: ${slug}`);
       const tb = document.getElementById('prompt-textarea');
       tb.focus();
       tb.textContent = prompt;
       tb.dispatchEvent(new InputEvent('input', { bubbles: true, inputType: 'insertText', data: prompt }));
       document.querySelector('[data-testid="send-button"]').click();
       const done = await waitForGenerationComplete();
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
- Insert prompt:
  ```js
  const tb = document.getElementById('prompt-textarea');
  tb.focus();
  tb.textContent = prompt;
  tb.dispatchEvent(new InputEvent('input', { bubbles: true, inputType: 'insertText', data: prompt }));
  ```
- Submit:
  ```js
  document.querySelector('[data-testid="send-button"]').click();
  ```

## Phase 4: Wait for Image *(single + sequential batch)*

- Poll until generation is complete — the stop-button disappears **and** the send-button reappears:
  ```js
  async function waitForGenerationComplete(timeout = 120000) {
    const start = Date.now();
    while (Date.now() - start < timeout) {
      const stopBtn = document.querySelector('[data-testid="stop-button"]');
      const sendBtn = document.querySelector('[data-testid="send-button"]');
      if (!stopBtn && sendBtn) return true;
      await sleep(2000);
    }
    return false;
  }
  ```
  - Do **not** use the presence of an `estuary` URL as the ready signal — those appear early as low-res previews.
  - After `waitForGenerationComplete()` returns `true`, fetch the last `estuary` URL from the DOM — that is the full-resolution image.
  - Timeout (120s) → report `❌ failed`, stop (single) or continue (batch).

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
