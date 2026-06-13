# Expected Output: Migration Plan

The split should:

- Separate schema work, data/backfill work, app read/write changes, tests, and rollout validation when context differs.
- Flag deployment order clearly.
- Include rollback or risk notes for changed settings after the new write path is enabled.
- Put compatibility/fallback behavior in shared context or the relevant app-code packet.
- Include validation for migration behavior and app behavior.

Packets should use dependency types such as:

- Schema `blocks` backfill and app-code work.
- Backfill `informs` rollout validation.
- Feature-flag or deployment decision marked as `decision` if details are missing.

Reject output that:

- Combines schema, backfill, app code, tests, and rollout into one broad packet.
- Omits invalid or missing JSON values.
- Treats fallback removal as part of the first release without flagging the later-release constraint.
