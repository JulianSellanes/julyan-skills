# Commit Style Guide for any AI reading this

version = 1.0

## Subject line

The subject is a SemVer-style version number.
- Format: `YEAR.MAJOR.PATCH`. No prefix (no `feat:`, `fix:`, `chore:`), no scope tag, no issue ref, no emoji, no trailing period.
- Two subject patterns are tolerated and only these two:
  1. SemVer (`26.5.0`) for normal releases.
  2. `Initial X` for the very first commit of a brand-new sub-project (e.g. `Initial dev files`, `Initial prod files`). Don't use this for anything that already has history.
- Auto-generated PR merge subjects (`Merge pull request #N from ...`) are acceptable when GitHub creates them, but never write them by hand.

## Body

The body explains what the release contains. Structure it as multiple short paragraphs, separated by blank lines, where each paragraph covers one logical theme.

Conventions seen across the repo:
- **One paragraph per theme.** Don't mix backend infra with frontend UI in the same paragraph. Examples of theme groupings: backend Lambda / SAM, frontend app shell, game UI, modals, Stripe / EventBridge, Cognito / Google auth, deploy/sync scripts, asset reorg.
- **Lead each paragraph with a present-tense verb** describing the action: Add, Replace, Refresh, Reorganize, Move, Wire, Bring, Set up, Create, Improve, Introduce, Extract, Extend, Convert, Pin, Remove, Harden, Support. Don't write past-tense ("Added X") or first-person ("I added X").
- **Name what changed concretely**: file names (`App.jsx`, `index.html`, `ads.txt`), services (Stripe, EventBridge, Cognito, AdSense, GA, DynamoDB, SAM, AdvanceQueue), or components (`LoadingModal`, `JoinModal`, `Nav`). Don't write vague summaries like "improved the UI" — write "Add sticky top Nav (burger + Sign In/Out) and replace floating auth button."
- **Position changes in time when relevant**: "the new menu", "the existing room flow", "the legacy auth form/modal assets". This makes a release diff readable months later.
- **Parenthetical detail is fine when it sharpens scope**, e.g. "(bots cannot)", "(VITE_GA_MEASUREMENT_ID)", "(top-1 wins for ranks 1–10)".
- **Prefer prose paragraphs over bullet lists** for normal releases. Bullet lists with a leading `-` are tolerated but only when the items are genuinely short and parallel.
- **Keep paragraphs to 1–3 sentences each.** A paragraph that runs longer is usually two themes mashed together, split it.
- **Don't list every file.** Only mention a file by name if it's the focal point of a paragraph or if the change is non-obvious from the area name.
