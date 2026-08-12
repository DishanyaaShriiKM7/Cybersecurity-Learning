# Security / Evidence Handling

This repository documents testing performed against an intentionally vulnerable training application.

## Sanitization

Before publication, evidence was redacted to remove:

- complete session identifiers;
- lab-instance hostnames;
- unnecessary target-specific identifiers.

The screenshots intentionally preserve HTTP methods, paths, response status codes, redirect behavior, and cookie security attributes because those are the evidence needed to explain the test.

## Do not commit

Do not add raw Burp project files, unredacted request exports, real credentials, active session tokens, reset links, API keys, or production target information.

## Scope

The techniques described here are intended for systems you own, intentionally vulnerable labs, or systems for which you have explicit authorization to test.
