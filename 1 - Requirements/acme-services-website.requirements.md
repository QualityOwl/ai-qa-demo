# Requirements Document: Acme Home Services Website

| Field | Value |
|-------|-------|
| Document ID | REQ-ACME-WEB-001 |
| Status | Draft for review |
| Owner | Product Owner (Acme Home Services) |
| Audience | Developers, QA, UX, Stakeholders |

---

## 1. Overview

Acme Home Services is a small business that offers plumbing and electrical repair. It needs a basic public website so customers can learn about services, create an account, sign in, contact the business, and search the site.

### 1.1 Goals

- Let visitors register for and sign in to a personal account.
- Let visitors send an inquiry to the business.
- Let visitors find content quickly through search.

### 1.2 In Scope

- Home page and global navigation
- User registration
- User login / logout
- Password reset
- Contact form
- Site search

### 1.3 Out of Scope

- Online payments or booking
- Admin portal / content management
- Social login (Google, Facebook, etc.)
- Native mobile apps
- Multi-language support

### 1.4 Users

| Role | Description |
|------|-------------|
| Visitor | Anonymous user, not signed in |
| Registered User | Visitor who has created an account and signed in |

### 1.5 Assumptions

- A-1: Email is sent by an existing, already-tested email service.
- A-2: Site content (services, about text) is supplied by the business before launch.
- A-3: Users have JavaScript enabled.

---

## 2. Global Navigation and Home Page

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-001 | The header on every page shall display the company logo, and links to **Home**, **Services**, **Contact**, and a **Search** box. | Must |
| FR-002 | When the user is not signed in, the header shall show **Log In** and **Register** links. When signed in, it shall show "Hello, *first name*" and a **Log Out** link instead. | Must |
| FR-003 | Clicking the logo shall navigate to the Home page. | Must |
| FR-004 | The footer on every page shall show the business phone number, email address, and copyright year. | Should |

**Acceptance criteria**
- AC-001.1: All header links navigate to the correct page with no broken links.
- AC-002.1: After login, **Log In** and **Register** are no longer visible.
- AC-002.2: A first name of up to 50 characters is displayed without breaking the layout.

---

## 3. User Registration

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-010 | The registration form shall collect: First Name, Last Name, Email, Password, Confirm Password. All fields are required. | Must |
| FR-011 | Email shall be a valid email format and unique across all accounts. | Must |
| FR-012 | Password shall be 8 to 64 characters and include at least one uppercase letter, one lowercase letter, one digit, and one special character. | Must |
| FR-013 | Confirm Password shall match Password. | Must |
| FR-014 | On successful registration, the system shall create the account, send a confirmation email, and sign the user in. | Must |
| FR-015 | If the email is already registered, the system shall show an error message and shall not create a duplicate account. | Must |
| FR-016 | Validation errors shall be shown next to the field that caused them, and previously entered values (except passwords) shall be retained. | Should |

**Acceptance criteria**
- AC-010.1: Submitting with any required field blank shows a "required" message for that field.
- AC-011.1: `user@` and `user.example.com` are rejected; `user@example.com` is accepted.
- AC-012.1: `Passw0rd!` is accepted; `password` and `Pass1!` are rejected.
- AC-014.1: The confirmation email contains the user's first name.

---

## 4. Login and Logout

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-020 | The login form shall accept Email and Password. | Must |
| FR-021 | On valid credentials, the user shall be signed in and redirected to the Home page. | Must |
| FR-022 | On invalid credentials, the system shall show a generic message ("Email or password is incorrect") that does not reveal which value was wrong. | Must |
| FR-023 | After 5 consecutive failed login attempts for the same account, the account shall be locked for 15 minutes. | Must |
| FR-024 | A signed-in user who selects **Log Out** shall be signed out and returned to the Home page. | Must |
| FR-025 | A user session shall expire after a period of inactivity. | Must |
| FR-026 | Passwords shall never be displayed in clear text, except while the user toggles a "Show password" control. | Should |

**Acceptance criteria**
- AC-023.1: The 5th consecutive failure shows a lockout message; a correct password during lockout is still rejected.
- AC-024.1: After logout, pressing the browser Back button does not show account-specific content.

---

## 5. Password Reset

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-030 | The login page shall provide a **Forgot password?** link that opens a form requesting the user's email. | Must |
| FR-031 | The system shall always show the same confirmation message ("If an account exists, a reset link has been sent") whether or not the email is registered. | Must |
| FR-032 | The reset link shall be single-use and expire 60 minutes after it is issued. | Must |
| FR-033 | The new password shall meet the same rules as FR-012. | Must |

