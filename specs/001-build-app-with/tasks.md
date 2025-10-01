# Tasks: Condo Management Web Application

**Input**: Design documents from `/specs/001-build-app-with/`
**Prerequisites**: plan.md (required), research.md, data-model.md, contracts/

## Execution Flow (main)
```
1. Load plan.md from feature directory
   → If not found: ERROR "No implementation plan found"
   → Extract: tech stack, libraries, structure
2. Load optional design documents:
   → data-model.md: Extract entities → model tasks
   → contracts/: Each file → contract test task
   → research.md: Extract decisions → setup tasks
3. Generate tasks by category:
   → Setup: project init, dependencies, linting
   → Tests: contract tests, integration tests
   → Core: models, services, CLI commands
   → Integration: DB, middleware, logging
   → Polish: unit tests, performance, docs
4. Apply task rules:
   → Different files = mark [P] for parallel
   → Same file = sequential (no [P])
   → Tests before implementation (TDD)
5. Number tasks sequentially (T001, T002...)
6. Generate dependency graph
7. Create parallel execution examples
8. Validate task completeness:
   → All contracts have tests?
   → All entities have models?
   → All endpoints implemented?
9. Return: SUCCESS (tasks ready for execution)
```

## Format: `[ID] [P?] Description`
- **[P]**: Can run in parallel (different files, no dependencies)
- Include exact file paths in descriptions

## Path Conventions
- **Web app**: `backend/src/`, `frontend/src/`
- Paths based on plan.md structure with separate frontend and backend

## Phase 3.1: Setup
- [ ] T001 Create project structure per implementation plan
- [ ] T002 Initialize Rust backend project with Cargo.toml and dependencies
- [ ] T003 Initialize frontend structure with HTML/CSS/JS files
- [ ] T004 [P] Configure Docker and docker-compose.yml
- [ ] T005 [P] Configure Rust linting (clippy) and formatting (rustfmt)
- [ ] T006 [P] Configure frontend linting (ESLint) and formatting (Prettier)

## Phase 3.2: Tests First (TDD) ⚠️ MUST COMPLETE BEFORE 3.3
**CRITICAL: These tests MUST be written and MUST FAIL before ANY implementation**

### Contract Tests
- [ ] T007 [P] Contract test POST /residents in backend/tests/contract/test_residents_post.rs
- [ ] T008 [P] Contract test GET /residents in backend/tests/contract/test_residents_get.rs
- [ ] T009 [P] Contract test PUT /residents/{id} in backend/tests/contract/test_residents_put.rs
- [ ] T010 [P] Contract test DELETE /residents/{id} in backend/tests/contract/test_residents_delete.rs
- [ ] T011 [P] Contract test POST /guests in backend/tests/contract/test_guests_post.rs
- [ ] T012 [P] Contract test GET /guests in backend/tests/contract/test_guests_get.rs
- [ ] T013 [P] Contract test PUT /guests/{id} in backend/tests/contract/test_guests_put.rs
- [ ] T014 [P] Contract test DELETE /guests/{id} in backend/tests/contract/test_guests_delete.rs
- [ ] T015 [P] Contract test POST /residents/{id}/vehicles in backend/tests/contract/test_vehicles_post.rs
- [ ] T016 [P] Contract test GET /residents/{id}/vehicles in backend/tests/contract/test_vehicles_get.rs
- [ ] T017 [P] Contract test POST /guests/{id}/vehicles in backend/tests/contract/test_guest_vehicles_post.rs
- [ ] T018 [P] Contract test GET /guests/{id}/vehicles in backend/tests/contract/test_guest_vehicles_get.rs
- [ ] T019 [P] Contract test POST /guests/{id}/permit in backend/tests/contract/test_permits_post.rs
- [ ] T020 [P] Contract test GET /permit/{permit_number}/print in backend/tests/contract/test_permits_print.rs

### Integration Tests
- [ ] T021 [P] Integration test resident management flow in backend/tests/integration/test_resident_flow.rs
- [ ] T022 [P] Integration test guest registration flow in backend/tests/integration/test_guest_flow.rs
- [ ] T023 [P] Integration test vehicle management flow in backend/tests/integration/test_vehicle_flow.rs
- [ ] T024 [P] Integration test parking permit generation in backend/tests/integration/test_permit_flow.rs
- [ ] T025 [P] Frontend E2E test resident management in frontend/tests/e2e/test_residents.spec.js
- [ ] T026 [P] Frontend E2E test guest registration in frontend/tests/e2e/test_guests.spec.js
- [ ] T027 [P] Frontend E2E test permit generation in frontend/tests/e2e/test_permits.spec.js

## Phase 3.3: Core Implementation (ONLY after tests are failing)

### Database Models
- [ ] T028 [P] Resident model in backend/src/models/resident.rs
- [ ] T029 [P] Vehicle model in backend/src/models/vehicle.rs
- [ ] T030 [P] Guest model in backend/src/models/guest.rs
- [ ] T031 [P] GuestVehicle model in backend/src/models/guest_vehicle.rs
- [ ] T032 [P] ParkingPermit model in backend/src/models/parking_permit.rs
- [ ] T033 [P] Database connection and migration setup in backend/src/database/mod.rs
- [ ] T034 [P] SQLite schema creation in backend/migrations/001_initial_schema.sql

### Services
- [ ] T035 [P] ResidentService CRUD in backend/src/services/resident_service.rs
- [ ] T036 [P] GuestService CRUD in backend/src/services/guest_service.rs
- [ ] T037 [P] VehicleService CRUD in backend/src/services/vehicle_service.rs
- [ ] T038 [P] PermitService generation in backend/src/services/permit_service.rs

