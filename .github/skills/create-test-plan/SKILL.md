---
name: create-test-plan
description: Creates a manual test plan for a software QA engineer, with positive, negative, and edge case manual test cases traced back to requirements and/or technical (design) documentation. Use when asked to write, draft, or generate a test plan or manual test cases from requirements, user stories, acceptance criteria, or design documents. Does not write automation code.
argument-hint: "[path to requirements doc] [optional: path to design/technical doc]"
user-invocable: true
disable-model-invocation: false
---

# Create Test Plan Skill

Assume the user is a **software QA engineer**. Produce a clear, traceable test plan that a QA team can review and execute **manually**. Every test case must be traceable to a requirement or a design statement.

**Scope limit:** this skill covers **manual test cases only**. Writing code is out of scope: do not generate automation scripts, test framework code (Playwright, Selenium, Cucumber, xUnit, JUnit, and so on), or test harness code. If the user asks for code, say it is outside this skill and offer to continue the manual test plan instead.

## When to Use This Skill

✅ **Use when:**
- The user provides (or points to) a requirements document, user stories, acceptance criteria, or a technical/design document and wants a test plan or manual test cases
- The user wants positive, negative, and edge case coverage for a feature or release

❌ **Don't use when:**
- The user wants automated test code or any other code (out of scope for this skill)
- The user wants only a review of an existing test plan or requirements (review them instead)
- No source documents exist and the user cannot describe the feature (ask for input first)

## Inputs

| Input | Required | Notes |
|-------|----------|-------|
| Requirements document | One of the two is required | Functional and non-functional requirements, acceptance criteria, open questions |
| Technical / design document | One of the two is required | Routes, data model, validation rules, error handling, design decisions |

**Source location:** if the user has not provided the location of the requirement document file(s) and/or the folder(s) containing them, **ask the user for the file path(s) and/or folder path(s)** before reading anything or drafting the plan (use the ask-user tool if available). The user may give individual files, folders, or both. When a folder is given, read the requirement and design documents inside it and confirm the list with the user if it is unclear which files apply. You may suggest likely candidates found in the workspace (for example folders such as `1 - Requirements` and `2 - Development`) as options, but wait for the user's answer. If both documents are provided, use both and compare them.

## Workflow

1. **Confirm the source documents.** If the user has not provided the requirement document file(s) and/or folder(s), ask for them before continuing (see Inputs).
2. **Read all inputs completely.** Do not skim. Note IDs (for example `FR-012`, `AC-023.1`, `NFR-004`).
3. **Extract testable statements.** List each requirement, acceptance criterion, business rule, limit, and design rule that can be verified.
4. **Cross-check requirements against design** (when both exist). Record every conflict, omission, or unconfirmed decision. Examples: a limit that differs between documents, a requirement with no design coverage, a design choice marked pending or TBD.
5. **Identify gaps and ambiguities.** Look for vague wording ("a period of", "normal load"), missing values, and untestable statements. Collect these as open questions rather than guessing.
6. **Design scenarios** using the techniques below, covering positive, negative, and edge cases for each requirement.
7. **Build the traceability matrix.** Every requirement must map to at least one scenario, or be listed as "not covered" with a reason.
8. **Write the test plan** using the template below.
9. **Confirm the output format and save location.** Always ask what file format the user wants for the test plan and recommend Markdown (`.md`) as the default. If the user has not said where to store it, ask them before writing any file (see Output).
10. **Save the file** in the confirmed format and summarize the result for the user.

## Scenario Design Rules

### The three test case types

| Type | Purpose | Examples |
|------|---------|----------|
| **Positive** | Confirms the system does what the requirement says with valid input and normal use | Valid registration; search with a matching term |
| **Negative** | Confirms the system rejects or safely handles invalid input, wrong state, or misuse | Blank required field; wrong password; duplicate email; expired token; unauthorized access; injection strings |
| **Edge case** | Probes limits, boundaries, and unusual-but-valid situations | Minimum, maximum, minimum-1, and maximum+1 lengths; exactly N failed attempts; special characters and Unicode; first and last page of results; concurrent actions; session expiring mid-action; browser Back and refresh |

### Techniques to apply

- **Equivalence partitioning:** one scenario per class of valid and invalid input.
- **Boundary value analysis:** test at, just below, and just above each numeric or length limit.
- **State transition testing:** logged out, logged in, locked, expired, and the moves between them.
- **Error guessing:** whitespace-only input, leading/trailing spaces, case differences, very long strings, special characters, double-clicking submit, repeated submissions.
- **Cross-document checks:** when the design differs from the requirement, write the scenario against the **requirement** and note the conflict. The requirement is the source of truth unless the user says otherwise.

### Non-functional coverage

Create scenarios (or a clearly labeled approach) for each non-functional requirement where stated: performance, compatibility (browsers, devices), responsive layout, accessibility, security, privacy, and reliability. Cover security basics (input sanitization, authorization, session handling, sensitive data in logs or responses) wherever the documents mention them.

### Manual test case quality rules

