## Context
CodeMirror 6 represents selections as `EditorSelection` with one or more ranges and one primary range. Nexus currently narrows that model to `{ anchor, head }`, which is easy for simple integrations but loses secondary cursors.

Live preview and table widgets already consume `state.selection.ranges` internally in several places, but public API and event contracts do not expose the full range set. The table widget also has strict interaction rules around DOM reuse and editing locks, so multi-selection support must preserve those invariants.

## Goals / Non-Goals
- Goals: expose full selection ranges, keep existing primary-selection APIs compatible, preserve live-preview behavior under multiple ranges, and document table-widget interaction safeguards.
- Non-goals: implement every platform shortcut for adding cursors, add collaborative cursors, or redesign table range selection.

## Decisions

### Add additive API instead of changing existing methods
Add `getSelections()` and `setSelections(ranges, primaryIndex?)` rather than changing `getSelection()` / `setSelection()`. Existing consumers continue to work with the primary range, while advanced consumers can opt into multi-range behavior.

### Add a dedicated multi-selection event
Keep `selectionChange` primary-only for compatibility and add `selectionsChange` for all ranges. This avoids changing handler payloads in existing integrations.

### Treat live-preview selection checks as range-aware
Live-preview helpers that already accept `SelectionRange[]` should remain range-aware. Any code still reading `selection.main` for decoration visibility must be audited so secondary ranges also reveal editable source where expected.

### Preserve table-widget editing locks
Table mouse interactions must keep using explicit editing locks and must not rely on selection-only transactions to rebuild widget DOM mid-interaction. Multi-selection changes should not clear active table range selection unless the existing table interaction state allows it.

## Risks / Mitigations
- **Existing plugins assume one selection.** Keep primary APIs unchanged and add new APIs/events.
- **Live-preview widgets flicker under secondary cursors.** Add regression tests for multiple ranges intersecting preview nodes.
- **Table interactions conflict with editor multi-selection.** Gate table cleanup on the existing active interaction flags and add tests for table selection with secondary editor ranges.
