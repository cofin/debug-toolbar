# Task Breakdown: MCP Server Integration

**Complexity**: High
**PRD**: `specs/active/mcp-server/prd.md`

---

## Implementation Tasks

### Phase 1: MCP Server Core

- [ ] **Task 1.1**: Create `DebugToolbarMCPServer` class
  - `src/debug_toolbar/mcp/server.py`
  - Initialize with `ToolbarStorage`
  - Register tools and resources
  - ~100 lines

- [ ] **Task 1.2**: Implement tool handlers
  - `get_request_history(limit)` - Recent requests
  - `get_slow_queries(request_id, threshold_ms)` - Slow SQL queries
  - `get_n_plus_one_issues(request_id)` - N+1 patterns
  - `get_performance_summary(request_id)` - Timing breakdown
  - `get_error_context(request_id)` - Full error info
  - `explain_query(request_id, query_index)` - EXPLAIN plan
  - `get_memory_allocations(request_id, limit)` - Top allocations
  - ~200 lines

- [ ] **Task 1.3**: Implement resource providers
  - `debug://current-request` - Latest request data
  - `debug://alerts` - Current alerts
  - `debug://request/{id}` - Specific request
  - `debug://request/{id}/queries` - SQL queries
  - `debug://request/{id}/profiling` - Profile data
  - ~100 lines

### Phase 2: Transport & CLI

- [ ] **Task 2.1**: Implement stdio transport (for Claude Code)
  - `run_stdio()` async function
  - Standard input/output communication
  - ~30 lines

- [ ] **Task 2.2**: Implement HTTP/SSE transport (for Cursor)
  - `run_http(port)` async function
  - SSE server transport
  - ~50 lines

- [ ] **Task 2.3**: Create CLI interface
  - `src/debug_toolbar/mcp/cli.py`
  - `python -m debug_toolbar.mcp.cli`
  - Arguments: `--transport`, `--port`, `--storage`
  - ~80 lines

### Phase 3: Testing

- [ ] **Test 1**: Unit tests for MCP server
  - Tool responses, resource URIs
  - ~200 lines

- [ ] **Test 2**: Unit tests for tools
  - Each tool handler
  - ~200 lines

- [ ] **Test 3**: Integration tests
  - Stdio transport, HTTP transport
  - With running debug toolbar
  - ~150 lines

### Phase 4: Documentation

- [ ] **Doc 1**: Claude Code configuration
  - `.claude/settings.json` example
  - Usage instructions

- [ ] **Doc 2**: Cursor configuration
  - `.cursor/mcp.json` example
  - Usage instructions

- [ ] **Doc 3**: Integration guide
  - `docs/integrations/mcp.md`

---

## Files to Create

1. `src/debug_toolbar/mcp/__init__.py` (~10 lines)
2. `src/debug_toolbar/mcp/server.py` (~200 lines)
3. `src/debug_toolbar/mcp/tools.py` (~300 lines)
4. `src/debug_toolbar/mcp/resources.py` (~100 lines)
5. `src/debug_toolbar/mcp/cli.py` (~80 lines)
6. `tests/unit/test_mcp_server.py` (~400 lines)
7. `docs/integrations/mcp.md` (~100 lines)
8. `docs/integrations/claude-code.md` (~50 lines)
9. `docs/integrations/cursor.md` (~50 lines)
10. `examples/mcp_integration.py` (~50 lines)

---

## Acceptance Criteria

- [ ] MCP server implements required protocol
- [ ] All tools return valid JSON
- [ ] Resources accessible via URI
- [ ] Works with Claude Code
- [ ] Works with Cursor
- [ ] Standalone CLI mode for testing
- [ ] HTTP transport option
- [ ] Documentation for integration
- [ ] 90%+ test coverage

---

## Quality Gates

- [ ] 90%+ test coverage
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] Pin MCP protocol version
- [ ] Read-only access (no write operations)
