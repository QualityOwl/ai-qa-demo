---
name: create-new-bug
description: Creates a detailed, clear bug report for a software QA engineer, with title, description, related test cases and requirements, environment, test data, repro steps, expected vs. actual results, and evidence. Asks the user for any missing details first. Use when asked to create, write, draft, log, file, or report a new bug, defect, or issue found during testing. Does not write automation code or fix the bug.
argument-hint: "[short description of the problem] [optional: failed test case ID]"
user-invocable: true
disable-model-invocation: false
---

# Create New Bug Skill

Assume the user is a **software QA engineer**. Produce a bug report that is detailed, clear, and easy to understand for **BAs, developers, and QAs**. A reader who was not present during testing must be able to reproduce the problem and understand why it is a defect without asking follow-up questions.

**Scope limit:** this skill documents bugs only. Do not diagnose root causes as fact, propose code fixes, or write code or automation scripts. If the user asks for those, say it is outside this skill and offer to continue the bug report.

## When to Use This Skill

✅ **Use when:**
- The user found a defect (during test execution or exploratory testing) and wants it documented
- The user asks to create, write, log, file, or draft a bug, defect, or issue report
- A test case failed and the user wants a bug raised for it

❌ **Don't use when:**
- The user wants to review or triage existing bugs
- The user wants a fix or code change for the problem
- The user wants a test plan or test cases (use `create-test-plan`)
- The "bug" is really a missing or unclear requirement (record it as an open question and confirm with the user before filing)

## Required Information

Collect every item below. **Check the conversation, attachments, and workspace documents first.** If an item was already provided or is clearly stated in context, use it and do not ask again. If any item is missing or unclear, ask the user (use the ask-user tool if available) before writing the report. Ask for all missing items in a single round of questions when possible.

| # | Item | Guidance |
|---|------|----------|
| 1 | **Bug title** | One line: what fails, where, and under what condition. See Title Rules. |
| 2 | **Description of the issue** | What is wrong and why it matters, in 2-5 sentences. |
| 3 | **Related test case(s)** | Unique identifiers as used by the project's test management system (for example `TC-LOGIN-004`, or a Zephyr key such as `PROJ-T123`). Ask which system is used if unclear. "None" is valid for exploratory finds. |
| 4 | **Related requirement(s)** | Unique identifiers of use cases, business rules, functional requirements, or acceptance criteria from the requirements document(s), exactly as written (for example `FR-012`, `AC-023.1`, `BR-004`). "None found" is valid; flag it so a BA can review. |
| 5 | **Impacted environment(s)** | For example Dev, QA, Staging, Production; include the URL or build/version of each environment where the bug was seen, and where it was **not** seen if known. |
| 6 | **Relevant system information** | Browser and version, OS, device, screen size or viewport, app build/version, user role or account type, locale/time zone, network conditions, as relevant. |
| 7 | **Test data used** | Exact inputs, accounts (never real passwords or real personal data), records, files, and the state they were in. |
| 8 | **Repro steps** | Numbered, human-executable steps starting from a stated precondition. |
| 9 | **Expected results** | Specific, observable behavior, based on the requirement. |
| 10 | **Actual results** | Specific, observable behavior, including exact error text. |
| 11 | **Evidence** | Screenshots, screen recordings, log files, console/network output, hyperlinks, and so on, when applicable. |

Also ask, when not already known and relevant:
- **Reproducibility:** always, intermittent (how often, for example 3 of 5 tries), or once
- **Severity** (impact on the system) and **priority** (urgency of fix); propose a value with a reason and let the user confirm. Use the definitions in the project's test plan (Defect Management section) if one exists.
- **Workaround**, if any
- **Regression:** did this work in a previous build or release?

### How to ask

- Group questions in one round; show what is already known and ask only for what is missing.
- Offer options where practical (for example environments from the test plan, severity levels).
- Never invent values. If the user does not know an item, write `Not provided` in the report and list it under **Open Items** so it can be completed later.
- If the user's description suggests the behavior may match the requirement (that is, may not be a bug), say so and confirm before filing.

## Workflow

1. **Gather context.** Read what the user provided. Search the workspace for relevant documents (requirements, design, test plan) to look up requirement IDs, test case IDs, environments, and severity definitions. Do not guess IDs; confirm them with the user when more than one could apply.
2. **Identify missing items** from the Required Information table and ask the user for them.
3. **Check for duplicates** when a bug tracker or existing bug files are available in the workspace; mention any likely duplicate and ask whether to continue.
4. **Draft the report** using the template below.
5. **Review against the checklist** and ask the user to confirm or correct anything marked `Not provided` or assumed.
6. **Confirm the output format and save location.** Ask for them if not already given (see Output).
7. **Save the file** and summarize the result.

## Writing Rules

### Title Rules

- Pattern: `<Area/Page>: <what fails> when <condition>`; for example `Registration: Submit button stays disabled when password has exactly 12 characters`.
- Be specific and searchable. No vague words ("broken", "doesn't work", "issue with"), no test case IDs, and no guesses at the cause.

### Description Rules

- State the problem and its user or business impact in plain language that a BA can understand.
- Facts only. Mark any suspected cause explicitly as a **suspicion**, in a separate sentence, or leave it out.

### Repro Step Rules

