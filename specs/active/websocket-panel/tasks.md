# Task Breakdown: WebSocket Panel

**Complexity**: High (12 checkpoints)
**PRD**: `specs/active/websocket-panel/prd.md`

---

## Implementation Tasks

### Phase 1: Data Models (Checkpoint 1)

- [ ] **Task 1.1**: Create `WebSocketMessage` dataclass
  - Fields: direction, message_type, content, timestamp, size_bytes, truncated
  - Methods: `get_content_preview()`, `get_binary_preview_hex()`
  - ~60 lines

- [ ] **Task 1.2**: Create `WebSocketConnection` dataclass
  - Fields: connection_id, path, query_string, headers, timestamps, state, counters
  - Methods: `add_message()`, `get_duration()`, `get_short_id()`
  - Circular buffer for messages
  - ~100 lines

### Phase 2: WebSocketPanel Core (Checkpoints 2-3)

- [ ] **Task 2.1**: Create `WebSocketPanel` class skeleton
  - ClassVar metadata (panel_id, title, template)
  - Class-level `_active_connections` dict
  - Thread-safe `_connections_lock`
  - ~100 lines

- [ ] **Task 2.2**: Implement connection tracking methods
  - `track_connection()` - add to active tracking
  - `untrack_connection()` - remove from tracking
  - `get_connection()` - retrieve by ID
  - `_cleanup_old_connections()` - enforce limits

- [ ] **Task 2.3**: Implement `generate_stats()` method
  - Active connections list
  - Recent connections list
  - Aggregate message/byte counts
  - Recent messages across all connections

- [ ] **Task 2.4**: Implement `get_nav_subtitle()` method
  - Return active connection count

### Phase 3: Middleware Integration (Checkpoints 4-7)

- [ ] **Task 3.1**: Add WebSocket detection to middleware `__call__()`
  - Check `scope["type"] == "websocket"`
  - Route to `_handle_websocket()` method

- [ ] **Task 3.2**: Implement `_handle_websocket()` method
  - Create `WebSocketConnection` from scope
  - Track connection in panel
  - Wrap send/receive callables
  - Cleanup in finally block

- [ ] **Task 3.3**: Implement `_create_websocket_send_wrapper()`
  - Handle `websocket.accept` - update state to connected
  - Handle `websocket.send` - log outgoing messages
  - Handle `websocket.close` - capture close code/reason
  - Apply truncation for large messages

- [ ] **Task 3.4**: Implement `_create_websocket_receive_wrapper()`
  - Handle `websocket.receive` - log incoming messages
  - Handle `websocket.disconnect` - update state
  - Apply same truncation rules

- [ ] **Task 3.5**: Add graceful error handling
  - Try/except in wrappers
  - Log errors but don't break WebSocket
  - Always forward to original send/receive

### Phase 4: Configuration (Checkpoint 8)

- [ ] **Config 1**: Add WebSocket config options to `DebugToolbarConfig`
  - `websocket_tracking_enabled: bool = True`
  - `websocket_max_connections: int = 50`
  - `websocket_max_messages_per_connection: int = 100`
  - `websocket_max_message_size: int = 10240`
  - `websocket_binary_preview_size: int = 256`
  - `websocket_show_binary_preview: bool = False`
  - `websocket_connection_ttl: int = 3600`

### Phase 5: UI Template (Checkpoint 10)

- [ ] **Task 5.1**: Create `templates/panels/websocket.html`
  - Active connections table
  - Recent connections section
  - Message timeline with direction indicators
  - Statistics section
  - ~100+ lines

- [ ] **Task 5.2**: Add template filters
  - `format_duration` - seconds to "1m 23s"
  - `format_bytes` - bytes to "1.2 KB"
  - `format_timestamp` - relative/absolute time

---

## Testing Tasks

- [ ] **Test 1**: Unit tests for `WebSocketMessage` and `WebSocketConnection`
  - Create, add messages, buffer limits, serialization
  - ~200 lines

- [ ] **Test 2**: Unit tests for `WebSocketPanel`
  - Tracking, stats generation, nav subtitle
  - ~300 lines

- [ ] **Test 3**: Integration tests with Litestar WebSocket handlers
  - Connection lifecycle, message tracking, error handling
  - ~400 lines

- [ ] **Test 4**: Performance benchmarks
  - Message logging overhead
  - Memory usage
  - ~100 lines

---

## Files to Create

1. `src/debug_toolbar/core/panels/websocket.py` (~350 lines)
2. `templates/panels/websocket.html` (~100 lines)
3. `tests/core/panels/test_websocket.py` (~500 lines)
4. `tests/litestar/test_websocket_integration.py` (~400 lines)
5. `tests/performance/test_websocket_performance.py` (~100 lines)

## Files to Modify

1. `src/debug_toolbar/core/config.py` - Add WebSocket config fields
2. `src/debug_toolbar/litestar/middleware.py` - Add WebSocket handling

---

## Quality Gates

- [ ] 90%+ test coverage for new code
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] No anti-patterns
- [ ] < 5% performance overhead on WebSocket throughput
