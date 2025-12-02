# Task Breakdown: GraphQL Panel (Strawberry Integration)

**Complexity**: Medium (8 checkpoints)
**PRD**: `specs/active/graphql-panel/prd.md`

---

## Implementation Tasks

### Phase 1: Strawberry Extension

- [ ] **Task 1.1**: Create `DebugToolbarExtension` class
  - `src/debug_toolbar/extras/strawberry/extension.py`
  - Extend `SchemaExtension`
  - Implement `on_operation()` context manager
  - ~100 lines

- [ ] **Task 1.2**: Implement resolver tracking
  - `resolve()` method wrapping
  - Track resolver name, duration
  - Aggregate by field
  - ~80 lines

- [ ] **Task 1.3**: Create `GraphQLTracker` class
  - Store operations and resolver timings
  - Request-scoped tracking
  - ~60 lines

### Phase 2: GraphQL Panel

- [ ] **Task 2.1**: Create `GraphQLPanel` class
  - `src/debug_toolbar/extras/strawberry/panel.py`
  - ClassVar metadata
  - Lifecycle hooks
  - ~100 lines

- [ ] **Task 2.2**: Implement `generate_stats()` method
  - Operation details (name, type, variables)
  - Resolver timing breakdown
  - Error tracking with locations
  - ~80 lines

- [ ] **Task 2.3**: Optional query complexity analysis
  - Calculate complexity score
  - Configurable via panel init
  - ~50 lines

### Phase 3: Testing

- [ ] **Test 1**: Unit tests for extension
  - Operation tracking, resolver timing
  - ~150 lines

- [ ] **Test 2**: Unit tests for panel
  - Stats generation, error handling
  - ~150 lines

- [ ] **Test 3**: Integration tests
  - With real Strawberry schema
  - ~100 lines

---

## Files to Create

1. `src/debug_toolbar/extras/strawberry/__init__.py` (~15 lines)
2. `src/debug_toolbar/extras/strawberry/extension.py` (~150 lines)
3. `src/debug_toolbar/extras/strawberry/panel.py` (~150 lines)
4. `tests/unit/test_graphql_panel.py` (~300 lines)
5. `tests/integration/test_strawberry_integration.py` (~100 lines)

---

## Acceptance Criteria

- [ ] Track GraphQL operations (query/mutation/subscription)
- [ ] Display operation name and type
- [ ] Resolver timing breakdown
- [ ] Variable inspection
- [ ] Error tracking with GraphQL locations
- [ ] Query complexity display (if enabled)
- [ ] 90%+ test coverage

---

## Quality Gates

- [ ] 90%+ test coverage
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] Works with Strawberry >= 0.200.0
