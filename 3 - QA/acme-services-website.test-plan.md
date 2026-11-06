# Test Plan: Acme Home Services Website

> **TRAINING EXAMPLE ONLY.** This plan is intentionally incomplete and contains deliberate flaws. It is for a QA training class on test plan review. Do not use it for real testing.

| Field | Value |
|-------|-------|
| Test Plan ID | TP-ACME-1 |
| Based on | REQ-ACME-WEB-001 |
| Author | QA Team |
| Status | Final |
| Date | 2026-11-05 |

## 1. Introduction

This test plan describes the testing of the Acme Home Services website. The site lets customers register, log in, contact the business, and search.

## 2. Scope

### 2.1 In Scope

- Registration
- Login / logout
- Password reset
- Contact form
- Site search
- Non-functional requirements

### 2.2 Out of Scope

- Payments
- Admin portal
- Mobile apps

## 3. Test Approach

Testers will run the test cases manually and check that the site works. Defects will be reported when found.

## 4. Test Environment and Tools

- Test environment
- Chrome and Edge
- Test accounts

## 5. Test Data

Use valid and invalid data as needed.

## 6. Entry and Exit Criteria

### 6.1 Entry Criteria

- Build is deployed.

### 6.2 Exit Criteria

- All tests pass.

### 6.3 Suspension / Resumption Criteria

Not applicable.

## 7. Manual Test Cases

### 7.1 Registration

| ID | Type | Requirement(s) | Title | Preconditions | Steps | Test data | Expected result | Priority |
|----|------|----------------|-------|---------------|-------|-----------|-----------------|----------|
| TC-REG-001 | Positive | FR-010, FR-014 | Register a new user | User is signed out | 1. Open the Register page<br>2. Fill in the form<br>3. Click Register | Valid data | Registration works correctly | High |
| TC-REG-002 | Negative | FR-010 | Blank required field | User is signed out | 1. Open the Register page<br>2. Leave First Name blank<br>3. Fill in the other fields<br>4. Click Register | Valid data except First Name | A "required" message is shown for First Name | High |
| TC-REG-003 | Negative | FR-011 | Invalid email format | User is signed out | 1. Open the Register page<br>2. Enter an invalid email<br>3. Click Register | `user@` | Email is rejected | High |
| TC-REG-004 | Negative | FR-015 | Duplicate email | An account exists for `user@example.com` | 1. Open the Register page<br>2. Enter `user@example.com`<br>3. Click Register | `user@example.com` | Error message is shown | High |
| TC-REG-005 | Positive | FR-012 | Valid password accepted | User is signed out | 1. Open the Register page<br>2. Enter the password<br>3. Click Register | `Passw0rd!` | Password is accepted | High |
| TC-REG-006 | Negative | FR-012 | Password with no digit | User is signed out | 1. Open the Register page<br>2. Enter the password<br>3. Click Register | `password` | Password is rejected | High |
| TC-REG-007 | Edge | FR-012 | Maximum password length | User is signed out | 1. Open the Register page<br>2. Enter a 128-character password that meets all rules<br>3. Click Register | 128-character password | Account is created | Medium |
| TC-REG-008 | Negative | FR-013 | Passwords do not match | User is signed out | 1. Open the Register page<br>2. Enter different values in Password and Confirm Password<br>3. Click Register | `Passw0rd!` / `Passw0rd?` | Error is displayed | High |

### 7.2 Login and Logout

| ID | Type | Requirement(s) | Title | Preconditions | Steps | Test data | Expected result | Priority |
|----|------|----------------|-------|---------------|-------|-----------|-----------------|----------|
| TC-LOGIN-001 | Positive | FR-020, FR-021 | Log in with valid credentials | Registered account exists | 1. Open the Log In page<br>2. Enter email and password<br>3. Click Log In | Valid credentials | User is logged in | High |
| TC-LOGIN-002 | Negative | FR-022 | Log in with wrong password | Registered account exists | 1. Open the Log In page<br>2. Enter a wrong password<br>3. Click Log In | Wrong password | An error is shown | High |
| TC-LOGIN-003 | Negative | FR-023 | Account lockout | Registered account exists | 1. Enter a wrong password 3 times<br>2. Try to log in again | Wrong password | Account is locked | High |
| TC-LOGIN-004 | Positive | FR-024 | Log out | User is logged in | 1. Click Log Out | None | User is logged out | High |
| TC-LOGIN-005 | Negative | AC-024.1 | Back button after logout | User has just logged out | 1. Click the browser Back button | None | Account pages are not visible | Medium |
| TC-LOGIN-006 | Positive | FR-021 | Log in redirects to Home | Registered account exists | 1. Open the Log In page<br>2. Enter valid credentials<br>3. Click Log In | Valid credentials | User is logged in | High |

### 7.3 Password Reset

| ID | Type | Requirement(s) | Title | Preconditions | Steps | Test data | Expected result | Priority |
|----|------|----------------|-------|---------------|-------|-----------|-----------------|----------|
| TC-RESET-001 | Positive | FR-030, FR-031 | Request a reset link | Registered account exists | 1. Click Forgot password?<br>2. Enter the email<br>3. Submit | Registered email | Confirmation message is shown and an email arrives | High |
| TC-RESET-002 | Positive | FR-033 | Set a new password | User has a reset link | 1. Open the reset link<br>2. Enter a new password<br>3. Submit | `NewPassw0rd!` | Password is changed | High |
| TC-RESET-003 | Edge | FR-032 | Reset link expiry | User has a reset link issued 24 hours ago | 1. Open the reset link | Expired link | Link is rejected | High |

### 7.4 Contact Form

