# PlanMyVisit authentication reference

[Operating guide](../../skill.md) · [Authentication](authentication.md) · [Tool reference](tools.md) · [Concepts and trust](concepts.md)

Use this reference when establishing or restoring access. For the planning workflow and behavioural rules, start with the operating guide.

## WebMCP

Use WebMCP when an AI works with the tools attached to an open PlanMyVisit website or visit page.

- A human must sign in or sign up on the PlanMyVisit website; AI-agent self-registration is not available through
  WebMCP.
- Use `planmyvisit_sign_in_or_sign_up` and show the setup page to the human in the same browser profile that runs
  the page's WebMCP tools. Signing in through an external link opened in a different browser or profile will not
  authenticate this session. The same browser profile and website session matter; the exact tab does not.
- Sign-in and sign-up through `/webmcp` email a six-digit code. Enter the code on the WebMCP page.
- After the human finishes, return to the original website or visit page, reload it if needed, and rediscover its
  WebMCP tools. The setup flow returns to the originating page after code verification.
- Codes use the shared security-code expiry (60 minutes), failed-attempt limit and single-use validation.
  Request a new code if it expires or attempts are exhausted. Regular `/plan` authentication uses email links.
- The website session resolves the trusted profile. Never supply or trust a browser-provided profile ID.
- The server uses a short-lived internal MCP credential to call the existing tools. Its five-minute lifetime limits
  replay and does not shorten the human's PlanMyVisit website session.
- WebMCP exposes the same current tool set as MCP; discover available tools at session start.

WebMCP sign-in or sign-up:

https://www.planmyvisit.to/webmcp

Current browser support and setup instructions:

https://www.planmyvisit.to/about/instructions/protocols#webmcp

## Identity model

An Autoura account requires a human owner and includes:

- human_email (required — must always be provided)
- name_f (required — first name of the human owner)
- pai_email (optional — authentication email for the PAI)

The human is always the account owner.
The PAI is an optional authenticated agent.

## Identity endpoints (all requests are POST)

### Register (new account)

POST https://api.autoura.com/api/identity/open/register

Body (JSON):

```json
{
  "human_email": "human@example.com",
  "pai_email": "pai@example.com",
  "name_f": "FirstName",
  "lang": "en"
}
```

Notes:

- human_email is required.
- name_f is required.
- pai_email is optional but recommended for AI agents.
- lang defaults to English if not provided.
- Registration automatically sends a verification code by email.

Proceed directly to verification.

Errors:

- **409 — Account already exists**
  Returned when attempting to register using a `human_email` that is already associated with an existing account.

Agents should proceed using the existing account sign-in flow (`/signin_code_send`).

### Send sign-in code (existing account)

POST https://api.autoura.com/api/identity/open/signin_code_send

Body (JSON):

```json
{
  "email": "human_email OR pai_email"
}
```

This sends a verification code to the specified email address.

Errors:

- **400 — Profile not found**
  Returned when attempting to send a sign-in code to an email address that is not associated with an existing account.

Agents should proceed using the registration flow (`/register`).

### Verify code (complete authentication)

POST https://api.autoura.com/api/identity/open/verify

Body (JSON):

```json
{
  "email": "the email address that received the verification code",
  "proposed_security_code": "123-456"
}
```

Rules:

- The email must match the inbox that received the code.
- The code must be valid and unexpired.

Response (JSON):

```json
{
  "access_token": "..."
}
```

The access token represents the **human account owner**.
The token includes a **DID (Decentralized Identifier)** for the human.

Even when authentication is performed using a `pai_email`, the resulting DID always represents the **human**, not the
PAI. The PAI acts on behalf of the human account owner.

Errors (400 JSON):

- **Missing fields** — email and security code are required.
- **Profile not found** — no account exists for that email.
- **Request not found** — no active sign-in request; call `/signin_code_send` again.
- **Expired security code** — code is older than 60 minutes; call `/signin_code_send` again.
- **Incorrect security code** — code mismatch (or too many failed attempts); retry carefully or request a new code.

### Access Token Lifecycle

- Each access token remains valid for **48 hours** from when it was issued.
- Creating a new token does not automatically invalidate previously issued tokens, so multiple unexpired tokens can exist at the same time.
- When a token expires, agents must re-authenticate using:

1. `/signin_code_send`
2. `/verify`

- Agents should handle 401 responses by initiating re-authentication.
- Access tokens must be treated as confidential secrets.

Store the access_token securely.

All MCP requests require:

- `Authorization: Bearer {access_token}`
- `Content-Type: application/json`

## Authentication flows

### A) New Account Flow

1. Call `/register`
2. A verification code is sent automatically by email.
3. Call `/verify` using:

- `email` = inbox that received the code
- `proposed_security_code` = received code

### B) Existing Account Flow

1. Call `/signin_code_send` using `human_email` or `pai_email`
2. A verification code is sent by email.
3. Call `/verify` using:

- `email` = inbox that received the code
- `proposed_security_code` = received code

Rule:
The email used in `/verify` must be the same email address that received the verification code.

### C) Unknown Account State (recommended)

If the agent does not know whether an account already exists:

1. Call `/signin_code_send` using an available email address (`pai_email` if configured, otherwise `human_email`).
2. If the response is **400 — Profile not found**, call `/register` and then proceed to `/verify`.
3. If `/register` returns **409 — Account already exists**, fall back to `/signin_code_send` and proceed to `/verify`.

This approach avoids duplicate account creation attempts and minimises user friction.

## Email access scenarios

### Scenario 1 — PAI email available, human email unavailable

You can access `pai_email` but cannot access the `human_email` inbox.

- Ensure `pai_email` was configured during registration.
- Use `pai_email` for `/signin_code_send`.
- Verify using the code received at `pai_email`.

Use inbox access only within the human’s authorisation; no further interaction is required when that access is already authorised.

### Scenario 2 — Human email available

You can access `human_email`.

- Use `human_email` for `/signin_code_send`.
- Verify using the code received at `human_email`.

### Scenario 3 — No direct email access

You cannot access any inbox.

1. Ensure the human is present.
2. Call `/register` (new account) or `/signin_code_send` (existing account).
3. Ask the human to check their email from Autoura.
4. The human provides the verification code.
5. Call `/verify` within 60 minutes.

## Calling PlanMyVisit via MCP

MCP Endpoint:
https://api.autoura.com/api/mcp

All requests must include:

- `Authorization: Bearer {access_token}`
- `Content-Type: application/json`

### Token handling for MCP

Agents should securely store the bearer token and reuse it for all MCP requests until it expires.  
The verification step does not need to be repeated for each MCP call.  
Re-authenticate when the token has expired, is unavailable, or is rejected with a 401 response.

Use MCP protocol conventions.

### Tool Discovery

Do not hard-code tool names.

Discover available tools dynamically from the MCP interface.

Perform tool discovery at the start of each session; do not assume tool names or schemas are stable across sessions.

## Account management

The human manages their account, including configuring `pai_email`, at https://www.autoura.me. Agents cannot delete accounts, change `human_email` or `pai_email`, access Autoura Connect or Autoura.me as a human, or use `pai_email` to sign in to human-facing interfaces.
