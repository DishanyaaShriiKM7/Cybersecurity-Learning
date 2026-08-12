# Lab Notes

## Session handling observations

- Protected endpoint tested: `/my-account`
- Authenticated request with valid session: `200 OK`
- Same request without session cookie: `302 Found` → `/login`
- Same request with invalid session value: `302 Found` → `/login`
- Previously valid authenticated token saved before logout
- Browser logout performed
- Exact saved pre-logout token replayed afterward
- Replay result: `302 Found` → `/login`
- Conclusion: old authenticated session was rejected after logout

## Cookie observations

Observed response cookie attributes included:

- `Secure`
- `HttpOnly`
- `SameSite=None`

These attributes were recorded only as configuration observations, not as a complete security assessment.

## Pending tests

- Login-time session rotation
- Session fixation
- Idle / absolute expiry
- Concurrent sessions
- Token predictability
