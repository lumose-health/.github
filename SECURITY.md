# Security Policy

This is the default security policy for every repository in the GlycemicGPT organization. A repository that has its own `SECURITY.md` -- for example [android-unofficial](https://github.com/lumose-health/android-unofficial/blob/main/SECURITY.md) -- follows that policy instead.

## Reporting a Vulnerability

**Please do not open a public issue, discussion, or pull request for a security vulnerability.** Report it privately so we can investigate and ship a fix before details are public.

- Use GitHub's private vulnerability reporting on the affected repository (**Security** tab → *Report a vulnerability*) where it is enabled. For the main platform -- the backend API, web dashboard, and AI sidecar -- that is [lumose-health/GlycemicGPT](https://github.com/lumose-health/GlycemicGPT/security/advisories/new), or
- Email <security@glycemicgpt.org>. This works for every repository, including those without private vulnerability reporting.

Please include:

- The affected repository, and the version, release, or commit SHA you tested against
- A description of the vulnerability and its potential impact
- Steps to reproduce, or a proof-of-concept if you have one
- Whether it exposes health data or credentials, or changes a glucose, insulin, or treatment value a user would see

We'll acknowledge your report, investigate, and keep you updated on the fix timeline. We ask that you give us a reasonable period to address the issue before any public disclosure.

If you haven't had an acknowledgement within a few days, mention one of the security contacts below on GitHub to say you've sent a report. Don't include any details of the vulnerability there.

## Security Contacts

| Contact | Role |
|---------|------|
| [@jlengelbrecht](https://github.com/jlengelbrecht) | Project lead |
| [@seitzbg](https://github.com/seitzbg) | Maintainer, security point of contact |

## What Counts as a Vulnerability Here

GlycemicGPT is a diabetes management platform, so a wrong number shown to a person managing their diabetes is a safety issue, not just a bug. Please report these through this channel even if they don't look like a classic security bug:

- **Incorrect glucose, insulin, or treatment values** -- displayed, alerted on, or passed to the AI.
- **Authentication, session, or authorization flaws** -- including one user or caregiver reaching another user's data.
- **Exposure of health data, credentials, or integration tokens** -- at rest, in logs, or in transit.
- **Prompt injection** or any other way to make the AI give unsafe advice or get around its safety limits.
- Standard issues: dependency vulnerabilities, unsafe deserialization, and CI, workflow, or release-pipeline weaknesses.

## Supported Versions

Security fixes ship through each repository's normal release process, described in its contributing guide. Only the latest release is supported -- or, for a repository without releases, its current default branch. Please update before reporting an issue that may already be fixed.
