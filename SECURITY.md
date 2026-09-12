# Security Policy

`pii-protector` is a privacy/security tool, so we take reports seriously.

## Reporting a vulnerability

**Please do not open a public issue for security or privacy vulnerabilities.**

Instead, report privately using one of these channels:

- GitHub's [private vulnerability reporting](https://github.com/sjain26/pii-protector/security/advisories/new) (preferred), or
- Email: **jainsatyam26@gmail.com** with the subject line `SECURITY: pii-protector`

Please include:
- A description of the issue and its impact
- Steps to reproduce (with **synthetic** data only — never real PII)
- The version affected

You can expect an initial acknowledgement within a few days. Please give a reasonable window for a fix before any public disclosure.

## Scope examples

- A crafted input that causes a crash or hang (ReDoS, etc.)
- A detection failure that could lead to PII leakage in a downstream system
- Dependency vulnerabilities affecting the package

## Out of scope

- Missed detections on ordinary text — please file those as a normal [bug report](https://github.com/sjain26/pii-protector/issues/new?template=bug_report.yml)

Thank you for helping keep users' data safe. 🔒
