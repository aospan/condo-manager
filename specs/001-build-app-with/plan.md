# Implementation Plan: Condo Management Web Application

**Branch**: `001-build-app-with` | **Date**: 2024-12-19 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/001-build-app-with/spec.md`

## Execution Flow (/plan command scope)
```
1. Load feature spec from Input path
   → If not found: ERROR "No feature spec at {path}"
2. Fill Technical Context (scan for NEEDS CLARIFICATION)
   → Detect Project Type from file system structure or context (web=frontend+backend, mobile=app+api)
   → Set Structure Decision based on project type
3. Fill the Constitution Check section based on the content of the constitution document.
4. Evaluate Constitution Check section below
   → If violations exist: Document in Complexity Tracking
   → If no justification possible: ERROR "Simplify approach first"
   → Update Progress Tracking: Initial Constitution Check
5. Execute Phase 0 → research.md
   → If NEEDS CLARIFICATION remain: ERROR "Resolve unknowns"
6. Execute Phase 1 → contracts, data-model.md, quickstart.md, agent-specific template file (e.g., `CLAUDE.md` for Claude Code, `.github/copilot-instructions.md` for GitHub Copilot, `GEMINI.md` for Gemini CLI, `QWEN.md` for Qwen Code or `AGENTS.md` for opencode).
7. Re-evaluate Constitution Check section
   → If new violations: Refactor design, return to Phase 1
   → Update Progress Tracking: Post-Design Constitution Check
8. Plan Phase 2 → Describe task generation approach (DO NOT create tasks.md)
9. STOP - Ready for /tasks command
```

**IMPORTANT**: The /plan command STOPS at step 7. Phases 2-4 are executed by other commands:
- Phase 2: /tasks command creates tasks.md
- Phase 3-4: Implementation execution (manual or via tools)

## Summary
Build a web-based condo management application that allows managing resident information (name, address, vehicles) and guest registrations with parking permit generation. The system uses a Rust backend with SQLite database, HTML/CSS/JavaScript frontend, and Docker containerization for deployment.

## Technical Context
**Language/Version**: Rust 1.75+ (backend), HTML5/CSS3/ES2020 (frontend)  
**Primary Dependencies**: Axum (web framework), SQLx (database), Askama (templates), Docker  
**Storage**: SQLite database with local file storage  
**Testing**: cargo test (Rust), Jest (JavaScript), Playwright (E2E)  
**Target Platform**: Linux server with Docker, modern web browsers  
**Project Type**: web (frontend + backend)  
**Performance Goals**: <200ms response time, support 100+ concurrent users  
**Constraints**: No authentication required, offline-capable frontend, printable permits  
**Scale/Scope**: Single condo building, ~500 residents, unlimited guests  

## Constitution Check
*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- [x] **Single Responsibility**: Each component has one clear purpose
- [x] **Simple Architecture**: Web app with clear frontend/backend separation
- [x] **Minimal Dependencies**: Using standard, well-maintained crates
- [x] **Testable Design**: Clear separation of concerns enables unit testing
- [x] **No Over-Engineering**: Simple CRUD operations with basic web UI

## Project Structure

### Documentation (this feature)
```
specs/001-build-app-with/
├── plan.md              # This file (/plan command output)
├── research.md          # Phase 0 output (/plan command)
├── data-model.md        # Phase 1 output (/plan command)
├── quickstart.md        # Phase 1 output (/plan command)
├── contracts/           # Phase 1 output (/plan command)
└── tasks.md             # Phase 2 output (/tasks command - NOT created by /plan)
```

### Source Code (repository root)
```
backend/
├── src/
│   ├── main.rs          # Application entry point
│   ├── models/          # Database models
│   │   ├── mod.rs
│   │   ├── resident.rs
│   │   ├── guest.rs
│   │   └── vehicle.rs
│   ├── handlers/        # HTTP request handlers
│   │   ├── mod.rs
│   │   ├── residents.rs
│   │   ├── guests.rs
│   │   └── permits.rs
│   ├── database/        # Database connection and migrations
│   │   ├── mod.rs
│   │   └── migrations/
│   ├── templates/       # HTML templates
│   │   ├── base.html
│   │   ├── residents.html
│   │   ├── guests.html
│   │   └── permits.html
│   └── static/          # Static assets (CSS, JS)
│       ├── style.css
│       └── app.js
├── tests/
│   ├── integration/
│   └── unit/
├── Cargo.toml
├── Dockerfile
└── migrations.sql

frontend/
├── src/
│   ├── index.html       # Main HTML file
│   ├── css/
│   │   └── style.css    # Main stylesheet
│   ├── js/
│   │   ├── app.js       # Main application logic
│   │   ├── residents.js # Resident management
│   │   ├── guests.js    # Guest management
│   │   └── permits.js   # Permit generation
│   └── assets/
│       └── images/
├── tests/
│   └── e2e/
└── package.json

