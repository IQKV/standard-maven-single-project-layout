# AGENTS.md — Repository Guidelines for AI Agents

This document defines how AI agents and contributors should work in this repository. It prioritizes safety, reproducibility, and consistency for a Java/Maven single-project on Windows/PowerShell.

## 1) Scope and Principles

- Safety first: never run destructive or irreversible actions without explicit request and clear rollback.
- Reproducibility: prefer commands and changes that are deterministic and CI-friendly.
- Minimal assumptions: detect project configuration from files (e.g., `pom.xml`) before deciding tools or versions.
- Transparency: explain planned changes and why; keep change sets focused and reviewable.

## 2) Environment and Shell

- OS: Windows
- Shell: PowerShell (pwsh)
- Path separators: use `\` for Windows paths in commands; repository-relative paths in documentation.
- Avoid interactive or full-screen commands. Prefer non-interactive flags.
- Disable pagers in VCS commands (e.g., use `git --no-pager ...`).

## 3) Safety and Secrets

- Never expose or log secrets. If a query contains redacted secrets (e.g., `******`), treat them as unavailable.
- Use environment variables for secrets and avoid echoing them. Example pattern:
  - Retrieve/set secret in env first (outside the scope of this repo’s commands), then reference as `$Env:MY_SECRET` without printing it.
- Avoid destructive commands (delete, force push, database mutations) unless explicitly asked and confirmed.

## 4) Repository Structure (Maven Single Project)

Typical layout:

- `pom.xml` — Maven configuration
- `src/main/java` — application source
- `src/main/resources` — resources
- `src/test/java` — tests (e.g., JUnit)
- `src/test/resources` — test resources
- `README.md` — project overview
- `AGENTS.md` — this guide

If structure differs, detect from the repo before acting.

## 5) Commands: Conventions and Examples

- Always prefer repository-root absolute paths in scripts/automation; use repository-relative paths in docs.
- PowerShell examples (non-interactive, no pagers):
  - Git
    - `git --no-pager status`
    - `git --no-pager log --oneline -n 20`
    - `git --no-pager diff`
  - Maven (quiet where appropriate, but keep useful output for failures):
    - `mvn -q -DskipTests=false test`
    - `mvn -q -DskipTests package`
    - `mvn -q verify`
  - Search (Windows):
    - `git --no-pager grep -n "ExactTerm"`
    - `Select-String -Path src/**/*.java -Pattern "ExactTerm"`

Notes:

- Prefer `git grep` for tracked files.
- Quote patterns properly in PowerShell.

## 6) Editing Files

- Make cohesive, minimal diffs. Do not mix unrelated changes.
- Preserve existing code style and patterns evident in the codebase.
- Do not rely on interactive editors in the terminal for substantial edits.
- Keep line endings consistent (respect existing `.gitattributes` if present). On Windows, avoid introducing mixed line endings.

## 7) Java and Maven Standards

- Java version: infer from `pom.xml` (e.g., `maven-compiler-plugin` `source`/`target`). If absent, default to the project standard; do not assume arbitrarily.
- Testing: use JUnit or framework configured by the project. Place tests under `src/test/java` mirroring package structure.
- Build targets:
  - Run tests locally: `mvn -q -DskipTests=false test`
  - Full verification (including integration checks if configured): `mvn -q verify`
  - Package artifact: `mvn -q -DskipTests package`
- Lint/format: if the project uses plugins (e.g., Spotless, Checkstyle), run them consistently:
  - Spotless (if configured): `mvn spotless:check` then `mvn spotless:apply` if needed
  - Checkstyle (if configured): `mvn checkstyle:check`

If these plugins are not present in `pom.xml`, do not assume they exist; align with current project configuration.

## 8) Code Style & Structure

- Follow existing naming, packaging, and dependency patterns.
- Keep public APIs stable unless a breaking change is explicitly requested; if so, document and bump versions accordingly.
- Add tests for bugfixes and new features; prefer TDD for small changes when practical.
- Avoid introducing new dependencies without strong justification. If needed:
  - Prefer small, well-maintained libraries.
  - Consider licensing, transitive dependency footprint, and security.
  - Document the rationale in the PR.

## 9) Documentation

- Update `README.md` when changing usage, setup, or external behavior.
- Keep `AGENTS.md` aligned with any process changes.
- If introducing or changing CLI, configuration, or environment variables, document them.

## 10) Git Workflow

- Branching:
  - `feat/<short-kebab-summary>` for features
  - `fix/<short-kebab-summary>` for bugfixes
  - `chore/<short-kebab-summary>` for maintenance
  - `docs/<short-kebab-summary>` for documentation-only
- Commit messages: follow Conventional Commits when possible:
  - `feat: add X`
  - `fix: correct Y`
  - `docs: update Z`
  - Include scope when helpful: `feat(api): add pagination`
- Keep commits focused and logically grouped. Avoid mixing formatting changes with logic changes.
- Rebasing:
  - Prefer `git --no-pager pull --rebase` for linear history when safe.
  - Resolve conflicts locally with clear, minimal conflict resolutions.
- Do not force-push to shared branches without explicit approval.

## 11) Pull Requests (PRs)

- Small, focused PRs are easier to review and merge.
- Include:
  - Summary of changes and motivation
  - Links to issues/tickets
  - Testing notes (commands run, evidence)
  - Any migration or operational steps
- Ensure all checks pass (build, tests, linters) before requesting review.

## 12) Versioning & Releases

- If using semantic versioning, follow semver rules:
  - fix = patch, feat = minor, breaking change = major
- For Maven projects, use the Maven Versions Plugin for bumps if configured:
  - `mvn versions:set -DnewVersion=<version>`
  - `mvn versions:commit`
- Update `CHANGELOG.md` if present and relevant.

## 13) Windows Considerations

- File paths may include spaces; quote paths in commands as needed.
- Use `Select-String` or `git grep` for searching. Prefer UTF-8 encoding when creating/editing files.
- Avoid commands that rely on Unix-only tools unless the environment provides them.

## 14) Security & Compliance Checklist (Pre-PR)

- No secrets added to code, config, or history.
- Dependencies are necessary, current, and reputable.
- External calls or URLs are validated and safe.
- Tests cover the changes and pass locally.
- Documentation updated as needed.

## 15) Agent Execution Checklist (Before and After Changes)

Before:

- Identify scope and confirm assumptions.
- Detect project configuration from `pom.xml` and file structure.
- Plan minimal change set and testing strategy.

During:

- Make atomic commits, keep diffs small, and write clear messages.
- Run targeted tests/verification frequently.

After:

- Run full verification: `mvn -q verify`
- Ensure no unintended file changes (e.g., line endings, formatting drift) were introduced.
- Prepare a clear PR description with testing notes.

## 16) Contact and Escalation

- If ambiguity arises (build plugins, Java version, CI specifics), prefer to inspect `pom.xml` and existing CI configuration.
- When in doubt, propose a short list of options with trade-offs and ask for guidance before implementing intrusive changes.

---

By following these guidelines, agents and contributors will produce safe, reviewable, and maintainable changes tailored for a standard Maven single-project repository on Windows/PowerShell.
