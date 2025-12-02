# Task Breakdown: OpenTelemetry Integration

**Complexity**: High
**PRD**: `specs/active/opentelemetry/prd.md`

---

## Implementation Tasks

### Phase 1: OTLP Exporter

- [ ] **Task 1.1**: Create `OTLPExporter` class
  - `src/debug_toolbar/otel/exporter.py`
  - Convert panel data to OTLP spans
  - Export to configured endpoint
  - ~150 lines

- [ ] **Task 1.2**: Implement span conversion
  - Request → Root span
  - Panel timings → Child spans
  - Database queries → DB spans
  - ~100 lines

### Phase 2: Span Processor

- [ ] **Task 2.1**: Create custom span processor
  - `src/debug_toolbar/otel/span_processor.py`
  - Capture spans during request
  - Store for panel display
  - ~80 lines

- [ ] **Task 2.2**: Implement trace context propagation
  - Extract trace context from headers
  - Propagate to child spans
  - ~50 lines

### Phase 3: OpenTelemetry Panel

- [ ] **Task 3.1**: Create `OpenTelemetryPanel` class
  - `src/debug_toolbar/otel/panel.py`
  - Visualize spans from current request
  - Show trace waterfall
  - Link to external trace viewer
  - ~150 lines

- [ ] **Task 3.2**: Create panel template
  - Span timeline visualization
  - Waterfall view
  - External link generation
  - ~100 lines

### Phase 4: Testing

- [ ] **Test 1**: Unit tests for exporter
  - Span conversion, OTLP format
  - ~150 lines

- [ ] **Test 2**: Unit tests for panel
  - Stats generation, visualization
  - ~100 lines

- [ ] **Test 3**: Integration tests
  - With Jaeger/Zipkin (if available)
  - ~100 lines

---

## Files to Create

1. `src/debug_toolbar/otel/__init__.py` (~10 lines)
2. `src/debug_toolbar/otel/exporter.py` (~150 lines)
3. `src/debug_toolbar/otel/span_processor.py` (~80 lines)
4. `src/debug_toolbar/otel/panel.py` (~150 lines)
5. `templates/panels/otel.html` (~100 lines)
6. `tests/unit/test_otel.py` (~350 lines)

---

## Acceptance Criteria

- [ ] OTLP exporter works with Jaeger
- [ ] OTLP exporter works with Zipkin
- [ ] Trace context propagated
- [ ] Span visualization in panel
- [ ] Trace waterfall view
- [ ] Link to external trace viewer
- [ ] 90%+ test coverage

---

## Quality Gates

- [ ] 90%+ test coverage
- [ ] `make lint` passes
- [ ] `make type-check` passes
- [ ] Works with OpenTelemetry SDK
