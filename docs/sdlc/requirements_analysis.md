# Requirements analysis: Customer password reset

**Stage:** `requirements_analysis` · **Schema version:** `1.0`

The customer wants to reset their password under the curated recovery standard. A reset request is distinct from a successfully completed password reset. The source references below apply to the requirements as mapped; both identical knowledge results are retained rather than deduplicated.

## Source references

| Key | Exact source reference | Evidence |
| --- | --- | --- |
| S1 | `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}` | “As a customer I want to reset my password. Apply the curated recovery standard.” |
| K1 | `{"source_type":"knowledge","source_id":"59efeb19-83f2-4ec6-bfd3-b4aeb7142930","locator":"v1, chunk 0"}` | Account recovery standard v1, ACC-STD-01 through ACC-STD-03. |
| K2 | `{"source_type":"knowledge","source_id":"467a71ce-c5f8-4557-8f1e-a8ff1b01ecc3","locator":"v1, chunk 0"}` | Identical account recovery standard v1, ACC-STD-01 through ACC-STD-03. |

## Requirements and acceptance criteria

| ID | Statement | Given/When/Then acceptance criterion | Standard mapping | Source references |
| --- | --- | --- | --- | --- |
| REQ-001 | The sign-in page offers a “Forgot password?” link that starts a password reset request. | Given a customer is on the sign-in page, when they select “Forgot password?”, then a password reset request starts. | ACC-STD-01 | S1, K1, K2 |
| REQ-002 | A password reset request receives the same response whether or not the account exists. | Given otherwise equivalent reset requests for an existing and a nonexistent account, when each request is submitted, then their responses are identical. | ACC-STD-01 | S1, K1, K2 |
| REQ-003 | Password reset requests are limited to five per account per hour. | Given five reset requests have been allowed for an account within an hour, when another is attempted for that account within the same hour, then no sixth reset request is allowed. | ACC-STD-01 | S1, K1, K2 |
| REQ-004 | A reset link can complete at most one password reset. | Given a reset link has already been used successfully, when it is used again, then it does not complete another reset. | ACC-STD-02 | S1, K1, K2 |
| REQ-005 | A reset link expires 30 minutes after issuance. | Given a reset link was issued, when its use is attempted 30 minutes or more after issuance, then it cannot complete a reset. | ACC-STD-02 | S1, K1, K2 |
| REQ-006 | A customer can complete a password reset using a valid reset link. | Given a customer has an unused, unexpired reset link, when the customer completes the reset, then the account password is reset. | ACC-STD-02 and S1 | S1, K1, K2 |
| REQ-007 | Completing a password reset signs the account out of every active session. | Given an account has active sessions, when its password reset completes successfully, then all those sessions are signed out. | ACC-STD-02 | S1, K1, K2 |
| REQ-008 | The account owner is notified after every successful password reset. | Given an account's password reset succeeds, when the reset has completed, then the account owner is notified after that reset. | ACC-STD-03 | S1, K1, K2 |

## Assumptions

1. acceptance-20261025221654-2fb546-skill-sentinel — the acceptance requirements skill produced this output.
2. Specify the reset request, reset link, and owner notification outcomes without assuming a delivery channel.
3. Require a completed password change without adding password policy rules.
4. Use a valid, unused, unexpired reset link as the stated completion mechanism; do not prescribe additional identity checks. Initiating a request does not itself complete a reset.
5. Test the clearly over-limit sixth request within an hour without stipulating a fixed or rolling window or a rate-limit response text or status.
6. Require a notification after each successful reset without specifying its wording.

## Non-blocking open questions

| Question | Blocking? | Assumption used |
| --- | --- | --- |
| What delivery channel carries the reset link and owner notification? | No | Specify the reset request, reset link, and owner notification outcomes without assuming a delivery channel. |
| What password policy applies to a new password? | No | Require a completed password change without adding password policy rules. |
| Is identity verification required beyond the reset link? | No | Use a valid, unused, unexpired reset link as the stated completion mechanism; do not prescribe additional identity checks. Initiating a request does not itself complete a reset. |
| What is the precise hour-window boundary and response to a rate-limited request? | No | Test the clearly over-limit sixth request within an hour without stipulating a fixed or rolling window or a rate-limit response text or status. |
| What wording must the owner notification contain? | No | Require a notification after each successful reset without specifying its wording. |
