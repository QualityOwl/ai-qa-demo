---
name: peer-review-test-plan
description: Peer reviews an existing manual test plan against its requirements and/or technical (design) documentation. Identifies gaps in test coverage (missing requirements, missing positive/negative/edge cases, missing boundaries and non-functional coverage) and checks that the plan is clear, complete, traceable, and executable. Use when asked to review, peer review, audit, or check a test plan or manual test cases. Does not rewrite the plan or write automation code.
argument-hint: "[path to test plan] [path to requirements doc or folder] [optional: path to design/technical doc]"
user-invocable: true
disable-model-invocation: false
---

# Peer Review Test Plan Skill

Assume the user is a **software QA engineer** acting as a peer reviewer. Produce a clear, actionable review of an existing test plan, judged against the source requirements (and design documents when provided). The review has two goals:

1. **Coverage:** find gaps where requirements, acceptance criteria, rules, limits, or non-functional needs are not tested, or are tested only partially.
2. **Clarity and completeness:** confirm that the plan is complete, unambiguous, traceable, and executable by someone unfamiliar with the application.

Every finding must point to a specific location in the test plan (section or test case ID) and, where relevant, to a specific source requirement ID.

**Scope limit:** this skill **reviews** a **manual** test plan. It does not rewrite the test plan, and it does not generate automation scripts, test framework code (Playwright, Selenium, Cucumber, xUnit, JUnit, and so on), or test harness code. If the user asks for code, say it is outside this skill. If the user asks for the plan to be fixed, finish the review first, then offer to apply the recommended changes as a separate step.

## When to Use This Skill

**Use when:**
- The user provides (or points to) an existing test plan and wants it reviewed, peer reviewed, or checked against requirements
- The user wants to know which requirements are not covered or are only partially covered
- The user wants feedback on test case clarity, completeness, traceability, or prioritization

**Don't use when:**
- No test plan exists yet and the user wants one created (use the `create-test-plan` skill instead)
- The user wants automated test code or any other code (out of scope for this skill)
- No requirements or design documents exist and the user cannot describe the feature (ask for input first; a coverage review is not possible without a baseline)

## Inputs

| Input | Required | Notes |
|-------|----------|-------|
| Test plan under review | Yes | The existing manual test plan (Markdown or another readable format) |
| Requirements document | One of the two is required | Functional and non-functional requirements, acceptance criteria, open questions |
| Technical / design document | One of the two is required | Routes, data model, validation rules, error handling, design decisions |

**Source location:** if the user has not provided the location of the test plan, or the location of the requirement document file(s) and/or the folder(s) containing them, **ask the user for the file path(s) and/or folder path(s)** before reading anything or starting the review (use the ask-user tool if available). The user may give individual files, folders, or both. When a folder is given, read the relevant documents inside it and confirm the list with the user if it is unclear which files apply. You may suggest likely candidates found in the workspace (for example folders such as `1 - Requirements`, `2 - Development`, and `3 - QA`) as options, but wait for the user's answer. If both requirements and design documents are provided, use both and compare them.

## Workflow

1. **Confirm the inputs.** If the user has not provided the test plan and the requirement document file(s) and/or folder(s), ask for them before continuing (see Inputs).
2. **Read all inputs completely.** Do not skim. Note every source ID (for example `FR-012`, `AC-023.1`, `NFR-004`) and every test case ID in the plan.
3. **Build an independent baseline.** From the source documents, list every testable statement: requirement, acceptance criterion, business rule, limit, error condition, state, and design rule. Do this **before** relying on the plan's own traceability matrix.
4. **Map the plan to the baseline.** For each testable statement, find the test cases that actually verify it (read the steps and expected results, not just the Requirement(s) column). Mark each as Covered, Partial, or Not covered.
5. **Check the plan's traceability.** Compare your mapping with the plan's traceability matrix. Flag wrong or missing requirement IDs, IDs that do not exist in the source documents, and coverage claims that the test cases do not support.
6. **Run the coverage checks** (see Review Checklist: Coverage).
7. **Run the clarity and completeness checks** (see Review Checklist: Clarity and Completeness).
8. **Cross-check requirements against design** (when both exist). Confirm that the plan lists every conflict, omission, or unconfirmed decision and does not silently resolve one in favor of the design.
9. **Classify and prioritize findings** using the severity definitions below. Do not report style preferences or cosmetic wording as findings.
10. **Write the review report** using the template below.
11. **Confirm the save location.** If the user has not said where to store the review, ask them before writing any file (see Output).
12. **Save the file** and summarize the result for the user. Do not modify the test plan under review unless the user explicitly asks.

