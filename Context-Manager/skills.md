# Shared agent skills

The skills in `.agents/skills/` are tracked with the repository so contributors can discover them in a Codex-compatible environment. The repository copies are the shared versions; they must not depend on paths under a particular contributor's home directory.

## Included skills

| Skill | Use it for | Notes |
| --- | --- | --- |
| [`design-taste-frontend`](../.agents/skills/design-taste-frontend/SKILL.md) | New or redesigned landing pages, portfolios, and editorial frontends. | Its own scope says it is not the default for dashboards and data tables. Preserve Finora's visual system in [`DESIGN.md`](../DESIGN.md). |
| [`redesign-existing-projects`](../.agents/skills/redesign-existing-projects/SKILL.md) | Auditing and improving the existing app without replacing its product behavior. | Prefer this for a broad visual upgrade of Finora's current surfaces. |
| [`impeccable`](../.agents/skills/impeccable/SKILL.md) | Detailed UI critique, UX polish, accessibility, responsive behavior, and interaction refinement. | The full skill includes optional scripts and workflows; only use those when the runtime and dependencies are available. |
| [`imagegen-frontend-web`](../.agents/skills/imagegen-frontend-web/SKILL.md) | Creating visual references for website sections. | Requires a compatible image-generation tool. It generates references; it does not implement the UI. |
| [`playwright-cli`](../.agents/skills/playwright-cli/SKILL.md) | Browser interaction, screenshot/snapshot inspection, and web verification. | Requires the `playwright-cli` executable and a usable browser. |
| [`web-design-guidelines`](../.agents/skills/web-design-guidelines/SKILL.md) | Accessibility and Vercel Web Interface Guidelines review. | The included reference is a snapshot; check the official current guidelines when recency matters. |
| [`transform-ai-slop-websites`](../.agents/skills/transform-ai-slop-websites/SKILL.md) | Auditing generic or inconsistent visual patterns in an existing site. | Apply only when the task is a visual redesign, not for backend-only changes. |

## Selecting a skill

- Use one skill that fits the request; do not stack every design skill on every UI change.
- Use `design-taste-frontend` for a landing-page direction, `redesign-existing-projects` for broad work in the existing app, or `impeccable` for focused UI critique/polish.
- Use `playwright-cli` when actual browser behavior or visual evidence needs checking. A local build is not browser verification.
- Use `imagegen-frontend-web` when the work needs section-specific visual references. Skip it when the design is code-native or the user asks only for a functional fix.
- The repo skill files do not grant permission to publish, alter hosted settings, transmit private data, or make destructive changes. Follow the request and repository instructions for those actions.

## Maintenance

Keep each skill's `SKILL.md` and any referenced `references/`, `scripts/`, `assets/`, or `agents/` resources together. When adding a skill, add its use case and actual tool requirements here. Do not copy the contributor's entire personal skill library; keep the shared set relevant to Finora.
