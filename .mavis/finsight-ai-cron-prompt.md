# finsight-ai cron prompt (v1)

## Config
- OWNER=aakashrathore136
- REPO=finsight-ai
- FORK_OWNER=tmdeveloper007
- FORK_REPO=FinSight-AI
- BASE_BRANCH=main
- ISSUE_COUNT=5

## Rules
1. NEVER emojis.
2. NEVER force without --force-with-lease.
3. NEVER delete/weaken existing tests.
4. NEVER touch .github/workflows/, root package.json, package-lock.json.
5. Cap 5 PRs per run.
6. ALWAYS git pull upstream main before branching.
7. Issue title: `<type> : add <what>`. PR title: `<type> : added <what>`.
8. Fork name: `FinSight-AI` (capital F, S, A, I).

## 5 Issues to Create + PRs

### Issue 1: fix : correct date range calculation in generateWeeklyComparison in trendsUtils
- **File**: `src/lib/trendsUtils.ts`
- **Bug**: `generateWeeklyComparison` uses `startOfWeek(subMonths(end, 0))` which is identical to `startOfWeek(end)`. It should go back by weeks using `subWeeks(end, weeks - 1)` so that it generates the correct number of weeks in the past.
- **Fix**: Change `startOfWeek(subMonths(end, 0))` to `subWeeks(end, weeks - 1)`.

### Issue 2: fix : pass error not settings object to handleFirestoreError in privacyUtils updatePrivacySettings
- **File**: `src/lib/privacyUtils.ts`
- **Bug**: In `updatePrivacySettings`, the `catch` block calls `handleFirestoreError(settings, ...)` passing the settings object instead of the error. Should pass `error`.
- **Fix**: Change `handleFirestoreError(settings, ...)` to `handleFirestoreError(error, ...)`.

### Issue 3: fix : remove debug console.log statements from Dashboard component
- **File**: `src/components/Dashboard.tsx`
- **Bug**: Dashboard component contains `console.error` calls that leak internal state. Remove all `console.log` and `console.error` debug statements that are not already guarded.
- **Fix**: Remove `console.log` and `console.error` debug calls, keeping those inside try/catch that are meaningful.

### Issue 4: fix : remove debug console.log statements from FileUpload component
- **File**: `src/components/FileUpload.tsx`
- **Bug**: FileUpload component has `console.log("UPLOAD START")`, `console.log("UPLOAD COMPLETE — server response:")`, `console.log("Failed record persisted to Firestore")` that leak internal flow info.
- **Fix**: Remove all `console.log` debug calls, keep `console.error` and `console.warn` in catch blocks.

### Issue 5: fix : remove debug console.log statements from AnalysisDetail component
- **File**: `src/components/AnalysisDetail.tsx`
- **Bug**: AnalysisDetail contains `console.log("Loading latest analysis from subcollection...")`, `console.log("Loaded analysis doc ID:")`, `console.log("Loaded sentiment_score:")`, and similar debug statements.
- **Fix**: Remove all `console.log` debug statements, keep `console.error` in catch blocks.

## Workflow per issue
1. `git pull upstream main --force-with-lease` (or git reset)
2. Create upstream issue with title `<type> : add <what>` and body with required sections
3. Branch: `git checkout -b "#$ISSUE_NUM"`
4. Implement the fix
5. Commit: `<type>: <past-tense summary>`
6. Push: `git push origin "#$ISSUE_NUM"`
7. Open PR: title `<type> : added <what>` with Closes #ISSUE_NUM
8. Wait for CI, fix if needed (up to 3 cycles)
