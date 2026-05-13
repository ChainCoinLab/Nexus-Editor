## 1. Specification
- [ ] 1.1 Confirm proposal scope with maintainers before implementation.

## 2. Core API
- [ ] 2.1 Add exported selection range types.
- [ ] 2.2 Add `getSelections()` and `setSelections(ranges, primaryIndex?)` to `EditorAPI`.
- [ ] 2.3 Add `selectionsChange` event while preserving existing `selectionChange` behavior.
- [ ] 2.4 Add unit tests for primary compatibility, multiple ranges, reversed ranges, and empty selections.

## 3. Live Preview
- [ ] 3.1 Audit all live-preview selection checks for range awareness.
- [ ] 3.2 Add regression tests for secondary ranges intersecting headings, code blocks, images, and tables.

## 4. Table Interactions
- [ ] 4.1 Verify table widget rAF cleanup respects multi-selection ranges and active table range selection.
- [ ] 4.2 Add regression coverage for table edit, range select, grip select, grip drag, outside click, and delete-key paths with multiple editor ranges.

## 5. Validation
- [ ] 5.1 Run `pnpm vitest run packages/core/test`.
- [ ] 5.2 Run `pnpm --filter @floatboat/nexus-core build`.
