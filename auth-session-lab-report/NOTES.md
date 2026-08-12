# Quick Observation Log

- Login interception: successful.
- Login method: `POST`.
- Login path: `/login`.
- Body encoding: `application/x-www-form-urlencoded`.
- Credential fields observed: `username`, `password`.
- Pre-login cookie named `session`: present.
- Authenticated account request: `/my-account?id=<LAB_USER>`.
- Valid authenticated session: `200 OK` account response.
- Session removed: `302 Found`, redirected to `/login`.
- Invalid session: `302 Found`, redirected to `/login`.
- New unauthenticated session cookie attributes observed: `Secure; HttpOnly; SameSite=None`.
- Logout invalidation: not completed; no conclusion yet.