### API Handlers
- [ ] T039 [P] Resident handlers in backend/src/handlers/residents.rs
- [ ] T040 [P] Guest handlers in backend/src/handlers/guests.rs
- [ ] T041 [P] Vehicle handlers in backend/src/handlers/vehicles.rs
- [ ] T042 [P] Permit handlers in backend/src/handlers/permits.rs

### Frontend Components
- [ ] T043 [P] Main application structure in frontend/src/index.html
- [ ] T044 [P] CSS styles in frontend/src/css/style.css
- [ ] T045 [P] Resident management JavaScript in frontend/src/js/residents.js
- [ ] T046 [P] Guest management JavaScript in frontend/src/js/guests.js
- [ ] T047 [P] Vehicle management JavaScript in frontend/src/js/vehicles.js
- [ ] T048 [P] Permit generation JavaScript in frontend/src/js/permits.js
- [ ] T049 [P] Main application JavaScript in frontend/src/js/app.js

### API Integration
- [ ] T050 Connect ResidentService to database
- [ ] T051 Connect GuestService to database
- [ ] T052 Connect VehicleService to database
- [ ] T053 Connect PermitService to database
- [ ] T054 Implement input validation for all endpoints
- [ ] T055 Implement error handling and logging
- [ ] T056 Set up Axum web server with routes

## Phase 3.4: Integration
- [ ] T057 Connect all services to database
- [ ] T058 Set up request/response logging middleware
- [ ] T059 Configure CORS and security headers
- [ ] T060 Set up static file serving for frontend
- [ ] T061 Configure HTML template rendering with Askama
- [ ] T062 Set up print-friendly CSS for parking permits
- [ ] T063 Configure Docker multi-stage builds
- [ ] T064 Set up docker-compose for development

## Phase 3.5: Polish
- [ ] T065 [P] Unit tests for validation in backend/tests/unit/test_validation.rs
- [ ] T066 [P] Unit tests for services in backend/tests/unit/test_services.rs
- [ ] T067 [P] Frontend unit tests in frontend/tests/unit/test_components.js
- [ ] T068 Performance tests (<200ms response time)
- [ ] T069 [P] Update API documentation
- [ ] T070 [P] Update quickstart guide
- [ ] T071 Remove code duplication
- [ ] T072 Run manual testing scenarios
- [ ] T073 Optimize Docker image size
- [ ] T074 Add health check endpoints

## Dependencies
- Tests (T007-T027) before implementation (T028-T056)
- T028-T032 (models) before T035-T038 (services)
- T035-T038 (services) before T039-T042 (handlers)
- T033-T034 (database) before T050-T056 (API integration)
- T043-T049 (frontend) can run in parallel with backend
- T057-T064 (integration) after core implementation
- T065-T074 (polish) after integration

## Parallel Execution Examples

### Phase 3.2: Contract Tests (T007-T020)
```bash
# Launch all contract tests together:
Task: "Contract test POST /residents in backend/tests/contract/test_residents_post.rs"
Task: "Contract test GET /residents in backend/tests/contract/test_residents_get.rs"
Task: "Contract test PUT /residents/{id} in backend/tests/contract/test_residents_put.rs"
Task: "Contract test DELETE /residents/{id} in backend/tests/contract/test_residents_delete.rs"
Task: "Contract test POST /guests in backend/tests/contract/test_guests_post.rs"
Task: "Contract test GET /guests in backend/tests/contract/test_guests_get.rs"
# ... continue with all contract tests
```

### Phase 3.3: Models (T028-T032)
```bash
# Launch all model creation together:
Task: "Resident model in backend/src/models/resident.rs"
Task: "Vehicle model in backend/src/models/vehicle.rs"
Task: "Guest model in backend/src/models/guest.rs"
Task: "GuestVehicle model in backend/src/models/guest_vehicle.rs"
Task: "ParkingPermit model in backend/src/models/parking_permit.rs"
```

### Phase 3.3: Services (T035-T038)
```bash
# Launch all services together:
Task: "ResidentService CRUD in backend/src/services/resident_service.rs"
Task: "GuestService CRUD in backend/src/services/guest_service.rs"
Task: "VehicleService CRUD in backend/src/services/vehicle_service.rs"
Task: "PermitService generation in backend/src/services/permit_service.rs"
```

### Phase 3.3: Frontend Components (T043-T049)
```bash
# Launch all frontend components together:
Task: "Main application structure in frontend/src/index.html"
Task: "CSS styles in frontend/src/css/style.css"
Task: "Resident management JavaScript in frontend/src/js/residents.js"
Task: "Guest management JavaScript in frontend/src/js/guests.js"
Task: "Vehicle management JavaScript in frontend/src/js/vehicles.js"
Task: "Permit generation JavaScript in frontend/src/js/permits.js"
Task: "Main application JavaScript in frontend/src/js/app.js"
```

## Notes
- [P] tasks = different files, no dependencies
- Verify tests fail before implementing
- Commit after each task
- Avoid: vague tasks, same file conflicts
- Follow TDD: write failing tests first, then implement to make them pass

## Task Generation Rules
*Applied during main() execution*

1. **From Contracts**:
   - Each contract file → contract test task [P]
   - Each endpoint → implementation task
   
2. **From Data Model**:
   - Each entity → model creation task [P]
   - Relationships → service layer tasks
   
3. **From User Stories**:
   - Each story → integration test [P]
   - Quickstart scenarios → validation tasks

4. **Ordering**:
   - Setup → Tests → Models → Services → Endpoints → Polish
   - Dependencies block parallel execution

## Validation Checklist
*GATE: Checked by main() before returning*

- [x] All contracts have corresponding tests
- [x] All entities have model tasks
- [x] All tests come before implementation
- [x] Parallel tasks truly independent
- [x] Each task specifies exact file path
- [x] No task modifies same file as another [P] task
