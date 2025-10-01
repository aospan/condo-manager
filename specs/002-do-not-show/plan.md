
# Implementation Plan: Embedded Status Notifications

**Branch**: `002-do-not-show` | **Date**: 2025-09-29 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/002-do-not-show/spec.md`

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
Replace all popup confirmation dialogs with embedded status sections within the page interface to improve user workflow and reduce disruption. This involves creating a reusable status notification system that displays success, error, and confirmation messages inline without blocking user interaction.

## Technical Context
**Language/Version**: JavaScript (ES6+), HTML5, CSS3, Rust 1.90  
**Primary Dependencies**: Existing frontend (HTML/CSS/JS), Axum backend, SQLite  
**Storage**: N/A (UI-only feature)  
**Testing**: Jest for frontend tests, cargo test for backend  
**Target Platform**: Web browsers (Chrome, Firefox, Safari, Edge)  
**Project Type**: Web application (frontend + backend)  
**Performance Goals**: Status messages appear within 100ms of action completion  
**Constraints**: Must be accessible (WCAG 2.1 AA), responsive design, no blocking UI  
**Scale/Scope**: Single condo management application, ~5-10 concurrent users

## Constitution Check
*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### Core Principles Compliance
- **Simplicity**: ✅ Status notification system is a focused UI enhancement
- **Test-First**: ✅ Will implement tests before status notification components
- **User Value**: ✅ Improves user experience by eliminating disruptive popups
- **Accessibility**: ✅ Must comply with WCAG 2.1 AA standards
- **Performance**: ✅ Non-blocking UI with <100ms response time

### Complexity Assessment
- **Scope**: Minimal - UI-only feature with no backend changes
- **Dependencies**: Uses existing frontend framework
- **Integration**: Simple integration with existing JavaScript modules
- **Risk**: Low - Non-breaking change to existing functionality

**Status**: ✅ PASS - No constitutional violations identified

## Project Structure

### Documentation (this feature)
```
specs/[###-feature]/
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
│   ├── models/
│   ├── services/
│   ├── handlers/
│   ├── database/
│   └── main.rs
└── tests/
    ├── contract/
    ├── integration/
    └── unit/

frontend/
├── src/
│   ├── css/
│   ├── js/
│   ├── assets/
│   └── index.html
└── tests/
    ├── e2e/
    └── unit/
```

**Structure Decision**: Web application structure with separate frontend and backend directories. The status notification system will be implemented primarily in the frontend JavaScript modules, with minimal backend changes for error response formatting.

## Phase 0: Outline & Research
1. **Extract unknowns from Technical Context** above:
   - For each NEEDS CLARIFICATION → research task
   - For each dependency → best practices task
   - For each integration → patterns task

2. **Generate and dispatch research agents**:
   ```
   For each unknown in Technical Context:
     Task: "Research {unknown} for {feature context}"
   For each technology choice:
     Task: "Find best practices for {tech} in {domain}"
   ```

3. **Consolidate findings** in `research.md` using format:
   - Decision: [what was chosen]
   - Rationale: [why chosen]
   - Alternatives considered: [what else evaluated]

**Output**: research.md with all NEEDS CLARIFICATION resolved

## Phase 1: Design & Contracts
*Prerequisites: research.md complete*

1. **Extract entities from feature spec** → `data-model.md`:
   - Entity name, fields, relationships
   - Validation rules from requirements
   - State transitions if applicable

2. **Generate API contracts** from functional requirements:
   - For each user action → endpoint
   - Use standard REST/GraphQL patterns
   - Output OpenAPI/GraphQL schema to `/contracts/`

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
- Status notification system implementation tasks
- Frontend integration tasks for existing modules
- Testing tasks for status message functionality
- Accessibility and performance validation tasks

**Specific Task Categories**:
1. **Status Manager Core** (5-6 tasks):
   - StatusManager class implementation
   - Message lifecycle management
   - Auto-dismiss functionality
   - Manual dismiss functionality
   - Message history management

2. **UI Components** (4-5 tasks):
   - Status message HTML structure
   - CSS styling for all message types
   - Animation and transitions
   - Responsive design
   - Action button components

3. **Frontend Integration** (6-8 tasks):
   - Update residents.js module
   - Update guests.js module
   - Update vehicles.js module
   - Update permits.js module
   - Error handling integration
   - Success feedback integration

4. **Testing** (8-10 tasks):
   - Unit tests for StatusManager
   - Integration tests for message display
   - E2E tests for user scenarios
   - Accessibility tests
   - Performance tests

5. **Documentation** (2-3 tasks):
   - Update user documentation
   - Code documentation
   - API documentation updates

**Ordering Strategy**:
- TDD order: Tests before implementation
- Core before integration: StatusManager before module updates
- Independent tasks marked [P] for parallel execution
- UI components can be developed in parallel with core logic

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
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |


## Progress Tracking
*This checklist is updated during execution flow*

**Phase Status**:
- [x] Phase 0: Research complete (/plan command)
- [x] Phase 1: Design complete (/plan command)
- [x] Phase 2: Task planning complete (/plan command - describe approach only)
- [x] Phase 3: Tasks generated (/tasks command)
- [x] Phase 4: Implementation complete
- [ ] Phase 5: Validation passed

**Gate Status**:
- [x] Initial Constitution Check: PASS
- [x] Post-Design Constitution Check: PASS
- [x] All NEEDS CLARIFICATION resolved
- [x] Complexity deviations documented

---
*Based on Constitution v2.1.1 - See `/memory/constitution.md`*
