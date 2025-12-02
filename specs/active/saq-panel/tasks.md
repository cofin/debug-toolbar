# Task Breakdown: SAQ Background Tasks Panel

**Complexity**: Medium (8 checkpoints)
**PRD**: `specs/active/saq-panel/prd.md`

---

## Implementation Tasks

### Phase 1: Core Implementation (Checkpoints 1-4)

- [ ] **Task 1.1**: Create `TrackedTask` dataclass
  - Fields: job_id, function_name, kwargs, timestamps, config, stack
  - Pattern hash for N+1 detection
  - ~30 lines

- [ ] **Task 1.2**: Implement `TaskTracker` class
  - `start()` / `stop()` lifecycle methods
  - `track_job()` to record tasks
  - `_serialize_kwargs()` for safe display
  - `_capture_stack()` with library filtering
  - ~150 lines

- [ ] **Task 1.3**: Implement Queue.enqueue() wrapping
  - `_setup_queue_wrapping()` function
  - `_wrapped_enqueue()` async wrapper
  - `_remove_queue_wrapping()` for cleanup
  - Preserve original method reference
  - ~100 lines

- [ ] **Task 1.4**: Create `SAQPanel` class
  - ClassVar metadata (panel_id, title, template)
  - `process_request()` starts tracker
  - `process_response()` stops tracker
  - `__init__` applies wrapping
  - ~100 lines

- [ ] **Task 1.5**: Implement `generate_stats()` method
  - Task list conversion
  - Summary statistics (counts, timing)
  - Group by function name
  - N+1 detection
  - ~100 lines

- [ ] **Task 1.6**: Implement N+1 detection
  - `_detect_n_plus_one()` method
  - Group by pattern hash + origin
  - Threshold: 3+ same tasks from same location
  - Generate fix suggestions
  - ~60 lines

- [ ] **Task 1.7**: Implement Server-Timing contribution
  - `generate_server_timing()` method
  - Total enqueue time in seconds
  - ~10 lines

### Phase 2: Testing (Checkpoints 5-6)

- [ ] **Test 1**: Unit tests for `TaskTracker`
  - Start/stop, track_job, serialization, stack capture
  - `tests/unit/test_saq_panel.py` TestTaskTracker class
  - ~150 lines

- [ ] **Test 2**: Unit tests for Queue wrapping
  - Setup/remove, wrapped enqueue behavior
  - TestQueueWrapping class
  - ~100 lines

- [ ] **Test 3**: Unit tests for `SAQPanel`
  - Metadata, lifecycle, generate_stats, N+1 detection
  - TestSAQPanel class
  - ~200 lines

- [ ] **Test 4**: Integration tests
  - `tests/integration/test_saq_integration.py`
  - Real/mock Queue instances
  - Full lifecycle test
  - ~150 lines

- [ ] **Test 5**: Create test fixtures
  - `mock_job` fixture
  - `task_tracker` fixture with cleanup
  - `saq_panel` fixture

### Phase 3: Optional Enhancements (Checkpoint 7)

- [ ] **Optional 1**: Status refresh endpoint
  - `GET /api/saq/job/<job_id>/status`
  - Query Redis via `job.refresh()`
  - Return JSON status
  - ~50 lines

### Phase 4: Documentation (Checkpoint 8)

- [ ] **Doc 1**: User documentation
  - `docs/panels/saq.md`
  - Installation, configuration, usage, troubleshooting

- [ ] **Doc 2**: Pattern extraction
  - `specs/active/saq-panel/tmp/new-patterns.md`
  - Tracker pattern for external libraries
  - Monkey patching best practices
  - Call stack capture

---

## Files to Create

1. `src/debug_toolbar/extras/saq/__init__.py` (~20 lines)
2. `src/debug_toolbar/extras/saq/tracker.py` (~300 lines)
3. `src/debug_toolbar/extras/saq/panel.py` (~300 lines)
4. `tests/unit/test_saq_panel.py` (~500 lines)
5. `tests/integration/test_saq_integration.py` (~150 lines)

---

## Quality Gates

- [ ] 90%+ test coverage for new code
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] Per-task overhead < 1ms
- [ ] Zero-config integration (works automatically)
