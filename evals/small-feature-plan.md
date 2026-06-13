# Eval: Small Feature Plan

## Goal

Add saved filters to the issue list so users can store commonly used search filters and reapply them later.

## Repo Notes

- Existing issue list UI lives near `src/routes/issues`.
- Existing user settings API lives near `src/lib/server/settings`.
- Follow repo guidance in `AGENTS.md` and package scripts before choosing validation commands.

## Plan

1. Add a small server-side model for saved issue filters.
2. Add API actions for create, rename, delete, and apply saved filters.
3. Add UI controls beside the issue search bar.
4. Add tests for persistence, permissions, and applying a saved filter.

## Constraints

- Saved filters are user-specific.
- The initial release does not need sharing or team-wide filters.
- Keep the existing issue list URL query behavior.
