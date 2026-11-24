---
name: web-design-guidelines
description: Review UI code for Vercel Web Interface Guidelines compliance. Use when asked to "review my UI", "check accessibility", "audit design", "review UX", or "check my site against best practices".
metadata:
  author: vercel
  version: "1.0.0"
  argument-hint: <file-or-pattern>
---

# Web Interface Guidelines (Vercel Design)

Audit UI code, layouts, and components against Vercel's official Web Interface Guidelines (`https://vercel.com/design/guidelines`).

## How It Works

1. Read local guidelines in `references/guidelines.md` (or optionally fetch latest from `https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md`).
2. Read the target component/page files.
3. Check against all rules across:
   - **Interactions**: Keyboard operability, visible `:focus-visible`, hit target sizes (≥24px desktop, ≥44px mobile), no paste blocking, loading buttons retaining labels, deep linking.
   - **Animations**: Honor `prefers-reduced-motion`, animate `transform`/`opacity` only, never `transition: all`, compositor-friendly GPU transforms.
   - **Layout**: Optical alignment, responsive coverage, safe areas for notches, intrinsic CSS layouts.
   - **Content & Typography**: Inline help, stable skeleton dimensions matching final content, tabular numbers (`tabular-nums`), curly quotes, no dead ends.
   - **Forms**: Autocomplete attributes, correct input types, inline error placement, autofocus rules.
4. Output findings in concise `file:line` format with actionable recommendations.
