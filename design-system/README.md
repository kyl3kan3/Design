# Attention Lab — Design System bundle

Card previews for a Claude Design (claude.ai/design) design-system project, extracted
from the concept demos in `../concepts/`:

- `foundations/` — Colors, Typography, and Motion (live duration/easing/stagger demos)
- `components/` — ten reusable motion components: magnetic button, spring toggle,
  morphing submit, heart burst, star cascade, toast stack, bell + unread badge,
  spotlight card, redaction reveal, stat counter
- `_registry.json` — card names, groups, and viewports for the Design System pane

Each file is self-contained (no dependencies) and starts with the
`<!-- @dsCard group="…" -->` marker the Design System pane indexes. The full concept
pages in `../concepts/` join these as a third "Concepts" group when synced.
