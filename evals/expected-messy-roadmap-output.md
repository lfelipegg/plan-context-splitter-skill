# Expected Output: Messy Roadmap

The split should:

- Extract phases from the roadmap instead of following the loose note order blindly.
- Mark ambiguous work as `Needs decision`.
- Mark unknown code locations as `likely area`, `TBD`, or `needs repo inspection`.
- Avoid inventing precise files for the welcome email or analytics work.
- Keep Phase 1 shippable and defer deeper personalization until decisions/data exist.

Packets should likely include:

- Discovery/audit packet.
- Phase 1 checklist and billing reminder packet, if repo inspection supports it.
- Welcome email packet marked `Needs repo inspection`.
- Suggestions/personalization packet marked `Needs decision`.
- Measurement packet with validation or success criteria.

Reject output that:

- Pretends the suggestion data source is known.
- Creates implementation packets for legal-sensitive personalization before the required decision.
- Produces a generic roadmap summary instead of Codex-ready packets.
