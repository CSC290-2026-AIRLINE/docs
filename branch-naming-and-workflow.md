# Branch Naming & Git Workflow

## Team ID

Every branch and commit is scoped to a **team ID**: `g01`–`g16`, or `core` for platform-wide work (auth/RBAC/shared code, owned by dev leads + infra leads).

Use your team ID in branches and commits — never a module name, never a person's name. Team IDs don't change even if members do or the module they're assigned to changes. Which team owns which module lives in `backend-structure.md` / `frontend-structure.md`, not here — this doc only cares about the ID.

---

## Branch naming

```
<type>/<team-id>-<short-description>
```

- `type`: `feature`, `fix`, `refactor`, `chore`, `docs`, `test`
- `team-id`: lowercase, from the table above (`g03`, `core`, etc.)
- `short-description`: lowercase, hyphen-separated, no underscores, no spaces, max ~6 words

**Examples:**
```
feature/g03-seat-hold-ttl
fix/g07-boarding-pass-qr-bug
refactor/g04-fare-rules-engine
docs/core-readme-update
chore/g11-cleanup-unused-imports
```

**Not allowed:** `johns-branch`, `test123`, `fix-bug`, `Feature/G03_SeatHold`, anything with your name, anything without a team-id.

`main` is protected — you cannot push to it directly regardless of branch name.

---

## Commit messages

Follow Conventional Commits, scoped by team ID:

```
<type>(<team-id>): <description>
```

**Examples:**
```
feat(g03): add seat hold TTL and automatic release
fix(g07): correct QR code generation for boarding pass
docs(core): add onboarding steps for new backend module
chore(g14): remove dead code in notification templating
```

Types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `style`

Keep the description under ~72 characters. If you need more detail, put it in the commit body, not the subject line.

---

## Pull requests

**PR title** — same convention as branches, in brackets:
```
[G03] Add seat hold TTL and automatic release
```

**PR must include** (use the PR template — don't delete sections):
- What changed and why
- How you tested it (steps, or link to test output)
- Screenshots, if frontend and visual
- Any other module/team your change touches, called out explicitly

**Size:** keep PRs small. If your diff is pushing past ~400 lines, split it. Large PRs get sat on and reviewed badly.

**Merge requirements** (enforced by branch protection on `main`):
- Required approving reviews
    - `backend` & `frontend`: 2 approvals required
    - `database`: 1 approval required
    - `docs`: 0 approvals (PR required, mergeable upon creation)
- Code owner approval required (see `CODEOWNERS` — if you touch another team's folder, their lead is auto-requested)
- All review comment threads must be resolved before merge
- No force-push, no deletion of `main`

**Merge method:** squash merge only. *(Current ruleset allows merge/squash/rebase — Infra Leads to tighten this to squash-only before teams start merging in volume, so `main` history stays one commit per PR instead of accumulating every WIP commit from every team.)*

---

## Branching model

Trunk-based. No `develop` branch, no long-lived branches.

1. Branch off `main`
2. Work, commit, push
3. Open PR back to `main` as soon as it's reviewable — don't sit on a branch for a week
4. Get required approvals, resolve threads
5. Squash-merge
6. Delete the branch after merge (GitHub can auto-delete on merge — enable this at repo settings level)

If your branch is more than a few days old and hasn't been merged, either it's too big (split it) or it's blocked (say so in the team channel).

---

## Cross-module changes

If your PR touches a folder owned by another team (shared code, another module's exposed interface), `CODEOWNERS` will auto-request that team's lead as a reviewer. Do not merge until they've approved, even if you already have 1 approval from someone else.

**API contracts between modules freeze by week 3–4.** After that point, changing an endpoint another team depends on requires agreement from both teams before the PR opens, not after.
