---
name: create-test-cases
description: Creates BDD test cases as Gherkin .feature files from a test plan and/or requirements and design documents, with positive, negative, and edge scenarios traced back to requirements. Use when asked to write, draft, or generate test cases, scenarios, or feature files from a test plan, requirements, user stories, acceptance criteria, or design documents. Output is always Gherkin (.feature); does not write step definitions or automation code.
argument-hint: "[path to test plan] [optional: requirements/design docs] [optional: output folder]"
user-invocable: true
disable-model-invocation: false
---

# Create Test Cases Skill

Assume the user is a **software QA engineer**. Turn a test plan and/or requirements into clear, traceable **BDD test cases** written in Gherkin. Every scenario must be traceable to a test plan case, a requirement, or a design statement.

**Output format (non-negotiable):** all test cases are written as Gherkin in one or more `*.feature` files. Do not output test cases as tables, prose, or any other format.

**Scope limit:** this skill writes `.feature` files only. Do not generate step definitions, page objects, hooks, config, or any Playwright/Cucumber/Selenium code. If the user asks for code, say it is outside this skill and offer to continue with the feature files.

## When to Use This Skill

✅ **Use when:**
- The user provides a test plan, requirements, user stories, acceptance criteria, or a design document and wants test cases / scenarios / feature files
- The user wants positive, negative, and edge coverage in BDD form

❌ **Don't use when:**
- The user wants a test plan (use `create-test-plan`) or a review of one (use `peer-review-test-plan`)
- The user wants automation code or step definitions (out of scope)

## Inputs

| Input | Required | Notes |
|-------|----------|-------|
| Test plan | **Yes, unless requirements are provided** | Primary source: test case IDs, types, priorities, test data, traceability matrix, open questions |
| Requirements document | Optional but recommended | Source of truth for expected behavior; use to verify and fill gaps |
| Technical / design document | Optional | Routes, validation rules, error messages, limits |
| Any other documents | Optional | Accept everything the user provides (notes, mockups, API specs, defect lists, Jira/Confluence exports) and consider it |

**Ask, do not assume** (use the ask-user tool if available):
1. **Test plan:** if the user has not provided a test plan (file or folder path), ask for it before reading or writing anything. If they have no test plan, ask for requirements/design documents instead. You may suggest candidates found in the workspace (for example `3 - QA`, `1 - Requirements`, `2 - Development`) but wait for the answer.
2. **Storage location:** if the user has not said where to store the feature files, ask before creating any file. You may suggest a folder (for example a `features` folder or `3 - QA\features`) but wait for the answer.
3. Ask whether any other documents should be considered, then use all that are given. When a folder is given, read the documents inside and confirm the list if unclear.

## Workflow

1. **Confirm inputs and output location** (see Inputs). Do not proceed without a test plan or requirements.
2. **Read all inputs completely.** Note IDs (`TC-LOGIN-004`, `FR-012`, `AC-023.1`, `NFR-004`).
3. **Check workspace conventions.** Look for existing `.feature` files, tag conventions, and step wording and match them. Load the `gherkin-bdd-best-practices` skill if available and follow it.
4. **Map sources to scenarios.** Convert each test plan case into one scenario (or one Scenario Outline when only data differs). Then check requirements/design for statements the plan misses and add scenarios for them, flagged as additions.
5. **Resolve conflicts.** The requirement is the source of truth unless the user says otherwise. Record conflicts and ambiguities; never silently pick one.
6. **Group into features** by area (one `.feature` file per feature or requirement group, for example registration, login, search).
7. **Write the feature files** per the rules below.
8. **Verify coverage** against the checklist and save.
9. **Summarize** for the user.

## Gherkin Authoring Rules

### Structure

- One `Feature:` per file, with a short business-value description (As a / I want / So that).
- Use `Background:` only for preconditions shared by **every** scenario in the file.
- Each `Scenario:` has **one purpose** and tests one behavior. Title states the behavior and outcome.
- Use `Scenario Outline` + `Examples:` for equivalence classes and boundary values that differ only by data.
- Strict **Given / When / Then** order. Use `And` / `But` for continuation. Avoid multiple `When`/`Then` blocks in one scenario.

### Style

