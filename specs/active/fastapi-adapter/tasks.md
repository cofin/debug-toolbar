# Task Breakdown: FastAPI Adapter

**Complexity**: Low (depends on PR #7 Starlette Adapter)
**PRD**: `specs/active/fastapi-adapter/prd.md`
**Dependency**: Must complete Starlette Adapter first

---

## Implementation Tasks

### Phase 1: Core Implementation

- [ ] **Task 1.1**: Create module structure
  - `src/debug_toolbar/fastapi/__init__.py` - Public exports
  - `src/debug_toolbar/fastapi/config.py` - FastAPI config
  - `src/debug_toolbar/fastapi/plugin.py` - FastAPI integration

- [ ] **Task 1.2**: Implement `FastAPIDebugToolbarConfig`
  - Extend `StarletteDebugToolbarConfig`
  - Add `track_dependencies: bool = True`
  - Add FastAPI-specific default panels
  - ~50 lines

- [ ] **Task 1.3**: Implement `setup_debug_toolbar()` function
  - Add middleware to FastAPI app
  - Patch dependency resolution (if enabled)
  - Mount debug toolbar routes
  - ~50 lines

### Phase 2: Dependencies Panel

- [ ] **Task 2.1**: Create `DependenciesPanel`
  - `src/debug_toolbar/fastapi/panels/dependencies.py`
  - Track resolved dependencies
  - Show timing per dependency
  - Indicate cached vs fresh resolution
  - ~80 lines

- [ ] **Task 2.2**: Implement dependency tracking
  - Hook into FastAPI's `solve_dependencies`
  - Track dependency name, duration, cached status
  - Handle nested dependencies
  - ~100 lines

### Phase 3: Example & Documentation

- [ ] **Task 3.1**: Create FastAPI example application
  - `examples/fastapi_app.py`
  - Routes with dependencies
  - Demonstrates dependency panel
  - ~100 lines

- [ ] **Task 3.2**: Create documentation
  - `docs/frameworks/fastapi.md`
  - Installation, usage, configuration

### Phase 4: Testing

- [ ] **Test 1**: Unit tests for `DependenciesPanel`
  - `tests/fastapi/panels/test_dependencies.py`
  - Track dependency, cached detection, timing
  - ~200 lines

- [ ] **Test 2**: Integration tests
  - `tests/fastapi/test_integration.py`
  - Full request cycle with dependencies
  - ~200 lines

---

## Files to Create

1. `src/debug_toolbar/fastapi/__init__.py` (~20 lines)
2. `src/debug_toolbar/fastapi/config.py` (~50 lines)
3. `src/debug_toolbar/fastapi/plugin.py` (~50 lines)
4. `src/debug_toolbar/fastapi/panels/__init__.py` (~10 lines)
5. `src/debug_toolbar/fastapi/panels/dependencies.py` (~80 lines)
6. `examples/fastapi_app.py` (~100 lines)
7. `docs/frameworks/fastapi.md` (~100 lines)

---

## Quality Gates

- [ ] 90%+ test coverage for new modules
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] Works with Starlette middleware (code reuse)
