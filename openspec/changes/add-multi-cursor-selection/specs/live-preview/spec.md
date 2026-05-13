# Live Preview Spec

## ADDED Requirements

### Requirement: support multi-selection-aware live preview

Live preview SHALL consider every active editor selection range when deciding whether raw markdown syntax must remain visible and editable.

#### Scenario: Secondary range intersects a preview node
- **GIVEN** the primary cursor is outside a rendered live-preview node
- **AND** a secondary selection range intersects that node
- **WHEN** live-preview decorations are rebuilt
- **THEN** the intersecting node SHALL reveal its editable markdown source

#### Scenario: Multiple cursors outside preview nodes
- **GIVEN** multiple cursor ranges are active outside a preview node
- **WHEN** live-preview decorations are rebuilt
- **THEN** the preview node SHALL remain rendered in preview mode

### Requirement: preserve table widget interactions with multiple selections

Editable table widgets SHALL preserve their existing editing locks, range-selection state, and cleanup behavior when multiple editor selection ranges are active.

#### Scenario: Table interaction is not interrupted by secondary ranges
- **GIVEN** multiple editor selection ranges are active
- **WHEN** the user edits a table cell, selects a cell range, clicks a grip, drags a grip, clicks outside, or presses Delete on a table selection
- **THEN** the table interaction SHALL follow the same behavior as with a single editor selection
- **AND** the widget DOM SHALL not be rebuilt mid-interaction
