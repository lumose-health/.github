# 🤝 Contributing to GlycemicGPT

Thanks for your interest in contributing to GlycemicGPT! Whether you're fixing a typo, squashing a bug, or building a whole new feature -- we appreciate you. 💙

This is the **org-wide master contributing guide**. It covers the policy and workflow that apply to *every* repository in the GlycemicGPT organization: safety rules, roles, the branching model, commit conventions, the pull request process, and our attribution and security posture.

**For build, test, and setup instructions, see the CONTRIBUTING.md in the specific repository you're working on** -- the per-repo guides cover the mechanics (how to run the stack, which commands to run, code style for each language). This master covers the rules that don't change from repo to repo.

---

## ⚠️ Safety First -- Please Read

**GlycemicGPT interacts with real diabetes management data. Incorrect data display or bad suggestions can directly impact health decisions.** Every repository in this organization treats medical safety as a first-class concern, not just code quality.

Before writing any code, please understand these non-negotiable rules:

- 🏷️ **All** AI-generated outputs must be clearly labeled as **suggestions, not medical advice**
- 💉 Insulin dosing recommendations must **always** include safety disclaimers
- 🧪 Test thoroughly -- a wrong number on a glucose chart is not just a UI bug, it's a safety issue
- 🔒 Safety limits (glucose range, max bolus, max basal) are enforced by the platform via `SafetyLimits` (backend-synced, user-configurable). These limits validate incoming pump and CGM history values; when a reading falls outside the limits, plugins must discard it (do not return, emit, or persist it) and log the rejection with the violated limit -- see the [Plugin Architecture Guide](https://github.com/GlycemicGPT/GlycemicGPT/blob/main/docs/dev/plugin-architecture.md).
- 🚫 **No device control** -- GlycemicGPT is a monitoring and analysis platform

If you're unsure whether a change has safety implications, ask in the PR -- a maintainer will review.

### Monitoring-Only Posture (org-wide)

GlycemicGPT is a monitoring and analysis platform. The plugin SDK exists for one purpose: **community-built device data drivers that read from new hardware**. Plugins read glucose values, insulin-on-board, basal rates, bolus history, and pump status. They do not control devices.

**The project does not provide, distribute, document, or solicit plugins that expose any therapeutic write or control surface -- no bolus dosing, no basal rate changes, no pump-setting modifications.** This applies to every official build (Docker images, APKs, App Store / Play Store releases) and to every contribution merged into any repository in this organization. Pull requests that introduce therapeutic write primitives will not be merged. Non-therapeutic device-management operations that already exist in the SDK (CGM calibration, BLE pair/unpair, connect/disconnect) are session and lifecycle operations -- not therapy -- and remain permitted.

**Forks are not endorsed.** Forks of this project that add device control capabilities operate outside the GlycemicGPT project. The maintainers do not review them, recommend them, accept liability for them, or accept contributions whose intent is to enable them. Users who choose to run such forks become the manufacturer of their own personal medical device, consistent with the legal posture of Loop, AndroidAPS, and other DIY diabetes projects -- see [MEDICAL-DISCLAIMER.md](https://github.com/GlycemicGPT/GlycemicGPT/blob/main/MEDICAL-DISCLAIMER.md).

The plugin SDK has no insulin-delivery primitives, and the AI layer has no architectural path to a device write surface. The per-repo guides that ship device drivers (the platform repo, the mobile repo) document the driver-contribution mechanics; the *policy* above is the same everywhere.

---

## 📖 Repository-Specific Guides

Each repository has its own CONTRIBUTING.md with setup, testing, and workflow specifics, targeted at what people actually contribute there. Start with the one that matches your work:

| Repository | You'd contribute... | Guide |
|-----------|---------------------|-------|
| [GlycemicGPT](https://github.com/GlycemicGPT/GlycemicGPT) (main platform) | Backend API, web dashboard, AI sidecar, plugin SDK | [CONTRIBUTING.md](https://github.com/GlycemicGPT/GlycemicGPT/blob/develop/CONTRIBUTING.md) |
| [android-unofficial](https://github.com/GlycemicGPT/android-unofficial) | Android app, Wear OS, device drivers | [CONTRIBUTING.md](https://github.com/GlycemicGPT/android-unofficial/blob/develop/CONTRIBUTING.md) |
| [website](https://github.com/GlycemicGPT/website) | The public website and docs portal | [CONTRIBUTING.md](https://github.com/GlycemicGPT/website/blob/main/CONTRIBUTING.md) |

Each per-repo guide will reference this master for the org-wide policy below.

### Mobile contributions during the repository split

The Android and Wear OS apps are being extracted from the main platform repo into [android-unofficial](https://github.com/GlycemicGPT/android-unofficial). While that split is in progress: **mobile PRs are accepted in the main platform repo today; maintainers port them to android-unofficial with your authorship preserved; once the split completes, mobile PRs move there.** Contribute to whichever tree the guide you're following points you at -- either way your work lands in both places and you keep the credit.

---

## 👥 Project Roles

GlycemicGPT has three contributor roles: **Contributor**, **Committer**, and **Maintainer** (plus the **Project Lead**). Most people start as contributors -- just open a PR, file an issue, or join a discussion.

If you contribute consistently and demonstrate good judgment (especially around medical safety), you may be invited to become a committer with Write access. Committers can approve PRs, triage issues, and own specific components.

Maintainers are project stewards responsible for releases, security, and architectural decisions. It's an invitation-only role that comes after sustained involvement as a committer.

For the full details -- including how decisions are made, what each role can and can't do, how funding works, and how branch protection is enforced -- see [GOVERNANCE.md](GOVERNANCE.md).

---

## 💡 Ways to Contribute

There are many ways to help, not all of them involve writing code:

- 🐛 **Report bugs** -- Use the issue templates in the repository where the bug lives
- ✨ **Request features** -- Use the feature request template
- 📝 **Improve documentation** -- Typos, unclear instructions, missing guides
- 🧪 **Write tests** -- More coverage is always welcome
- 🔍 **Review PRs** -- Fresh eyes catch things automated checks can't
- 💬 **Answer questions** -- Help others in [Discussions](https://github.com/GlycemicGPT/GlycemicGPT/discussions)

Before opening an issue, please search existing issues in that repository to avoid duplicates. For general questions and support, use [Discussions](https://github.com/GlycemicGPT/GlycemicGPT/discussions/categories/q-a) instead of creating an issue.

---

## 🌿 Branching & Workflow

Every repository in the organization uses the same **develop/main** branching model:

```
feature branch --> squash merge --> develop --> merge --> main
                                      |                     |
                                  dev builds           stable releases
```

### Rules

- **`develop`** is the integration branch. **All contributor PRs target `develop`.**
- **`main`** is the stable release branch. Do **not** target PRs to `main`.
- Feature branches are created from `develop` and squash-merged back.
- Promotion from `develop` to `main` happens via a promotion PR; only the project lead merges it (see [GOVERNANCE.md](GOVERNANCE.md)).

> Some repositories that release continuously (for example, the website) may use `main` as their integration branch. When they do, their per-repo guide says so. When in doubt, target the branch that repo's guide names as the integration branch.

> **Note on the GitHub branch counter:** GitHub's branch comparison may show `develop` as a number of commits *behind* `main`. This is a cosmetic SHA-graph artifact, not content drift -- after a version bump or automated changelog update on `main`, the sync-back workflow cherry-picks those commits to `develop` as new commits with new SHAs. The file content stays identical.

### Branch Naming

Use a descriptive prefix: `feat/`, `fix/`, `docs/`, `refactor/`, `ci/`.

---

## 📝 Commit Messages

We use [Conventional Commits](https://www.conventionalcommits.org/). This drives automated CHANGELOG generation via [release-please](https://github.com/googleapis/release-please).

| Prefix | Usage | CHANGELOG |
|--------|-------|-----------|
| `feat:` | New features | Visible |
| `fix:` | Bug fixes | Visible |
| `perf:` | Performance improvements | Visible |
| `docs:` | Documentation only | Visible |
| `refactor:` | Code restructuring | Visible |
| `ci:` | CI/CD changes | Visible |
| `chore:` | Maintenance, deps | Hidden |
| `test:` | Adding/updating tests | Hidden |

Keep the subject line under 72 characters. Prefixes marked "Hidden" won't appear in the CHANGELOG but are still good practice.

---

## 🔀 Pull Request Process

1. Push your feature branch to your fork.
2. Open a PR **targeting `develop`** (not `main`), and fill out the PR template completely.
3. Link related issues using `Fixes #123` or `Relates to #123`.

### What happens next

1. **CI runs automatically** -- every required check must pass. The exact checks vary by repository (they're listed in each repo's guide), but they always include tests, linting, and security scanning.
2. **CodeRabbit review** -- an AI-powered code review runs automatically on every PR, checking for bugs, security issues, medical safety concerns, and code quality.
3. **Code owner review** -- a maintainer reviews your PR. All PRs require at least one approving review from a code owner.
4. **Feedback** -- you may be asked to make changes; push new commits to the same branch.
5. **Merge** -- once approved and CI passes, a maintainer squash-merges your PR.

If a check fails for what looks like an environmental reason rather than a problem in your code, ping a maintainer in the PR and we'll investigate.

---

## 🤖 AI-Assisted Development & Attribution Policy

Let's be real -- the code owners didn't write every line of this project by hand, and we don't expect you to either. **Using AI tools (Claude, Copilot, ChatGPT, Cursor, etc.) to help write code is completely fine.** We'd be hypocrites if we said otherwise.

That said, there's a difference between using AI as a tool and blindly pasting whatever it spits out. We don't want vibe-coded junk. **You are responsible for the code you submit**, regardless of who (or what) helped write it.

### What we expect

- **Understand your code.** If you can't explain what a function does and why, don't submit it.
- **Match existing patterns.** AI tools love to invent their own conventions. Make sure AI-generated code follows _our_ code style, architecture, and naming patterns.
- **Test it.** AI-generated code is especially prone to subtle bugs. Run the tests. Add new ones if needed.
- **Review it yourself.** Treat AI output like a junior developer's first draft -- helpful starting point, needs a careful eye.

### No AI attribution in code

This one is non-negotiable. Our repositories must be **clean of AI attribution lines**:

- **No** `Co-Authored-By: Claude`, `Generated by ChatGPT`, or similar lines in commits
- **No** `// Generated by AI` or `// Copilot suggestion` comments in code
- **No** AI tool branding, promotional links, or attribution banners in PR descriptions

Every repository runs an **Attribution Check** in CI that scans three layers -- commit message trailers, code comments in changed files, and PR descriptions -- covering all major AI tools. It will fail your PR if any attribution is found.

Why? Because attribution to a tool that can't be held accountable for code quality is meaningless noise. The _contributor_ is the author. Own it.

---

## 🔒 Security

This is a medical platform. We take security seriously across every repository.

- **Never commit** secrets, API keys, tokens, or credentials. CI secret-scanning and GitHub Push Protection are enabled org-wide as a safety net -- but the first line of defense is you.
- **Report vulnerabilities privately.** Do not open a public issue for a security vulnerability. Use the affected repository's **Security** tab → *Report a vulnerability* (GitHub private vulnerability reporting), which routes the report to maintainers only.
- **Security scan findings must be addressed, not ignored.** Each repository's CI runs targeted security tests based on what a PR changes; the per-repo guide documents the specific tools and how to respond to findings.
- **Every suppression needs a documented reason.** Accepting a known risk (a false positive, an unexploitable dependency CVE) is fine, but the suppression must record *why* and reference the issue that will fix the underlying problem. No reason = no merge. Maintainers review suppressions periodically and remove stale ones.

Suppression decisions and security-infrastructure changes require project-lead approval (enforced via CODEOWNERS). See [GOVERNANCE.md](GOVERNANCE.md) for the full security-governance model.

---

## 📜 License

The GlycemicGPT platform is licensed under the [GNU General Public License v3.0](https://github.com/GlycemicGPT/GlycemicGPT/blob/main/LICENSE). Each repository declares its license in its own `LICENSE` file; by contributing to a repository, you agree that your contributions are licensed under that repository's license.

---

## 💬 Questions?

- 🙏 **General questions & help** -- Post in [Q&A Discussions](https://github.com/GlycemicGPT/GlycemicGPT/discussions/categories/q-a)
- 💡 **Feature ideas & brainstorming** -- Post in [Ideas Discussions](https://github.com/GlycemicGPT/GlycemicGPT/discussions/categories/ideas)
- 🐛 **Bug reports** -- Open an issue in the relevant repository using the appropriate template

Please **do not** open issues for general questions -- use Discussions instead. Issues are for actionable bugs and feature requests.

All contributors are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).
