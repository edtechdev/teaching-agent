# Task: manage-git

## Purpose

Supports users (especially beginners) in all git and GitHub related tasks: pulling, pushing, staging, committing, viewing diffs, resolving conflicts, and writing meaningful commit messages.

## Inputs

- User's git/GitHub experience (always ask before proceeding)
- Current workspace files and changes
- User's intent (what do they want to do: pull, push, commit, resolve, etc.)

## Output

- Guided git operations (pull, push, stage, commit, diff, resolve conflicts)
- Explanations and step-by-step instructions for each action
- Suggestions for meaningful commit messages

## Steps

1. Ask the user about their git/GitHub experience and clarify their intent (what do they want to do?).
2. Explain the basics of git operations (staging, committing, pushing, pulling, resolving conflicts) as needed.
3. Guide the user through:
   - Staging files (explain what staging means)
   - Writing a clear, meaningful commit message (suggest examples)
   - Committing changes
   - Pulling latest changes from remote
   - Pushing local commits to GitHub
   - Viewing diffs and status
   - Resolving merge conflicts (step-by-step)
4. Offer to automate common operations or let the user do them manually.
5. Confirm each step with the user before proceeding, and explain any errors or issues.
6. Provide links to official git and GitHub documentation for further learning.

## Usage

This task is invoked when:
- The user needs help with any git or GitHub operation
- Beginners need step-by-step guidance
- There are errors, conflicts, or uncertainty about version control
