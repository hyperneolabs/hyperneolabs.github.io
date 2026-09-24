# HyperNeo Review

AI code review on every pull request: per-finding inline comments, one-line
verdicts, no ceremony. This page is the operating manual. It is current,
complete, and machine-readable.

## If you are an AI agent maintaining a repository

### What this does

On every pull request to an enrolled repo: reviews the diff, posts each
finding as an inline review comment at its diff position — severity
(P0/P1/P2; P0 and P1 are blocking, P2 is a minor note) and confidence — and
finishes with a one-line verdict: "Approved." or "Requires changes." The
review body names the engine and model that reviewed, the commit reviewed,
and the trigger. A status-table comment tracks each run: started/finished,
head, engine, verdict. Reviews are advisory by default; humans merge.

### Setup — human, once, ~15 minutes

1. Install the GitHub App: https://github.com/apps/hyperneo-ai/installations/new
   (Marketplace listing pending)
2. Grant access to the repositories to review
3. Sign in with GitHub OAuth (the installation account
   owner or a collaborator on any enrolled repository):
   https://agent.hyperneo.dev/login — operators land in the admin
   console (`/admin`, allowlisted via `OPERATOR_LOGINS`); everyone else
   lands in the customer app (`/app`), which lists the repositories
   they can access.

### Enroll a repository

Admin console (`/admin`, operator) → Repos → Enroll → `owner/name` plus the installation id (the
GitHub App installation; required with App auth). Or simply grant the App
access to the repo in your GitHub settings — auto-enroll follows, no id
needed. New repos start advisory, primary engine `claude`, triggers
open/push/mention. Reviews fire on the next PR open/push.

### Trigger a review

- Automatic: PR opened, reopened, or marked ready for review; every new
  push. Draft PRs are skipped until ready. Newest head wins — an in-flight
  review folds into the new head.
- On demand: comment on the PR mentioning `@hyperneo-ai` (e.g.
  `@hyperneo-ai review`) — needs the mention trigger on (it is by
  default). Any extra text in the comment steers that review.

All triggers honor the repo's trigger settings; open, push, and mention
are on by default.

### The review contract

- Findings: inline comments at the exact diff line — severity, confidence,
  what and why
- Verdict: one line in the review body, with engine/model provenance; clean
  PRs get the verdict and provenance lines only
- Findings on lines outside the diff: bare `file:line — severity` pointers
  in the body
- 👀 while running; removed when the run finishes

### Configure per repo (admin console, operator)

Mode — advisory (default; comments only) or gate (approve / request
changes) · primary engine + shadow engines (shadows run alongside, are
recorded, and are not posted) · triggers (open / push / mention / manual) ·
diff excludes · extra guidance (steers what the reviewer looks at)

### Agent etiquette

- Address findings by pushing fixes; the next push is re-reviewed
  automatically when the push trigger is on (it is by default)
- The verdict is advisory to the human who merges
- The bot does not converse — don't reply to its comments

### Machine access

This manual: https://hyperneolabs.github.io/agent.md (text/markdown, stable URL)
