# Change: Add multi-cursor and multi-selection support

## Why
Nexus currently exposes and emits only the primary CodeMirror selection, which prevents host applications and plugins from supporting multi-cursor editing workflows. CodeMirror 6 already supports multiple ranges, but Nexus needs explicit API semantics and live-preview safeguards before enabling the behavior.

## What Changes
- Add editor-core selection APIs and events that expose all selection ranges while preserving existing primary-selection APIs.
- Enable multi-selection creation through host-provided CodeMirror extensions and future built-in shortcuts without collapsing secondary ranges.
- Update live-preview and table-widget behavior so decoration rebuilds and table interactions remain stable when multiple ranges are active.
- Add tests covering multi-range API behavior, selection events, live-preview decoration stability, and table interaction regressions.

## Impact
- Affected specs: editor-core, live-preview
- Affected code: `packages/core/src/types.ts`, `packages/core/src/editor.ts`, `packages/core/src/live-preview.ts`, `packages/core/src/live-preview-ranges.ts`, `packages/core/src/live-preview-table.ts`, core tests
- Breaking changes: none; existing `getSelection()`, `setSelection()`, and `selectionChange` semantics remain primary-selection based
