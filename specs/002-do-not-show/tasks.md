# Tasks: Embedded Status Notifications

**Input**: Design documents from `/specs/002-do-not-show/`
**Prerequisites**: plan.md (complete), research.md (complete), data-model.md (complete), contracts/ (complete)

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
- Paths shown below follow the project structure from plan.md

## Phase 3.1: Setup
- [ ] T001 Create status notification module directory structure in `frontend/src/js/`
- [ ] T002 Add Jest test framework configuration to `frontend/package.json` if not present
- [ ] T003 [P] Create test directory structure in `frontend/tests/unit/status/` and `frontend/tests/e2e/status/`

## Phase 3.2: Tests First (TDD) ⚠️ MUST COMPLETE BEFORE 3.3
**CRITICAL: These tests MUST be written and MUST FAIL before ANY implementation**

### Unit Tests
- [ ] T004 [P] Unit test for StatusManager.show() in `frontend/tests/unit/status/test_status_manager.spec.js`
- [ ] T005 [P] Unit test for StatusManager.dismiss() in `frontend/tests/unit/status/test_status_dismiss.spec.js`
- [ ] T006 [P] Unit test for StatusManager auto-dismiss in `frontend/tests/unit/status/test_auto_dismiss.spec.js`
- [ ] T007 [P] Unit test for message type rendering in `frontend/tests/unit/status/test_message_types.spec.js`
- [ ] T008 [P] Unit test for action buttons in `frontend/tests/unit/status/test_action_buttons.spec.js`

### Integration Tests
- [ ] T009 [P] Integration test for resident creation success message in `frontend/tests/e2e/status/test_resident_success.spec.js`
- [ ] T010 [P] Integration test for guest creation error message in `frontend/tests/e2e/status/test_guest_error.spec.js`
- [ ] T011 [P] Integration test for message replacement in `frontend/tests/e2e/status/test_message_replacement.spec.js`
- [ ] T012 [P] Integration test for accessibility (ARIA) in `frontend/tests/e2e/status/test_accessibility.spec.js`

## Phase 3.3: Core Implementation (ONLY after tests are failing)

### Status Manager Core
- [ ] T013 Implement StatusMessage class in `frontend/src/js/status-message.js`
- [ ] T014 Implement StatusManager singleton in `frontend/src/js/status-manager.js`
- [ ] T015 Add auto-dismiss timer functionality in `frontend/src/js/status-manager.js`
- [ ] T016 Add manual dismiss functionality in `frontend/src/js/status-manager.js`
- [ ] T017 Add message history tracking in `frontend/src/js/status-manager.js`

### UI Components
- [ ] T018 [P] Create status message HTML template in `frontend/src/js/status-template.js`
- [ ] T019 [P] Add base status message CSS styles in `frontend/src/css/status.css`
- [ ] T020 Add message type-specific CSS styles (success, error, info, warning) in `frontend/src/css/status.css`
- [ ] T021 Add animation and transition CSS in `frontend/src/css/status.css`
- [ ] T022 Add responsive design CSS for mobile in `frontend/src/css/status.css`
- [ ] T023 [P] Implement action button component in `frontend/src/js/status-actions.js`

### Accessibility Features
- [ ] T024 Add ARIA live region attributes in `frontend/src/js/status-template.js`
- [ ] T025 Add keyboard navigation support in `frontend/src/js/status-manager.js`
- [ ] T026 Add high contrast mode support in `frontend/src/css/status.css`
- [ ] T027 Add screen reader announcements in `frontend/src/js/status-manager.js`

## Phase 3.4: Integration

### Remove Popup Dialogs
- [ ] T028 Remove alert() calls from `frontend/src/js/residents.js`
- [ ] T029 Remove alert() calls from `frontend/src/js/guests.js`
- [ ] T030 Remove alert() calls from `frontend/src/js/vehicles.js`
- [ ] T031 Remove alert() calls from `frontend/src/js/permits.js`
- [ ] T032 Remove confirm() calls from `frontend/src/js/residents.js`
- [ ] T033 Remove confirm() calls from `frontend/src/js/guests.js`

### Add Status Messages
- [ ] T034 Add success message for resident creation in `frontend/src/js/residents.js`
- [ ] T035 Add success message for resident update in `frontend/src/js/residents.js`
- [ ] T036 Add success message for resident deletion in `frontend/src/js/residents.js`
- [ ] T037 Add error message handling for resident operations in `frontend/src/js/residents.js`
- [ ] T038 Add success message for guest registration in `frontend/src/js/guests.js`
- [ ] T039 Add success message for guest update in `frontend/src/js/guests.js`
- [ ] T040 Add success message for guest deletion in `frontend/src/js/guests.js`
- [ ] T041 Add error message handling for guest operations in `frontend/src/js/guests.js`
- [ ] T042 Add success message for vehicle operations in `frontend/src/js/vehicles.js`
- [ ] T043 Add error message handling for vehicle operations in `frontend/src/js/vehicles.js`
- [ ] T044 Add success message for permit generation in `frontend/src/js/permits.js`
- [ ] T045 Add error message handling for permit operations in `frontend/src/js/permits.js`

### Main App Integration
- [ ] T046 Import StatusManager in `frontend/src/js/app.js`
- [ ] T047 Initialize status notification container in `frontend/src/index.html`
- [ ] T048 Link status.css stylesheet in `frontend/src/index.html`
- [ ] T049 Add global error handler with status messages in `frontend/src/js/app.js`

## Phase 3.5: Polish