- **Declarative, business-level language**, not UI scripting. Prefer `When the visitor submits the registration form with a password of 7 characters` over "click the field, type, click button".
- Consistent persona wording, present tense; no technical selectors, internal URLs, or code.
- **Concrete test data** in steps or Examples (for example `Passw0rd!`, a 1,001-character message). No real credentials or personal data.
- **Specific, observable `Then` steps** (exact message text, state, redirect). Avoid "works correctly" or "behaves as expected".
- Reuse consistent step phrasing across files so steps can be shared later.
- Do not invent requirements. If behavior is unspecified, still write the scenario, tag it `@needs-clarification`, and add a comment with the open question and the assumption used.

### Coverage

For each feature cover:

| Type | Tag | Purpose |
|------|-----|---------|
| Positive | `@positive` | Valid input and normal use work as specified |
| Negative | `@negative` | Invalid input, wrong state, or misuse is rejected or safely handled |
| Edge | `@edge` | At/below/above each boundary, special characters, Unicode, whitespace, repeated submits, session expiry, Back/refresh |

Apply equivalence partitioning, boundary value analysis (at, below, above each limit), state transitions, and error guessing. Include non-functional scenarios (accessibility, security basics, compatibility, performance) where requirements state them; tag `@nfr` plus the area. Where a non-functional item cannot be expressed as a testable scenario, add a commented note in the feature file rather than dropping it.

### Tags and traceability

Place tags above `Feature:` (file-wide) and `Scenario:` (specific):

- Type: `@positive` `@negative` `@edge` `@nfr`
- Priority: `@high` `@medium` `@low` (High for security, data loss, core flows, Must; Medium for Should; Low for Could/cosmetic)
- Requirement(s): `@FR-010` `@AC-023.1` `@NFR-004`, using IDs exactly as written in the sources
- Test plan case: `@TC-LOGIN-004`; for scenarios added beyond the plan, use a new ID in the same convention and tag `@added`
- Status as needed: `@needs-clarification`, `@blocked`
- Area: `@registration`, `@login`, etc.

Keep tags consistent with any existing project convention.

### Example

```gherkin
@registration
Feature: Customer registration
  As a visitor
  I want to create an account
  So that I can access member features

  @TC-REG-001 @positive @high @FR-010 @FR-014
  Scenario: Visitor registers with valid details
    Given the visitor is on the registration page
    When the visitor registers with email "qa.user+01@example.com" and password "Passw0rd!"
    Then the account is created
    And a confirmation message "Registration successful" is displayed

  @TC-REG-007 @edge @medium @FR-011
  Scenario Outline: Password length boundaries
    Given the visitor is on the registration page
    When the visitor registers with a password of <length> characters
    Then the registration is <outcome>

    Examples:
      | length | outcome  |
      | 7      | rejected |
      | 8      | accepted |
      | 64     | accepted |
      | 65     | rejected |
```

## Output

1. **Location:** use the folder the user confirmed. Never assume a default.
2. **File names:** one file per feature, kebab-case, based on the area, for example `registration.feature`. Use the user's naming if given.
3. **Never overwrite** existing feature files without asking.
4. Save only `.feature` files unless the user asks for something else.
5. In chat, summarize concisely: file paths; scenario counts by type (Positive / Negative / Edge / NFR); scenarios added beyond the test plan; requirements or plan cases not covered, with reasons; and the top conflicts and open questions to raise.

## Final Checklist

- [ ] Test plan (or requirements) and storage location were provided by the user or confirmed with them
- [ ] All other documents the user supplied were read and considered
- [ ] Every output file is a valid Gherkin `.feature` file; no tables, prose test cases, or code
- [ ] Every test plan case maps to a scenario (or is listed as not covered with a reason)
- [ ] Every requirement maps to at least one scenario or is listed as not covered
- [ ] Each feature has positive, negative, and edge scenarios; boundaries tested at, below, and above
- [ ] Non-functional requirements have scenarios or a noted approach
- [ ] Scenarios are declarative, single-purpose, with strict Given/When/Then and concrete data
- [ ] Then steps are specific and observable
- [ ] Tags include type, priority, requirement ID(s), and test plan ID
- [ ] Conflicts and ambiguities are flagged (`@needs-clarification`), not silently resolved
- [ ] No invented requirements; no real credentials or personal data
- [ ] No existing file was overwritten without asking
