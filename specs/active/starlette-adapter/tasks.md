# Task Breakdown: Starlette Adapter

**Complexity**: Medium (8 checkpoints)
**PRD**: `specs/active/starlette-adapter/prd.md`

---

## Implementation Tasks

### Phase 1: Core Implementation (Checkpoints 1-3)

- [ ] **Task 1.1**: Create module structure
  - `src/debug_toolbar/starlette/__init__.py` - Public exports
  - `src/debug_toolbar/starlette/config.py` - Starlette config
  - `src/debug_toolbar/starlette/middleware.py` - Pure ASGI middleware

- [ ] **Task 1.2**: Implement `StarletteDebugToolbarConfig`
  - Extend `DebugToolbarConfig`
  - Add `exclude_paths`, `exclude_patterns`
  - Add `show_toolbar_callback`
  - Implement `should_show_toolbar()` method
  - ~85 lines

- [ ] **Task 1.3**: Implement `DebugToolbarMiddleware` (Pure ASGI)
  - `__init__()` with config and toolbar
  - `__call__()` detecting HTTP scope
  - ResponseState dataclass for tracking
  - ~200 lines

- [ ] **Task 1.4**: Implement send wrapper for response interception
  - `_create_send_wrapper()` method
  - Handle `http.response.start` - capture headers/status
  - Handle `http.response.body` - buffer for injection
  - Inject toolbar HTML at `</body>`
  - Add Server-Timing headers

- [ ] **Task 1.5**: Implement error handling
  - Graceful degradation on errors
  - Buffered response recovery
  - Logging for debugging

### Phase 2: Routes & Panels (Checkpoint 3)

- [ ] **Task 2.1**: Create `routes.py` with API handlers
  - `create_debug_toolbar_router()` factory function
  - `GET /_debug_toolbar/` - History page
  - `GET /_debug_toolbar/{request_id}` - Detail page
  - `GET /_debug_toolbar/api/requests` - JSON list
  - `GET /_debug_toolbar/api/requests/{request_id}` - JSON detail
  - `GET /_debug_toolbar/static/toolbar.css` - CSS
  - `GET /_debug_toolbar/static/toolbar.js` - JavaScript
  - Reuse rendering functions from Litestar handlers
  - ~100 lines

- [ ] **Task 2.2**: Create Starlette-specific `RoutesPanel`
  - `src/debug_toolbar/starlette/panels/routes.py`
  - Collect routes from `app.routes`
  - Handle Route, Mount, WebSocketRoute types
  - ~80 lines

- [ ] **Task 2.3**: Implement route metadata collection in middleware
  - `_populate_routes_metadata()` method
  - Extract path, methods, handler name
  - Detect matched route from scope

### Phase 3: Integration & Examples (Checkpoints 4-6)

- [ ] **Task 3.1**: Create basic example application
  - `examples/starlette_basic/app.py`
  - Homepage, about, users, API status endpoints
  - Demonstrates all core panels
  - ~150 lines

- [ ] **Task 3.2**: Create example README
  - `examples/starlette_basic/README.md`
  - Setup instructions
  - Run commands
  - Feature demonstration

- [ ] **Task 3.3**: Update main README
  - Add Starlette installation section
  - Add usage examples
  - Document configuration options

- [ ] **Task 3.4**: Add Starlette as optional dependency
  - Update `pyproject.toml`
  - `starlette = ["starlette>=0.27.0"]`

### Phase 4: Testing (Checkpoint 7)

- [ ] **Test 1**: Unit tests for middleware
  - `tests/starlette/test_middleware.py`
  - Initialization, request interception, HTML injection
  - Server-Timing headers, error handling
  - ~250 lines

- [ ] **Test 2**: Unit tests for config
  - `tests/starlette/test_config.py`
  - Defaults, `should_show_toolbar()`, exclusions
  - ~150 lines

- [ ] **Test 3**: Unit tests for routes
  - `tests/starlette/test_routes.py`
  - Handler responses, JSON API, static files
  - ~100 lines

- [ ] **Test 4**: Unit tests for routes panel
  - `tests/starlette/panels/test_routes_panel.py`
  - Route collection, stats generation
  - ~100 lines

- [ ] **Test 5**: Create test fixtures
  - `tests/starlette/conftest.py`
  - Starlette app fixture, config fixture, client fixture

### Phase 5: Review & Polish (Checkpoint 8)

- [ ] **Review 1**: Code review against patterns
- [ ] **Review 2**: Type checking with `make type-check`
- [ ] **Review 3**: Linting with `make lint`
- [ ] **Review 4**: Verify all 402+ tests pass
- [ ] **Review 5**: Extract patterns for future framework integrations

---

## Files to Create

1. `src/debug_toolbar/starlette/__init__.py` (~30 lines)
2. `src/debug_toolbar/starlette/config.py` (~85 lines)
3. `src/debug_toolbar/starlette/middleware.py` (~350 lines)
4. `src/debug_toolbar/starlette/routes.py` (~100 lines)
5. `src/debug_toolbar/starlette/panels/__init__.py` (~10 lines)
6. `src/debug_toolbar/starlette/panels/routes.py` (~80 lines)
7. `examples/starlette_basic/app.py` (~150 lines)
8. `examples/starlette_basic/README.md` (~50 lines)

## Files to Modify

1. `pyproject.toml` - Add starlette optional dependency
2. `README.md` - Add Starlette section

---

## Quality Gates

- [ ] 90%+ test coverage for new modules
- [ ] All 402 existing tests still pass
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] < 5ms overhead for excluded routes
