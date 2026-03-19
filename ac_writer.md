# Acceptance Criteria Writer — Password Reset via Email
**Story submitted:** 2026-03-19

---

## USER STORY

As a **registered user who has been locked out of their account**,
I want to **request a password reset link from the login page by entering my email**,
so that **I can regain access to my account without contacting support**.

---

## ACCEPTANCE CRITERIA

- The user can see a "Forgot your password?" link on the login page, visible without scrolling.
- The user can click the link and be taken to a dedicated password reset request page.
- The user can enter their email address into a single input field on that page.
- The user can submit the form by clicking a clearly labelled "Send reset link" button.
- The system sends a password reset email to the entered address within 60 seconds of submission.
- The reset email contains a single-use link that directs the user to a password reset form.
- The reset link expires after 60 minutes from the time it was generated.
- When the user clicks an expired link, the system displays a clear error message and offers the option to request a new link.
- When the user clicks a valid reset link, they are taken to a page where they can enter and confirm a new password.
- The system enforces the existing password strength rules on the new password field.
- When the new password is successfully saved, the system invalidates the reset link so it cannot be reused.
- When the new password is successfully saved, the user is redirected to the login page with a confirmation message.
- The system logs the password reset request event (timestamp, email, IP address) for security auditing.

---

## EDGE CASES

**1. Email address is not registered in the system**
- The system displays the same success message as a valid submission ("If this email is registered, you'll receive a reset link shortly.")
- No email is sent. No information is leaked about whether the account exists.

**2. User submits the reset form multiple times in quick succession**
- The system rate-limits requests to a maximum of 3 reset emails per email address per hour.
- On exceeding the limit, the system displays a message: "Too many requests. Please try again in [X] minutes."

**3. User clicks the reset link after already using it**
- The system displays an error: "This link has already been used. Please request a new one."
- The user is offered a link back to the reset request page.

**4. User has multiple active reset links (requested more than once)**
- Only the most recently generated link is valid.
- All previously issued links for that email are invalidated when a new one is generated.

**5. User enters mismatched passwords on the reset form**
- The system displays an inline validation error before submission: "Passwords do not match."
- The form cannot be submitted until the fields match.

**6. User's email client does not render HTML emails**
- The reset link is also included as plain text in the email body.

---

## DEFINITION OF DONE

- [ ] "Forgot your password?" link is visible on the login page on both desktop and mobile
- [ ] Reset request page renders correctly on mobile, tablet, and desktop
- [ ] Password reset email is delivered within 60 seconds in staging environment
- [ ] Reset link expires correctly after 60 minutes (verified with a test)
- [ ] Expired link shows correct error message and re-request option
- [ ] Used link cannot be reused (verified with a test)
- [ ] Multiple active links: only the latest one works (verified with a test)
- [ ] Rate limiting is active and returns correct error after 3 attempts
- [ ] Unregistered email returns same success message as registered email
- [ ] New password is subject to existing password strength validation
- [ ] Password reset event is logged with timestamp, email, and IP
- [ ] Reset flow tested end-to-end in staging by PM and QA
- [ ] No console errors or broken states in any edge case scenario

---

## WHAT IS OUT OF SCOPE

1. **Password reset via SMS or phone number.** This story covers email only. SMS-based reset is a separate feature requiring phone number collection and a third-party SMS provider.

2. **Forcing a password reset on first login or after admin action.** This story is user-initiated only. Admin-triggered resets (e.g., after a security event) are out of scope and require a separate flow.

3. **Changing a password while already logged in.** That is handled by the account settings flow. This story covers only the unauthenticated, login-page-initiated reset path.
