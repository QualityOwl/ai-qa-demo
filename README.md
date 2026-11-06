# AI QA Demo

A practice repository for QA professionals who are new to AI tools, or who just want a small space for no-risk practice. 

Use AI to do things like understand requirements, write tests, and review test plans.

The example in this repository is the `Acme Home Services` website. This repo contains documents only, not application code or a working website.

## Repository Contents

Start with these three documents:

1. **Requirements:** [Website Requirements](1%20-%20Requirements/acme-services-website.requirements.md) explains what the website should do, what counts as success, and which questions remain open.
2. **Development:** [Technical Design](2%20-%20Development/acme-services-website.code-design.md) explains the proposed .NET structure, data storage, pages, feature rules, and security controls.
3. **QA:** [Test Plan](3%20-%20QA/acme-services-website.test-plan.md) provides tests to review. It has intentional gaps and mistakes for training and must not be used for real testing.

## Suggested Workflow

1. Read the requirements. Note anything missing, unclear, or hard to test.
2. Compare the design with the requirements. Look for answered questions and conflicting rules.
3. Review the test plan. Check for missing tests, wrong expected results, clear steps, and links to the correct requirements. Do not assume its coverage claims are correct.
4. Ask AI to help with one task at a time. Check its answers against the documents.

## Example Prompts

- "What questions must be answered before we can write tests for these requirements?"
- "Create a test plan with valid, invalid, and boundary test cases."
- "Compare the test plan with the requirements and design. Find missing tests, wrong expected results, and incorrect requirement links."
- "Find conflicting password and reset-link rules across the three documents. Do not choose which rule is correct."
- "Suggest tests for the contact form's required fields, message limit, and submission."

## Example Product Scope

The requirements cover navigation, the home page, registration, login and logout, password reset, the contact form, and site search. They also cover accessibility, browser support, mobile and desktop layouts, speed, security, privacy, and availability.

Payments, booking, an admin portal, social login, native mobile apps, and multiple languages are out of scope.

Some questions are deliberately left open so you can practice identifying risks and asking for clarification.