---

## 6. Contact Form

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-040 | The contact form shall collect: Name (required), Email (required), Phone (optional), Subject (required, selected from a list), Message (required, max 1,000 characters). | Must |
| FR-041 | The Subject list shall contain: General Question, Request a Quote, Report a Problem. | Must |
| FR-042 | On successful submission, the system shall display a confirmation message and send the inquiry to the business inbox. | Must |
| FR-043 | The form shall display a live character counter for the Message field. | Could |
| FR-044 | The form shall be protected against automated (bot) submissions. | Must |
| FR-045 | Phone, when provided, shall be a valid phone number. | Should |

**Acceptance criteria**
- AC-040.1: A message of exactly 1,000 characters is accepted; 1,001 characters is rejected.
- AC-042.1: The user sees the confirmation on screen within 3 seconds of submitting.

---

## 7. Site Search

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-050 | The search box shall accept text and submit on Enter key or on clicking the search icon. | Must |
| FR-051 | Search shall return matching pages from the website's public content, 10 results per page, with pagination. | Must |
| FR-052 | Each result shall display the page title, a short excerpt, and a link to the page. | Must |
| FR-053 | When there are no matches, the system shall display "No results found" and a suggestion to contact the business. | Must |
| FR-054 | Search shall be case-insensitive. | Must |
| FR-055 | Search input shall be sanitized so that HTML or script content is never executed or rendered. | Must |

**Acceptance criteria**
- AC-051.1: A search matching 25 pages shows 10, 10, and 5 results across 3 pages.
- AC-055.1: Searching `<script>alert(1)</script>` displays the text safely and runs no script.

---

## 8. Non-Functional Requirements

| ID | Category | Requirement |
|----|----------|-------------|
| NFR-001 | Performance | Pages shall load in under 3 seconds on a standard broadband connection under normal load. |
| NFR-002 | Compatibility | The site shall work on the latest two versions of Chrome, Edge, Firefox, and Safari. |
| NFR-003 | Responsive design | The site shall be usable on screen widths from 360 px (mobile) to 1920 px (desktop). |
| NFR-004 | Accessibility | The site shall meet WCAG 2.1 Level AA, including keyboard navigation, visible focus, labels on all form fields, and sufficient color contrast. |
| NFR-005 | Security | All traffic shall use HTTPS. Passwords shall be stored hashed and salted, never in plain text. |
| NFR-006 | Security | The site shall be protected against common web vulnerabilities (e.g., OWASP Top 10 injection and XSS). |
| NFR-007 | Privacy | Personal data (name, email, phone) shall not be written to application logs. |
| NFR-008 | Reliability | The site shall be available 99.5% of the time, excluding planned maintenance. |

---

## 9. Open Questions

> Some of these are deliberately left unanswered for training. A good test plan calls out how each unresolved item affects test design and risk.

| # | Question | Related Req |
|---|----------|-------------|
| Q-1 | How long is the inactivity timeout for a session? (Requirement says "a period of inactivity" with no value.) | FR-025 |
| Q-2 | Must users verify their email before they can sign in? FR-014 signs them in immediately. | FR-014 |
| Q-3 | Is the lockout counter reset after a successful login or after the 15 minutes? | FR-023 |
| Q-4 | Which phone formats are valid (US only, international)? | FR-045 |
| Q-5 | Which content does search index (all pages, or only Services and Contact)? Are signed-in-only pages included? | FR-051 |
| Q-6 | What does "normal load" mean for performance (concurrent users)? | NFR-001 |
| Q-7 | What bot-protection mechanism is used (CAPTCHA, honeypot, rate limit)? | FR-044 |
| Q-8 | What happens to the contact form data if the email service is unavailable? | FR-042 |
| Q-9 | Are there any restrictions on non-ASCII characters in names (e.g., "José", "O'Brien")? | FR-010 |
| Q-10 | Is the Services page content in scope for test? It is listed in navigation but has no requirements. | FR-001 |

---

## 10. Glossary

| Term | Definition |
|------|------------|
| Account lockout | Temporary block on sign-in after repeated failed attempts |
| Pagination | Splitting results across multiple pages |
| WCAG | Web Content Accessibility Guidelines |
| XSS | Cross-site scripting, a security attack that injects script into a page |