### Additional Testing
- [ ] T050 [P] Add performance test for message display (<100ms) in `frontend/tests/unit/status/test_performance.spec.js`
- [ ] T051 [P] Add browser compatibility tests in `frontend/tests/e2e/status/test_browser_compat.spec.js`
- [ ] T052 [P] Add visual regression tests in `frontend/tests/e2e/status/test_visual.spec.js`

### Code Quality
- [ ] T053 Add JSDoc comments to StatusManager in `frontend/src/js/status-manager.js`
- [ ] T054 Add JSDoc comments to StatusMessage in `frontend/src/js/status-message.js`
- [ ] T055 Run ESLint and fix any issues in status notification modules
- [ ] T056 Run Prettier and format all status notification files

### Documentation
- [ ] T057 [P] Update README.md with status notification usage examples
- [ ] T058 [P] Create developer guide in `docs/status-notifications.md`
- [ ] T059 [P] Update user documentation with new UI patterns

### Validation
- [ ] T060 Run all quickstart.md test scenarios manually
- [ ] T061 Verify WCAG 2.1 AA compliance using accessibility tools
- [ ] T062 Test on Chrome, Firefox, Safari, and Edge browsers
- [ ] T063 Test responsive design on mobile devices
- [ ] T064 Verify no console errors or warnings

## Dependencies

### Phase Order
- Setup (T001-T003) must complete before Tests (T004-T012)
- Tests (T004-T012) must complete before Core Implementation (T013-T027)
- Core Implementation (T013-T027) must complete before Integration (T028-T049)
- Integration (T028-T049) must complete before Polish (T050-T064)

### Specific Dependencies
- T013 (StatusMessage class) blocks T014 (StatusManager)
- T014 (StatusManager) blocks T015-T017 (manager features)
- T018 (HTML template) blocks T024 (ARIA attributes)
- T019 (base CSS) blocks T020-T022 (specific styles)
- T014 (StatusManager) blocks all integration tasks (T028-T049)
- T047 (HTML container) blocks T046 (app integration)

### Independent Tasks (Can Run in Parallel)
- All test files (T004-T012) are independent
- T018 (template) and T019 (CSS) are independent
- T023 (action buttons) is independent of T013-T017
- T053-T054 (documentation) are independent
- T057-T059 (documentation) are independent

## Parallel Execution Examples

### Batch 1: Test Creation (Phase 3.2)
```bash
# All test files can be created in parallel
Task: "Unit test for StatusManager.show() in frontend/tests/unit/status/test_status_manager.spec.js"
Task: "Unit test for StatusManager.dismiss() in frontend/tests/unit/status/test_status_dismiss.spec.js"
Task: "Unit test for StatusManager auto-dismiss in frontend/tests/unit/status/test_auto_dismiss.spec.js"
Task: "Unit test for message type rendering in frontend/tests/unit/status/test_message_types.spec.js"
Task: "Unit test for action buttons in frontend/tests/unit/status/test_action_buttons.spec.js"
Task: "Integration test for resident creation success in frontend/tests/e2e/status/test_resident_success.spec.js"
Task: "Integration test for guest creation error in frontend/tests/e2e/status/test_guest_error.spec.js"
Task: "Integration test for message replacement in frontend/tests/e2e/status/test_message_replacement.spec.js"
Task: "Integration test for accessibility in frontend/tests/e2e/status/test_accessibility.spec.js"
```

### Batch 2: Core Components (Phase 3.3)
```bash
# Template and base CSS can be created in parallel
Task: "Create status message HTML template in frontend/src/js/status-template.js"
Task: "Add base status message CSS styles in frontend/src/css/status.css"
```

### Batch 3: Documentation (Phase 3.5)
```bash
# All documentation tasks are independent
Task: "Update README.md with status notification usage"
Task: "Create developer guide in docs/status-notifications.md"
Task: "Update user documentation with new UI patterns"
```

## Notes
- [P] tasks = different files, no dependencies
- Verify tests fail before implementing (Phase 3.2 → 3.3)
- Run tests after each implementation task to verify green
- Commit after completing each phase
- Focus on accessibility throughout (WCAG 2.1 AA compliance)
- Ensure no breaking changes to existing functionality

## Validation Checklist
*GATE: Must be complete before marking tasks as done*

- [x] All contracts have corresponding tests (status-api.yaml → unit/integration tests)
- [x] All entities have model tasks (StatusMessage, StatusManager)
- [x] All tests come before implementation (Phase 3.2 before 3.3)
- [x] Parallel tasks truly independent (marked with [P])
- [x] Each task specifies exact file path
- [x] No task modifies same file as another [P] task
- [x] Accessibility requirements included
- [x] Integration with existing modules planned
- [x] Documentation tasks included

## Summary

**Total Tasks**: 64
- **Setup**: 3 tasks
- **Tests**: 9 tasks (all must fail before implementation)
- **Core Implementation**: 15 tasks
- **Integration**: 22 tasks
- **Polish**: 15 tasks

**Estimated Time**: 
- Setup: 1-2 hours
- Tests: 4-6 hours
- Core Implementation: 8-10 hours
- Integration: 6-8 hours
- Polish: 4-6 hours
- **Total**: 23-32 hours

**Key Milestones**:
1. Tests written and failing (end of Phase 3.2)
2. StatusManager core working (T014-T017)
3. UI components complete (T018-T023)
4. All popup dialogs removed (T028-T033)
5. All modules integrated (T034-T049)
6. All validations passing (T060-T064)