docker-compose.yml       # Development environment
Dockerfile.backend       # Backend container
Dockerfile.frontend      # Frontend container
```

**Structure Decision**: Web application with separate frontend and backend directories. Backend serves both API endpoints and static files. Frontend is vanilla HTML/CSS/JS for simplicity and performance.

## Phase 0: Outline & Research
1. **Extract unknowns from Technical Context** above:
   - Rust web framework comparison (Axum vs Actix vs Warp)
   - SQLite with Rust best practices and connection pooling
   - HTML template engine selection (Askama vs Tera)
   - Frontend state management patterns for vanilla JS
   - Docker multi-stage builds for Rust applications
   - Print-friendly CSS techniques for parking permits

2. **Generate and dispatch research agents**:
   ```
   Task: "Research Rust web frameworks for simple CRUD applications"
   Task: "Find best practices for SQLite with Rust and SQLx"
   Task: "Research HTML template engines for Rust web applications"
   Task: "Find patterns for vanilla JavaScript state management"
   Task: "Research Docker optimization for Rust web applications"
   Task: "Find CSS techniques for print-friendly documents"
   ```

3. **Consolidate findings** in `research.md` using format:
   - Decision: [what was chosen]
   - Rationale: [why chosen]
   - Alternatives considered: [what else evaluated]

**Output**: research.md with all technology choices justified

## Phase 1: Design & Contracts
*Prerequisites: research.md complete*

1. **Extract entities from feature spec** → `data-model.md`:
   - Resident: id, name, address, created_at, updated_at
   - Vehicle: id, resident_id, license_plate, make, model, color, created_at
   - Guest: id, resident_id, name, phone, created_at, valid_until
   - GuestVehicle: id, guest_id, license_plate, make, model, color
   - ParkingPermit: id, guest_id, valid_from, valid_until, printed_at

2. **Generate API contracts** from functional requirements:
   - GET /residents - List all residents
   - POST /residents - Create new resident
   - PUT /residents/{id} - Update resident
   - DELETE /residents/{id} - Delete resident
   - GET /guests - List all guests
   - POST /guests - Create new guest
   - PUT /guests/{id} - Update guest
   - DELETE /guests/{id} - Delete guest
   - POST /guests/{id}/permit - Generate parking permit
   - GET /permit/{id}/print - Print parking permit

3. **Generate contract tests** from contracts:
   - One test file per endpoint
   - Assert request/response schemas
   - Tests must fail (no implementation yet)

4. **Extract test scenarios** from user stories:
   - Each story → integration test scenario
   - Quickstart test = story validation steps

5. **Update agent file incrementally** (O(1) operation):
   - Run `.specify/scripts/bash/update-agent-context.sh cursor`
     **IMPORTANT**: Execute it exactly as specified above. Do not add or remove any arguments.
   - If exists: Add only NEW tech from current plan
   - Preserve manual additions between markers
   - Update recent changes (keep last 3)
   - Keep under 150 lines for token efficiency
   - Output to repository root

**Output**: data-model.md, /contracts/*, failing tests, quickstart.md, agent-specific file

## Phase 2: Task Planning Approach
*This section describes what the /tasks command will do - DO NOT execute during /plan*

**Task Generation Strategy**:
- Load `.specify/templates/tasks-template.md` as base
- Generate tasks from Phase 1 design docs (contracts, data model, quickstart)
- Each contract → contract test task [P]
- Each entity → model creation task [P] 
- Each user story → integration test task
- Implementation tasks to make tests pass

**Ordering Strategy**:
- TDD order: Tests before implementation 
- Dependency order: Models before services before UI
- Mark [P] for parallel execution (independent files)

**Estimated Output**: 25-30 numbered, ordered tasks in tasks.md

**IMPORTANT**: This phase is executed by the /tasks command, NOT by /plan

## Phase 3+: Future Implementation
*These phases are beyond the scope of the /plan command*

**Phase 3**: Task execution (/tasks command creates tasks.md)  
**Phase 4**: Implementation (execute tasks.md following constitutional principles)  
**Phase 5**: Validation (run tests, execute quickstart.md, performance validation)

## Complexity Tracking
*Fill ONLY if Constitution Check has violations that must be justified*

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| Separate frontend/backend | Clear separation of concerns, different deployment needs | Single file would mix concerns and reduce maintainability |

## Progress Tracking
*This checklist is updated during execution flow*

**Phase Status**:
- [x] Phase 0: Research complete (/plan command)
- [x] Phase 1: Design complete (/plan command)
- [x] Phase 2: Task planning complete (/plan command - describe approach only)
- [x] Phase 3: Tasks generated (/tasks command)
- [ ] Phase 4: Implementation complete
- [ ] Phase 5: Validation passed

**Gate Status**:
- [x] Initial Constitution Check: PASS
- [x] Post-Design Constitution Check: PASS
- [x] All NEEDS CLARIFICATION resolved
- [x] Complexity deviations documented

---
*Based on Constitution v2.1.1 - See `/memory/constitution.md`*