## Review Checklist: Coverage

For each feature or requirement group, check:

- **Requirement coverage:** every requirement and acceptance criterion maps to at least one test case, or is listed in the plan as not covered with a reason.
- **Test case types:** each feature has **Positive**, **Negative**, and **Edge** test cases.
  - Positive: valid input and normal use confirm the stated behavior.
  - Negative: invalid input, wrong state, or misuse is rejected or handled safely (blank required fields, wrong password, duplicates, expired tokens, unauthorized access, injection strings).
  - Edge: limits, boundaries, and unusual-but-valid situations (special characters and Unicode, first/last page, concurrent actions, session expiring mid-action, browser Back and refresh).
- **Boundary values:** each numeric or length limit is tested at, just below, and just above the limit (minimum, minimum-1, maximum, maximum+1).
- **Equivalence classes:** each class of valid and invalid input has a test case.
- **State transitions:** relevant states (for example logged out, logged in, locked, expired) and the moves between them are tested.
- **Error handling:** each documented error condition and message is tested.
- **Non-functional requirements:** performance, compatibility (browsers, devices), responsive layout, accessibility, security, privacy, and reliability each have test cases or a clearly labeled approach wherever the documents state them.
- **Security basics:** input sanitization, authorization, session handling, and sensitive data in logs or responses are tested wherever the documents mention them.
- **Requirement vs. design conflicts:** where the documents differ, the test case is written against the **requirement** (the source of truth unless the user says otherwise) and the conflict is recorded.
- **Unspecified behavior:** behavior that the documents do not define is marked **Blocked / needs clarification** with an open question, not covered by an invented expected result.

## Review Checklist: Clarity and Completeness

### Plan-level completeness

Confirm that the plan contains these sections (or says why one does not apply): header table (ID, sources, author, status, date), Introduction, Scope (in and out), Test Approach, Test Environment and Tools, Test Data, Entry/Exit/Suspension Criteria, Manual Test Cases, Non-Functional Test Cases, Requirements Traceability Matrix, Requirements vs. Design Discrepancies (when both documents exist), Open Questions, Risks and Assumptions, Defect Management, and Deliverables and Schedule.

Also check:
- In-scope and out-of-scope items agree with the source documents and with the test cases actually written.
- Entry and exit criteria are measurable (for example "100% of High priority test cases executed, no open Critical defects"), not vague.
- Test data covers boundary data and special states needed by the test cases (for example a locked account, an expired token).
- Open questions each list the affected test cases and the assumption used in the meantime.

### Test case quality

For each test case, check that it:
- Has **one clear purpose** and a unique, consistently formatted ID (for example `TC-<AREA>-<NNN>`).
- Has a correct **Type** (Positive, Negative, or Edge) that matches what it actually tests.
- References source IDs **exactly** as written in the input documents.
- States **preconditions** (for example "user is signed out", "account is locked").
- Has **numbered, human-executable steps** that someone unfamiliar with the application can follow, with no code, scripts, or automation snippets.
- Uses **concrete test data** (for example `Passw0rd!`, a 1,001-character message) rather than "valid password".
- Has a **specific, observable expected result**. Flag "works correctly", "behaves as expected", "error is shown" without the message or behavior, and similar wording.
- Does not **invent requirements**: the expected result is supported by the source documents, or the case is marked Blocked / needs clarification.
- Is not a **duplicate** of another test case covering the same behavior.
- Has a **priority** that matches the risk: High for security, data loss, core flows, and Must requirements; Medium for Should; Low for Could and cosmetic items.
- Contains no real credentials or personal data.

## Severity Definitions

| Severity | Meaning | Examples |
|----------|---------|----------|
| **High** | Coverage gap or defect in the plan that could let a significant defect escape, or that makes test cases unexecutable or misleading | Must requirement not covered; security requirement untested; expected result contradicts the requirement; invented requirement; design silently overrides requirement |
| **Medium** | Partial coverage or clarity problem that weakens the plan's reliability | Missing negative or edge cases for a feature; boundary tested on one side only; vague expected result; missing preconditions or test data; wrong traceability ID; misclassified priority on a core flow |
| **Low** | Minor completeness or consistency issue with limited risk | Should/Could requirement partially covered; inconsistent ID format; missing placeholder section; minor duplicate |