- Start with preconditions (account state, role, data that must exist, starting URL).
- One action per numbered step, written so someone unfamiliar with the application can follow it. Name the exact page, field, button, and value.
- End with the step where the problem is observed.
- Use concrete test data inline (for example enter `Passw0rd!` in Password), not "a valid password".
- Include the reproduction rate and any needed timing or sequence (for example "within 30 seconds").
- No code, scripts, or automation snippets.

### Expected vs. Actual Rules

- **Expected** is traced to a requirement ID or design statement. Quote or paraphrase it. If no requirement covers the behavior, say "Not specified in requirements" and add it to Open Items.
- **Actual** is observable and exact: quote error messages, status codes, values shown, and what did not happen.
- Do not use "works correctly", "behaves as expected", or "wrong".

### Evidence Rules

- Reference each attachment by file name or link and say what it shows (for example `login-error.png: red banner shown after step 5`).
- Redact credentials, tokens, and personal data in logs and screenshots. Tell the user if the evidence they supplied appears to contain such data.
- If the user has no evidence yet, write `Not provided` and suggest what would help (screenshot of the error, browser console log, network response).

## Bug Report Template

```markdown
# BUG: <Title>

| Field | Value |
|-------|-------|
| Bug ID | <assigned by tracker, or "TBD"> |
| Status | New |
| Reported by | <QA engineer> |
| Date reported | <date> |
| Severity | <Critical / High / Medium / Low> - <reason> |
| Priority | <High / Medium / Low> |
| Reproducibility | <Always / Intermittent (n of m) / Once> |
| Regression | <Yes (last working build) / No / Unknown> |

## 1. Description
<What is wrong and its impact.>

## 2. Traceability
| Type | Identifier(s) | Notes |
|------|---------------|-------|
| Related test case(s) | <TC-...> | <failed step, if known> |
| Related requirement(s) | <FR-..., AC-..., BR-...> | <short description> |

## 3. Environment
| Item | Value |
|------|-------|
| Impacted environment(s) | <Dev / QA / Staging / Prod - URL, build/version> |
| Not impacted (if known) | <environment(s) where it does not occur> |
| Browser / version | <...> |
| OS / device | <...> |
| Viewport / screen size | <...> |
| App build / version | <...> |
| User role / account | <...> |
| Other relevant system information | <locale, time zone, network, feature flags> |

## 4. Test Data
<Exact inputs, accounts (no real credentials), records, files, and the starting state.>

## 5. Preconditions
<State that must be true before step 1.>

## 6. Steps to Reproduce
1. <Action>
2. <Action>
3. <Action where the problem is observed>

## 7. Expected Result
<Specific, observable behavior. Source: <requirement ID / design reference>.>

## 8. Actual Result
<Specific, observable behavior, with exact messages or values.>

## 9. Evidence
| File / Link | Description |
|-------------|-------------|
| <screenshot.png> | <what it shows> |

## 10. Workaround
<Workaround, or "None known".>

## 11. Additional Notes
<Suspected cause (labeled as a suspicion), related bugs, frequency details.>

## 12. Open Items
<Anything marked "Not provided", unclear requirement coverage, or questions for BA/dev. Write "None" if empty.>
```

Omit a section only if it truly does not apply, and say so. Keep section order.

## Severity Guide (use when the project defines none)

| Severity | Meaning |
|----------|---------|
| Critical | System unusable, data loss or corruption, security breach, or core flow blocked with no workaround |
| High | Major feature fails or violates a Must requirement; a workaround is difficult |
| Medium | Feature partly fails or misbehaves; an acceptable workaround exists |
| Low | Cosmetic, minor wording or layout issue, no functional impact |

Priority reflects urgency of the fix and is set with the team; propose a value but do not present it as final.

## Output

1. **Storage location:** if the user has not said where to store the bug report, **ask them** before creating any file (use the ask-user tool if available). You may suggest a likely folder (for example a `3 - QA` folder or a `bugs` subfolder beside the test plan), but wait for the answer. If the user says they will paste it into a bug tracker, show the report in chat and do not create a file.
2. Ask what file format they want and recommend Markdown (`.md`) as the default.
3. Suggest the file name `BUG-<short-kebab-title>.md` (for example `BUG-registration-submit-disabled-12-char-password.md`), or use the tracker ID when one exists. Use the user's name if they give one.
4. Never overwrite an existing file without asking the user.
5. In chat, give a short summary: file path, title, severity/priority proposed, related test case and requirement IDs, and any **Open Items** the user still needs to supply.

## Final Checklist

Before finishing, verify:

- [ ] All 11 required items were provided by the user, found in context, or marked `Not provided` and listed under Open Items
- [ ] The title is specific, follows the pattern, and contains no guessed cause
- [ ] Related test case and requirement IDs are exactly as written in the source documents or tracker
- [ ] Environment, system information, and test data are specific enough to reproduce
- [ ] Repro steps are numbered, one action each, start from stated preconditions, and use concrete data
- [ ] Expected result is specific, observable, and traced to a requirement or design statement
- [ ] Actual result is specific, observable, and quotes exact messages or values
- [ ] Evidence is described, and credentials and personal data are redacted
- [ ] Severity and priority are proposed with a reason and confirmed by the user
- [ ] The report is understandable to a BA, a developer, and a QA without further explanation
- [ ] The save location and format were provided by the user or confirmed with them
- [ ] No code, automation scripts, or fix proposals were written
