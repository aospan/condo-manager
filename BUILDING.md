# Building the Condo Management System

This guide explains how to build the Condo Management System from the specifications in this repository.

## Quick Start (Cursor IDE)

If you're using Cursor IDE with the Specify framework:

```bash
# 1. View available features
ls specs/

# 2. Choose a feature (e.g., 001-build-app-with)
# 3. In Cursor IDE, run:
/plan

# 4. Review the generated plan, then run:
/implement

# 5. The application will be built automatically!
```

## Manual Build Process

### Feature 001: Main Application

1. **Review the specification**:
   ```bash
   cat specs/001-build-app-with/spec.md
   ```

2. **Study the implementation plan**:
   ```bash
   cat specs/001-build-app-with/plan.md
   cat specs/001-build-app-with/data-model.md
   cat specs/001-build-app-with/contracts/openapi.yaml
   ```

3. **Follow the tasks**:
   ```bash
   cat specs/001-build-app-with/tasks.md
   ```
   
   The tasks are numbered (T001-T074) and include:
   - Setup (T001-T006): Create project structure, dependencies
   - Tests (T007-T027): Write tests FIRST (TDD)
   - Implementation (T028-T056): Implement to make tests pass
   - Integration (T057-T064): Connect components
   - Polish (T065-T074): Documentation, performance, validation

4. **Validate the implementation**:
   ```bash
   # Follow quickstart scenarios
   cat specs/001-build-app-with/quickstart.md
   ```

### Feature 002: Status Notifications

After implementing Feature 001, implement Feature 002:

1. **Review the specification**:
   ```bash
   cat specs/002-do-not-show/spec.md
   ```

2. **Follow the tasks**:
   ```bash
   cat specs/002-do-not-show/tasks.md
   ```

3. **Validate**:
   ```bash
   cat specs/002-do-not-show/quickstart.md
   ```

## Expected Project Structure After Build

```
condo-manager/
├── .specify/                    # Specify framework (committed)
├── .cursor/                     # Cursor commands (committed)
├── specs/                       # Feature specs (committed)
│   ├── 001-build-app-with/
│   └── 002-do-not-show/
├── backend/                     # Generated from specs
│   ├── src/
│   │   ├── main.rs
│   │   ├── models/
│   │   ├── services/
│   │   ├── handlers/
│   │   └── database/
│   ├── tests/
│   ├── migrations/
│   └── Cargo.toml
├── frontend/                    # Generated from specs
│   ├── src/
│   │   ├── index.html
│   │   ├── css/
│   │   └── js/
│   ├── tests/
│   └── package.json
├── docker-compose.yml           # Generated from specs
├── Dockerfile.backend           # Generated from specs
├── .gitignore                   # Committed
└── README.md                    # Committed
```

## Tech Stack (from specs)

- **Backend**: Rust 1.90, Axum web framework, SQLx for database
- **Database**: SQLite with migrations
- **Frontend**: HTML5, CSS3, JavaScript ES6+
- **Testing**: Cargo test (Rust), Jest (JavaScript), Playwright (E2E)
- **Container**: Docker with multi-stage builds

## TDD Approach

The specifications follow Test-Driven Development:

1. **Red**: Write tests that fail
2. **Green**: Implement code to make tests pass
3. **Refactor**: Clean up code while keeping tests green

Each feature's `tasks.md` explicitly separates:
- Phase 3.2: Tests First (must fail before implementation)
- Phase 3.3: Core Implementation (make tests pass)

## Validation Checklist

After building, verify:

- [ ] All tests pass (`cargo test` and `npm test`)
- [ ] Application runs (`cargo run` or `docker-compose up`)
- [ ] Health check responds: `curl http://localhost:3000/health`
- [ ] All quickstart scenarios work
- [ ] No linter warnings
- [ ] Database migrations applied successfully

## Estimated Build Time

- **Feature 001** (Main Application): 40-50 hours
  - Setup: 2-3 hours
  - Tests: 8-12 hours
  - Implementation: 20-25 hours
  - Integration: 6-8 hours
  - Polish: 4-6 hours

- **Feature 002** (Status Notifications): 23-32 hours
  - Setup: 1-2 hours
  - Tests: 4-6 hours
  - Implementation: 8-10 hours
  - Integration: 6-8 hours
  - Polish: 4-6 hours

**Total**: 63-82 hours (depending on experience level)

With AI assistance (`/implement`): 10-20% of manual time

## Troubleshooting

### Common Issues

**Issue**: Tests failing during Phase 3.2
- **Expected**: Tests SHOULD fail before implementation
- **Action**: Continue to Phase 3.3 and implement the code

**Issue**: Database connection errors
- **Solution**: Ensure `backend/data/` directory exists and is writable

**Issue**: Static files not loading (404 errors)
- **Solution**: Verify paths in `backend/src/main.rs` match `frontend/src/`

**Issue**: Frontend date format errors
- **Solution**: Convert HTML datetime-local format to ISO 8601 with timezone

See `specs/001-build-app-with/spec.md` section "Implementation Issues Discovered and Resolved" for detailed troubleshooting.

## Support

For questions about:
- **Specifications**: Review the spec.md files
- **Architecture**: Review the plan.md and data-model.md files
- **Implementation**: Review the tasks.md files
- **Testing**: Review the quickstart.md files

## Next Steps

1. Start with Feature 001 to build the main application
2. Then add Feature 002 for better UX
3. Create your own features using `/specify`

Happy building! 🚀

