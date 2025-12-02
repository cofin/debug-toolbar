# Task Breakdown: Async Profiling Panel

**Complexity**: Complex (12 checkpoints)
**PRD**: `specs/active/async-profiler/prd.md`

---

## Implementation Tasks

### Phase 1: Foundation (Checkpoints 1-4)

- [ ] **Task 1.1**: Create `AsyncProfilerBackend` ABC in `src/debug_toolbar/core/panels/async_profiling/base.py`
  - Define abstract methods: `start()`, `stop()`, `get_stats()`, `is_available()`
  - Add type hints and docstrings
  - ~60 lines

- [ ] **Task 1.2**: Implement `EventLoopMonitorBackend` in `eventloop.py`
  - Basic event loop lag detection using `asyncio.sleep()`
  - Task counting via `asyncio.all_tasks()`
  - Always available (no dependencies)
  - ~150 lines

- [ ] **Task 1.3**: Create `AsyncProfilingPanel` skeleton in `panel.py`
  - ClassVar metadata (panel_id, title, template)
  - Backend selection logic (`_select_backend()`)
  - Configuration options from `DebugToolbarConfig`
  - ~200 lines

- [ ] **Task 1.4**: Implement panel lifecycle integration
  - `process_request()` calls `backend.start()`
  - `process_response()` calls `backend.stop()`
  - `generate_stats()` returns backend stats
  - Error handling with graceful degradation

### Phase 2: Yappi Integration (Checkpoints 5-7)

- [ ] **Task 2.1**: Implement `YappiBackend` in `yappi.py`
  - Configure yappi for wall time profiling
  - Extract function statistics
  - Identify blocking calls > threshold
  - ~250 lines

- [ ] **Task 2.2**: Implement blocking call detection
  - Analyze function durations from yappi stats
  - Filter synchronous calls in async context
  - Classify impact (warning/critical)

- [ ] **Task 2.3**: Implement top async functions extraction
  - Sort by cumulative time
  - Filter to coroutines only
  - Return top 50

### Phase 3: Advanced Features (Checkpoints 8-10)

- [ ] **Task 3.1**: Implement `TaskTracker` utility in `task_tracker.py`
  - Monkey patch `asyncio.create_task()`
  - Track parent-child relationships
  - Use weak references for memory safety
  - ~200 lines

- [ ] **Task 3.2**: Build task hierarchy construction
  - `get_hierarchy()` method returns tree structure
  - Handle nested task creation (3+ levels)
  - Limit tracking to `max_tasks` (default: 100)

- [ ] **Task 3.3**: Implement await point analysis (if possible)
  - Correlate coroutine yields with functions
  - Aggregate await counts per function
  - Calculate total/average wait times

### Phase 4: UI & Polish (Checkpoints 11-12)

- [ ] **Task 4.1**: Create HTML template `panels/async_profiling.html`
  - Summary section with metrics
  - Blocking calls table
  - Task hierarchy tree
  - Timeline visualization (JavaScript)
  - ~400 lines

- [ ] **Task 4.2**: Implement navigation subtitle
  - Show blocking count and lag
  - Format: "3 blocking, 5ms lag" or "OK"

- [ ] **Task 4.3**: Add Server-Timing header integration
  - `generate_server_timing()` returns timing data
  - Include profiling overhead and event loop lag

---

## Configuration Tasks

- [ ] **Config 1**: Add async profiler options to `DebugToolbarConfig`
  - `async_profiler_backend: Literal["yappi", "monitoring", "eventloop", "auto"]`
  - `async_profiler_track_tasks: bool`
  - `async_profiler_detect_blocking: bool`
  - `async_profiler_blocking_threshold: float`
  - `async_profiler_max_tasks: int`

- [ ] **Config 2**: Add optional dependency to `pyproject.toml`
  - `async-profiling = ["yappi>=1.4.0"]`

---

## Testing Tasks

- [ ] **Test 1**: Unit tests for `AsyncProfilingPanel` (~500 lines)
  - Initialization, backend selection, lifecycle, stats, Server-Timing, nav subtitle

- [ ] **Test 2**: Unit tests for `YappiBackend` (~300 lines)
  - Skip if yappi not installed

- [ ] **Test 3**: Unit tests for `EventLoopMonitorBackend` (~200 lines)

- [ ] **Test 4**: Unit tests for `TaskTracker` (~300 lines)

- [ ] **Test 5**: Integration tests (~400 lines)
  - Real async request profiling
  - Blocking call detection
  - Task hierarchy construction

---

## Documentation Tasks

- [ ] **Doc 1**: API documentation (docstrings)
- [ ] **Doc 2**: User guide (installation, configuration, interpreting results)
- [ ] **Doc 3**: Example applications

---

## Quality Gates

- [ ] 90%+ test coverage for new code
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] No anti-patterns (PEP 604, future annotations)
