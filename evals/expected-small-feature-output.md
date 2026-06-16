# Expected Output: Small Feature Plan

The split should:

- Produce 2-4 packets.
- Avoid any packet larger than Medium.
- Include validation commands based on repo inspection, not invented scripts.
- Preserve the existing URL query behavior as shared context.
- Include one recommended first prompt.
- Keep packet text concise while still including handoff/report-back fields and concrete carry-forward notes for later Codex sessions.

Packets should likely separate:

- Server/model/API work.
- Issue list UI work.
- Tests and hardening, if not naturally included with the relevant packet.

Reject output that:

- Repeats the full original plan in every prompt.
- Creates a separate packet for every tiny API action.
- Invents exact files beyond the likely areas in the source plan.
- Creates a Large packet without explaining why splitting would harm execution.