Do not report formatting, style preferences, or cosmetic wording unless they make a test case ambiguous or unexecutable.

## Review Report Template

Use these sections in this order. Omit a section only if it truly does not apply, and say so.

```markdown
# Test Plan Peer Review: <Test Plan ID / Name>

| Field | Value |
|-------|-------|
| Review ID | TPR-<APP>-<number> |
| Test plan reviewed | <test plan file name and ID> |
| Reviewed against | <requirements doc ID/name>, <design doc ID/name> |
| Reviewer | <QA engineer or "QA Peer Reviewer"> |
| Date | <date> |
| Overall assessment | Approved / Approved with changes / Changes required |

## 1. Summary
Two to four sentences on overall quality, the most important gaps, and the recommendation. Include counts: findings by severity, and requirements Covered / Partial / Not covered.

## 2. Coverage Gap Analysis
### 2.1 Requirements Coverage
| Requirement | Description (short) | Plan's claimed test cases | Reviewer assessment | Gap |
|-------------|---------------------|---------------------------|---------------------|-----|
Reviewer assessment is Covered, Partial, or Not covered.

### 2.2 Missing Test Case Types and Boundaries
Per feature: which of Positive / Negative / Edge are missing, and which boundaries are untested.

### 2.3 Non-Functional Coverage
Each NFR and whether it is covered, partial, or missing.

## 3. Findings
| # | Severity | Category | Location (section / TC ID) | Source ref | Finding | Recommendation |
|---|----------|----------|----------------------------|------------|---------|----------------|
| 1 | High | Coverage | Section 7.2 | FR-014 | ... | ... |

Category is one of: Coverage, Traceability, Clarity, Completeness, Accuracy, Priority, Duplication.

## 4. Suggested Additional Test Cases
Short descriptions (not full rewrites) of test cases needed to close the gaps:
| Suggested ID | Type | Requirement(s) | Title | Key data / boundary | Priority |
|--------------|------|----------------|-------|---------------------|----------|

## 5. Traceability Issues
Wrong, missing, or non-existent requirement IDs, and coverage claims not supported by the test cases.

## 6. Requirements vs. Design Discrepancies Not Addressed
Only when both documents were provided. Conflicts the plan missed or silently resolved.

## 7. Questions for the Test Plan Author
Points that need the author's clarification before the plan can be approved.

## 8. Strengths
Brief list of what the plan does well (keep short).
```

## Output

1. **Storage location:** if the user has not provided a location for the review report, **ask them where to store it** before creating any file (use the ask-user tool if available). Do not assume a default folder. You may suggest a likely folder (for example the `3 - QA` folder beside the test plan) as an option, but wait for the answer.
2. Save the review as Markdown. Suggest a file name based on the test plan, replacing the suffix: for `acme-services-website.test-plan.md`, suggest `acme-services-website.test-plan-review.md`. Use the user's name if they give one.
3. Never overwrite an existing review, and never modify the test plan under review, without asking the user.
4. In chat, give a short summary: file path, overall assessment, number of findings by severity, requirements with partial or no coverage, and the **top findings** the test plan author should address first.

## Final Checklist

Before finishing, verify:

- [ ] The test plan and the requirement document file(s) and/or folder(s) were provided by the user or confirmed with them
- [ ] An independent requirements baseline was built from the source documents, not copied from the plan's traceability matrix
- [ ] Every requirement and acceptance criterion has a reviewer coverage assessment (Covered, Partial, Not covered)
- [ ] Missing Positive / Negative / Edge cases and untested boundaries are identified per feature
- [ ] Non-functional requirements were checked for coverage
- [ ] Test case clarity (preconditions, steps, test data, expected results) and plan completeness were checked
- [ ] Traceability errors and requirements vs. design conflicts are reported
- [ ] Every finding has a severity, a specific location, and an actionable recommendation
- [ ] No style-only or cosmetic findings were reported
- [ ] No code of any kind was written, and the test plan under review was not modified
- [ ] The save location was provided by the user or confirmed with them