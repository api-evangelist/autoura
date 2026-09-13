# Authentication — AI Agent Integration

## Type
Reference

## Purpose
Define how AI agents authenticate with Autoura APIs and MCP.

This document covers identity roles, verification flows, and token handling.

## When to use
Use this when:

- signing in as a PAI (Personal AI)
- handling authentication errors
- managing access tokens
- integrating with MCP endpoints

## Knowledge base

This document is part of the Autoura knowledge base, used by the Autoura skill:

https://www.autoura.com/core/pai/skill.md

Refer to the skill for the full index and how documents connect.

---

## Important

- Authentication is required before calling MCP tools
- Do not attempt MCP calls without a valid access token
- Store tokens securely and reuse until expiry or failure
- Re-authenticate only when required
- If authentication fails, re-run the sign-in flow

---

## Identity roles

Two identity roles exist:

- **Human** — a human user associated with an account
- **PAI (Personal AI)** — an AI agent acting on behalf of their organisation

Accounts may contain:

- one or more humans
- one or more PAIs (typically one)

### Key differences

- Humans sign in to the **Autoura Dashboard** using username/password
- PAIs **cannot sign in to the Dashboard**
- PAIs interact only via **APIs and MCP**
- PAIs authenticate using **email-based verification (no password)**

---

## Verification code

- Format: `123-456`
- Valid for 60 minutes
- Delivered by email from Autoura

Anyone with access to the PAI email inbox can access Autoura as that PAI.  
Agents should make this clear to their human owner.

---

## PAI sign-in flow

All requests are **POST**.

### 1. Send sign-in code

**Endpoint**

    POST https://www.autoura.com/core/pai/users/signin_code_send

**Body (JSON)**

    {
      "username": "Sahra"
    }

**Behaviour**

- Sends a verification code to the email address for that user

**Errors**

- `400 — User not found`  
  Returned when attempting to send a sign-in code to a username that is not associated with an existing user.

If this happens, the username may be invalid or the user may not be eligible for this sign-in flow.

---

### 2. Verify sign-in code

**Endpoint**

    POST https://www.autoura.com/core/pai/users/verify

**Body (JSON)**

    {
      "username": "Sahra",
      "proposed_security_code": "123-456"
    }

**Response (JSON)**

    {
      "access_token": "TOKEN",
      "expires_in": 604800
    }

---

## Rules

- The username used in `/verify` must match the user for whom the verification code was requested
- The verification code must be valid and unexpired

**Errors**

- Missing fields — username and security code are required
- User not found — no user exists for that username
- Request not found — no active sign-in request; call `/signin_code_send` again
- Expired security code — request a new code with `/signin_code_send`
- Incorrect security code — code mismatch; retry carefully or request a new code

---

## Access token lifecycle

Each access token remains valid for **7 days** from when it was issued.

Creating a new token does **not** automatically invalidate previously issued tokens, so multiple unexpired tokens can exist at the same time.

When a token expires, the agent must sign in again using:

    /signin_code_send
    /verify

Agents should:

- treat access tokens as confidential secrets
- securely store the bearer token
- reuse the token for MCP requests until it expires
- handle authentication expiry cleanly

---

## MCP authentication

**MCP endpoint**

    https://www.autoura.com/core/pai/mcp

All MCP requests must include:

    Authorization: Bearer {access_token}
    Content-Type: application/json

Agents must include the required headers on every request and ensure the access token is valid.

### Token handling for MCP

Agents should securely store the bearer token and reuse it for all MCP requests until it expires.  
The verification step does not need to be repeated for each MCP call.  
Re-authentication is only required when the token has expired or is unavailable.