| ID | Type | Requirement(s) | Title | Preconditions | Steps | Test data | Expected result | Priority |
|----|------|----------------|-------|---------------|-------|-----------|-----------------|----------|
| TC-CONTACT-001 | Positive | FR-040, FR-041, FR-042 | Submit the contact form | None | 1. Open the Contact page<br>2. Fill in all fields<br>3. Click Send | Valid data | Confirmation is shown | High |
| TC-CONTACT-002 | Negative | FR-040 | Missing required field | None | 1. Open the Contact page<br>2. Leave Name blank<br>3. Click Send | Name blank | Form is not submitted | High |
| TC-CONTACT-003 | Edge | AC-040.1 | Message at limit | None | 1. Open the Contact page<br>2. Enter a 1,000-character message<br>3. Click Send | 1,000 characters | Message is accepted | Medium |
| TC-CONTACT-004 | Edge | AC-040.1 | Message over limit | None | 1. Open the Contact page<br>2. Enter a 1,001-character message<br>3. Click Send | 1,001 characters | Message is rejected | Medium |

### 7.5 Site Search

| ID | Type | Requirement(s) | Title | Preconditions | Steps | Test data | Expected result | Priority |
|----|------|----------------|-------|---------------|-------|-----------|-----------------|----------|
| TC-SEARCH-001 | Positive | FR-050, FR-051 | Search by pressing Enter | None | 1. Type a word in the Search box<br>2. Press Enter | `plumbing` | Results are displayed | High |
| TC-SEARCH-002 | Positive | FR-052 | Result content | None | 1. Search for a word<br>2. Look at the results | `electrical` | Each result looks right | Medium |
| TC-SEARCH-003 | Negative | FR-055 | Script input | None | 1. Type the text in the Search box<br>2. Press Enter | `<script>alert(1)</script>` | No script runs | High |
| TC-SEARCH-004 | Edge | AC-051.1 | Pagination | 25 pages match the term | 1. Search for the term<br>2. Go through the result pages | Term matching 25 pages | 10, 10, and 5 results are shown | Medium |
| TC-SEARCH-005 | Positive | FR-099 | Search is case-insensitive | None | 1. Search for `PLUMBING`<br>2. Search for `plumbing` | `PLUMBING`, `plumbing` | Same results appear | Medium |

## 8. Non-Functional Manual Test Cases

| ID | Type | Requirement(s) | Title | Preconditions | Steps | Test data | Expected result | Priority |
|----|------|----------------|-------|---------------|-------|-----------|-----------------|----------|
| TC-NFR-001 | Positive | NFR-001 | Page load time | None | 1. Open the Home page | None | Page loads quickly | Medium |
| TC-NFR-002 | Positive | NFR-002 | Browser compatibility | None | 1. Open the site in Chrome<br>2. Open the site in Edge | None | Site works | Medium |
| TC-NFR-003 | Positive | NFR-005 | HTTPS | None | 1. Open the site | `http://` URL | Site uses HTTPS | High |

## 9. Requirements Traceability Matrix

| Requirement | Description (short) | Scenario IDs | Coverage |
|-------------|---------------------|--------------|----------|
| FR-001 | Header links | None | Covered |
| FR-002 | Header signed in / out | TC-LOGIN-001 | Covered |
| FR-003 | Logo goes Home | None | Covered |
| FR-010 | Registration fields | TC-REG-001, TC-REG-002 | Covered |
| FR-011 | Email format and unique | TC-REG-003 | Covered |
| FR-012 | Password rules | TC-REG-005, TC-REG-006, TC-REG-007 | Covered |
| FR-013 | Confirm password | TC-REG-008 | Covered |
| FR-014 | Account created and signed in | TC-REG-001 | Covered |
| FR-015 | Duplicate email | TC-REG-004 | Covered |
| FR-020 | Login form | TC-LOGIN-001 | Covered |
| FR-021 | Login redirect | TC-LOGIN-001, TC-LOGIN-006 | Covered |
| FR-022 | Generic error | TC-LOGIN-002 | Covered |
| FR-023 | Lockout | TC-LOGIN-003 | Covered |
| FR-024 | Logout | TC-LOGIN-004 | Covered |
| FR-025 | Session expiry | None | Covered |
| FR-030 | Forgot password link | TC-RESET-001 | Covered |
| FR-031 | Same message | TC-RESET-001 | Covered |
| FR-032 | Reset link expiry | TC-RESET-003 | Covered |
| FR-033 | New password rules | TC-RESET-002 | Covered |
| FR-040 | Contact fields | TC-CONTACT-001, TC-CONTACT-002 | Covered |
| FR-041 | Subject list | TC-CONTACT-001 | Covered |
| FR-042 | Confirmation and email | TC-CONTACT-001 | Covered |
| FR-044 | Bot protection | None | Covered |
| FR-050 | Search submit | TC-SEARCH-001 | Covered |
| FR-051 | 10 results per page | TC-SEARCH-004 | Covered |
| FR-052 | Result content | TC-SEARCH-002 | Covered |
| FR-054 | Case-insensitive | TC-SEARCH-005 | Covered |
| FR-055 | Sanitized input | TC-SEARCH-003 | Covered |
| NFR-001 | Page load | TC-NFR-001 | Covered |
| NFR-002 | Browsers | TC-NFR-002 | Covered |
| NFR-005 | HTTPS and hashing | TC-NFR-003 | Covered |

## 11. Open Questions and Clarifications Needed

- What is the session timeout?
- Which phone formats are valid?

## 12. Risks and Assumptions

- Risk: schedule may be tight.
- Assumption: the requirements are correct.

## 13. Defect Management

Defects will be logged in the defect tracker with a severity of High, Medium, or Low and fixed by developers.

## 14. Deliverables and Schedule

- Test plan
- Test results
