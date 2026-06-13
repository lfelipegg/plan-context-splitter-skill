# Eval: Migration Plan

## Goal

Move notification preferences out of a `users.preferences_json` blob and into normalized tables.

## Current State

- The existing app reads and writes notification settings through a user preferences service.
- Data currently lives in one JSON column.
- Some users have missing or partially invalid notification keys.

## Proposed Work

1. Add schema for notification preference categories and per-user settings.
2. Write a backfill that copies valid JSON values into the new tables and records skipped invalid entries.
3. Update reads to prefer the new tables with a temporary fallback to JSON.
4. Update writes to use the new tables.
5. Add tests for missing keys, invalid values, and backward compatibility.
6. Roll out behind a feature flag, then remove the JSON fallback in a later release.

## Risks

- The migration may touch many user rows.
- Rollback must not lose settings changed after the new write path is enabled.
- Admin reporting uses notification settings indirectly.
