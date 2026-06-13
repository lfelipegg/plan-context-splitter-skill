# Baseline Gaps Before Skill Update

Baseline skill word count: 499 words.

The current skill already splits plans into dependency-ordered packets, but the evals expose missing guidance:

- It does not define how to handle pasted text, file paths, Markdown, plain text, referenced repo files, or plans embedded in issues and PRs.
- It says to name likely files/modules but does not require repo guidance or existing validation commands before doing so.
- It has `Source sections`, but not a stricter traceability rule or `Source anchors` field.
- It has dependency ordering, but not typed dependency labels such as `blocks`, `informs`, `parallel`, `decision`, and `validation`.
- It has no packet readiness status, so ambiguous work is not clearly marked as `Needs decision`, `Needs repo inspection`, or `Blocked`.
- It warns against repeating the full source plan, but does not define what a concise suggested prompt should include or avoid.
- It warns against oversized packets, but not against over-splitting tiny changes that must be edited together.