- Write every test case as a **manual** case: numbered, human-executable steps a tester performs in the browser or application. No code, scripts, or automation snippets.
- Each test case has **one clear purpose** and a **specific, observable expected result**. Avoid "works correctly" or "behaves as expected".
- Include **preconditions** (for example "user is signed out", "account is locked").
- Use **concrete test data** (for example `Passw0rd!`, a 1,001-character message) rather than "valid password".
- Write steps so someone unfamiliar with the application can follow them.
- Do **not** invent requirements. If behavior is not specified, create a scenario marked **Blocked / needs clarification** and add an open question.
- Do not duplicate scenarios. One scenario per distinct behavior.
- Prioritize by risk: **High** for security, data loss, core flows, and Must requirements; **Medium** for Should; **Low** for Could and cosmetic items.

## Test Plan Template

Use these sections in this order. Omit a section only if it truly does not apply, and say so.

```markdown
# Test Plan: <Application / Feature> (<version>)

| Field | Value |
|-------|-------|
| Test Plan ID | TP-<APP>-<number> |
| Based on | <requirements doc ID/name>, <design doc ID/name> |
| Author | <QA engineer or "QA Team"> |
| Status | Draft |
| Date | <date> |

## 1. Introduction
Purpose of this test plan and a one-paragraph summary of the system under test.

## 2. Scope
### 2.1 In Scope
### 2.2 Out of Scope

## 3. Test Approach
Test types (functional, negative, boundary, security, accessibility, compatibility, performance) and how defects will be reported. All test cases are executed manually; do not include automation scripts or code.

## 4. Test Environment and Tools
Environments, browsers/devices, test accounts, email sink, tools.

## 5. Test Data
Data needed, including boundary data and special states (for example a locked account, an expired token).

## 6. Entry and Exit Criteria
### 6.1 Entry Criteria
### 6.2 Exit Criteria
### 6.3 Suspension / Resumption Criteria

## 7. Manual Test Cases
One subsection per feature or requirement group, each with a table:

| ID | Type | Requirement(s) | Title | Preconditions | Steps | Test data | Expected result | Priority |
|----|------|----------------|-------|---------------|-------|-----------|-----------------|----------|
| TC-REG-001 | Positive | FR-010, FR-014 | ... | ... | 1. ...<br>2. ... | ... | ... | High |

Type is one of: Positive, Negative, Edge.

## 8. Non-Functional Manual Test Cases
Same table format, grouped by NFR.

## 9. Requirements Traceability Matrix
| Requirement | Description (short) | Scenario IDs | Coverage |
|-------------|---------------------|--------------|----------|
Coverage is Covered, Partial, Blocked, or Not covered (with reason).

## 10. Requirements vs. Design Discrepancies
Only when both documents were provided. Table of: ID, requirement says, design says, impact on testing, recommended question or action.

## 11. Open Questions and Clarifications Needed
Ambiguities, missing values, unconfirmed decisions. Each with the affected scenarios and an assumption used in the meantime.

## 12. Risks and Assumptions
Risk, likelihood, impact, mitigation. List assumptions used to write the scenarios.

## 13. Defect Management
Severity and priority definitions, and the defect lifecycle.

## 14. Deliverables and Schedule
Test plan, manual test cases, execution results, defect reports, summary report. Include effort or schedule only if the user provides inputs; otherwise leave placeholders.
```

### ID conventions

- Test cases: `TC-<AREA>-<NNN>` (for example `TC-LOGIN-004`).
- Keep AREA short and consistent (REG, LOGIN, RESET, CONTACT, SEARCH, NAV, NFR).
- Reference source IDs exactly as written in the input documents.

## Output

1. **Storage location:** if the user has not provided a location for the test plan, **ask them where to store it** before creating any file (use the ask-user tool if available). Do not assume a default folder. You may suggest a likely folder (for example a `3 - QA` folder beside the source documents) as an option, but wait for the answer.
2. Always ask what file format the user wants and recommend Markdown (`.md`) as the default. Save the test plan in the format they confirm. Suggest a file name based on the requirements document, replacing the suffix: for `acme-services-website.requirements.md`, suggest `acme-services-website.test-plan.md`. Use the user's name if they give one.
3. Never overwrite an existing test plan without asking the user.
4. In chat, give a short summary: file path, number of test cases by type (Positive / Negative / Edge), requirements with partial or no coverage, and the **top discrepancies and open questions** the QA engineer should raise.

## Final Checklist

Before finishing, verify:

- [ ] Every requirement and acceptance criterion maps to at least one test case or is listed as not covered with a reason
- [ ] Each feature has positive, negative, and edge test cases
- [ ] Boundary values are tested at, below, and above each limit
- [ ] Non-functional requirements have test cases or an approach
- [ ] Every test case is manual, with numbered steps; no code of any kind was written
- [ ] The requirement document file(s) and/or folder(s) were provided by the user or confirmed with them
- [ ] The save location was provided by the user or confirmed with them
- [ ] Requirements and design conflicts are listed, not silently resolved
- [ ] Ambiguities are listed as open questions with a stated assumption
- [ ] Expected results are specific and observable
- [ ] No invented requirements; no real credentials or personal data in test data
