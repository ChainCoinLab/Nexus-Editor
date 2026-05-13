# Editor Core Spec

## ADDED Requirements

### Requirement: expose all editor selection ranges

The editor core SHALL expose every active CodeMirror selection range through an additive public API while preserving the existing primary-selection API.

#### Scenario: Read multiple selection ranges
- **GIVEN** the editor has multiple active selection ranges
- **WHEN** `getSelections()` is called
- **THEN** it SHALL return all ranges in document order
- **AND** each range SHALL include `anchor`, `head`, `from`, `to`, and `empty`

#### Scenario: Existing primary selection API remains compatible
- **GIVEN** the editor has multiple active selection ranges
- **WHEN** `getSelection()` is called
- **THEN** it SHALL return the primary selection range only
- **AND** the returned shape SHALL remain `{ anchor, head }`

### Requirement: set multiple editor selection ranges

The editor core SHALL allow host applications and plugins to set multiple selection ranges without collapsing them to a single cursor.

#### Scenario: Set multiple ranges
- **WHEN** `setSelections()` is called with two or more valid ranges
- **THEN** the editor SHALL update CodeMirror state with all provided ranges
- **AND** the primary range SHALL be selected by `primaryIndex` when provided

#### Scenario: Invalid or empty range input is rejected safely
- **WHEN** `setSelections()` is called with an empty range list or out-of-document positions
- **THEN** the editor SHALL avoid throwing to host code
- **AND** the document content SHALL remain unchanged

### Requirement: emit multi-selection changes

The editor core SHALL emit full-range selection updates without changing the existing primary-only `selectionChange` event contract.

#### Scenario: Multiple selections change
- **WHEN** editor selection changes to multiple ranges
- **THEN** `selectionsChange` SHALL emit every range
- **AND** `selectionChange` SHALL continue to emit the primary range only
