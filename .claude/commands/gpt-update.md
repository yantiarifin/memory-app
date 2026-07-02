---
description: Generate a shareable update of repo changes since the last sync, for pasting into ChatGPT
argument-hint: "[base-ref]  (optional: a commit SHA, tag, or e.g. HEAD~1 to override the range)"
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(git rev-parse:*), Bash(git show:*), Bash(git remote:*), Bash(git merge-base:*), Read, Write
---

# /gpt-update — brief ChatGPT on what changed

The user is co-developing this project with ChatGPT, which **cannot browse the
GitHub repo**. Your job: produce a **self-contained, copy-pasteable brief** that
catches ChatGPT up on everything that changed since the last sync — so it needs
no repo access to understand the current state.

## Step 1 — Determine the base ref (where the last update left off)

- If `$ARGUMENTS` is non-empty, use it verbatim as the base ref.
- Otherwise, read the marker file `.claude/gpt-sync` (one line: a commit SHA).
  - If it exists, the base ref is that SHA.
  - If it does NOT exist (first run), set base ref to the repository's **root
    commit** so this first brief covers the whole project. Get it with:
    `git rev-list --max-parents=0 HEAD`. Mention in the brief that this is the
    initial full catch-up.
- Let `HEAD` be the current commit: `git rev-parse HEAD`.
- If base == HEAD, tell the user there's nothing new since the last sync and stop
  (do not write a marker or emit a brief).

## Step 2 — Gather the changes

Run, using the resolved base ref:

- `git remote get-url origin` — to build GitHub links.
- `git log --reverse --pretty=format:'%h %s' <base>..HEAD` — commits in the range.
- `git diff --stat <base>..HEAD` — files touched and size.
- `git diff <base>..HEAD` — the actual changes. **Read and understand these** —
  the brief must summarize the substance, not just file names.

## Step 3 — Write the brief

Print it between clear delimiters so the user can copy it cleanly. Do **not** wrap
the whole thing in a code fence (it contains markdown). Use exactly this frame:

```
===== COPY BELOW THIS LINE — PASTE TO CHATGPT =====
<the brief>
===== COPY ABOVE THIS LINE =====
```

The brief itself should contain, in this order:

1. **Heading** — `# DiscoveryApp — repo update` and one line with the repo URL and
   the commit range (`<base-short>..<head-short>`).
2. **TL;DR** — 2–4 sentences: what meaningfully changed and why it matters.
3. **Changes** — grouped by area (docs, decisions/ADRs, code, config), each a
   short bullet describing *what* and *why*, not a raw diff. Quote short, telling
   snippets only when they carry the meaning.
4. **New / changed decisions** — if any ADRs or architecture decisions changed,
   list them explicitly with their resolution, since these are the things ChatGPT
   most needs to track.
5. **Current state & open questions** — where the project stands now and what's
   undecided, so ChatGPT can reason about next steps.
6. **Reference links** — the GitHub compare link
   (`<repo>/compare/<base>...<head>`) and `blob/<head-sha>/<path>` links for the
   key files, so the user *can* point ChatGPT at them if needed (but the brief
   stands alone without them).

Keep it tight and high-signal. Honor the project's "calm / less is more" tenets —
this is a briefing, not a changelog dump.

## Step 4 — Advance the marker

After printing the brief, write the current HEAD SHA (full, single line, no
trailing text) to `.claude/gpt-sync`. Then tell the user, outside the copy block:

- that the marker advanced to `<head-short>`, so the next `/gpt-update` will start
  from here;
- that if they did **not** actually share this brief, they can regenerate from the
  previous point with `/gpt-update <previous-base-sha>` (give them the exact SHA).
