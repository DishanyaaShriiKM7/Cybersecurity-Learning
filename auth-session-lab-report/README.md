# Authentication & Session Handling — Burp Suite Lab Notes

> Educational security testing performed against an intentionally vulnerable Web Security Academy lab. Sensitive values in evidence images have been redacted.

## Objective

Map the basic authentication flow and experimentally determine how the application maintains authenticated state:

`Login → Request → Cookie → Session → Logout`

This session focused on login interception, session-cookie behavior, and controlled comparisons in Burp Repeater. Logout invalidation was planned but **not completed** during this session.

## Environment

- Burp Suite Community Edition
- Burp Proxy / HTTP history / Repeater
- Intentionally vulnerable Web Security Academy lab
- Test account supplied by the lab

## Methodology

1. Intercepted the login request in Burp Proxy.
2. Identified the credential parameters and pre-login `session` cookie.
3. Forwarded the valid login and captured an authenticated `/my-account` request.
4. Sent the authenticated request to Repeater as the control.
5. Removed the session cookie and compared the response.
6. Replaced the cookie with an invalid value and compared the response.
7. Inspected cookie attributes returned to an unauthenticated client.

## Observations

### 1. Login request

The application submitted credentials using a `POST` request to `/login` with `application/x-www-form-urlencoded` data. Burp Inspector identified two request-body parameters: `username` and `password`.

A `session` cookie was already present before authentication. This demonstrates that possession of a cookie named `session` does not itself imply an authenticated user; it can represent anonymous/pre-authentication state.

### 2. Authenticated request

After successful authentication, the account page was requested using a request conceptually equivalent to:

```http
GET /my-account?id=<LAB_USER> HTTP/2
Host: <REDACTED_LAB_HOST>
Cookie: session=<REDACTED_AUTHENTICATED_SESSION>
```

With the valid authenticated session, the server returned `HTTP/2 200 OK` and the account page.

### 3. Session cookie removed

The same account request was replayed with the `Cookie` header removed while leaving the account query parameter unchanged.

Observed response:

```http
HTTP/2 302 Found
Location: /login
Set-Cookie: session=<REDACTED_NEW_SESSION>; Secure; HttpOnly; SameSite=None
```

This demonstrates that the account identifier in the URL was not sufficient to authenticate the request. The valid session state was required.

### 4. Invalid session value

The request was replayed with a deliberately invalid session value:

```http
Cookie: session=<INVALID_TEST_VALUE>
```

The application again returned `302 Found` with `Location: /login` and issued a new session cookie. The arbitrary cookie value was therefore not accepted as authenticated state.

### 5. Cookie attributes observed

For the newly issued unauthenticated session, the response included:

- `Secure`
- `HttpOnly`
- `SameSite=None`

These attributes protect specific cookie properties but do not, by themselves, establish correct authentication or session lifecycle behavior.

## Test matrix

| Test | Session state | Result | Interpretation |
|---|---|---|---|
| Control | Valid authenticated session | `200 OK` | Authenticated session accepted |
| Cookie removed | No session cookie | `302 → /login` | Authentication state absent |
| Invalid cookie | Arbitrary invalid value | `302 → /login` | Invalid token rejected |

## Key learning

The lab demonstrated the separation between a resource/account selector and authentication state. A parameter such as `id=<user>` indicates the resource being requested; the server's acceptance of the session token is what maintained authenticated access in the tested flow.

It also demonstrated a useful testing pattern:

`Control request → change one variable → resend → compare response`

This minimizes ambiguity when investigating authentication behavior.

## Not yet tested

The following items should **not** be claimed as findings from this session because they were not fully tested:

- Session rotation from pre-login to post-login
- Logout/session invalidation
- Idle or absolute expiration
- Concurrent sessions
- Session token entropy/predictability
- Session fixation
- Authorization behavior for another user's resources

## Next session

1. Record pre-login and post-login session identifiers and confirm whether rotation occurs.
2. Save a valid authenticated request in Repeater.
3. Log out normally in the browser.
4. Replay the old authenticated session and verify server-side invalidation.
5. Document the logout request/response and any cookie deletion behavior.

## Evidence

Redacted screenshots are stored in [`evidence/`](evidence/). They are intentionally sanitized so the repository does not publish session tokens, credentials, or unique lab host identifiers.

## Scope & ethics

These notes document testing of an intentionally vulnerable training application. Techniques should only be used on systems you own or have explicit authorization to test.
