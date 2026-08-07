# Pullwright — roadmap

Org-level roadmap for the Pullwright offering.

## Planned

### Tech-debt management framework

Adopt the per-item tech-debt register proven across the Poetic-Poems
repositories as part of the Pullwright offering:

- one Markdown file per item (`tech-debt/<id>.md`, YAML frontmatter plus a
  prose body), as an append-only set — item files are never deleted or
  renamed once merged, and IDs are never reused (CI-enforced);
- IDs unique across every repository with no cross-repo coordination
  (`TD-<scope>-<YYMMDD><NN>`, with a fixed-width scope code declared once
  per repository);
- race-safe claiming for concurrent writers, human or agent — pushing the
  `td/<id>` branch is the claim lock (remote ref creation as
  compare-and-swap), with draft-PR visibility from the moment of claim;
- tooling: ID allocation (`next-tech-debt-id.pl`), record lookup
  (`get-tech-debt-record.pl`), register validation (`td-check.pl`), CI
  enforcement of the append-only guarantee, and drift-checked byte-identical
  script copies in every consuming repository;
- canonical source today: `Poetic-Poems/poetic` (format specification in
  `docs/TECH-DEBT-REGISTER.md`); heaviest production user:
  `Poetic-Poems/agent-ops`.

Known gap to close before or during adoption: ID allocation is a scan, not
an atomic reservation, so concurrent writers can collide. The fix design —
reservation through the same `td/<id>` ref-push compare-and-swap that
claiming already uses, unifying filing and claiming into one primitive — is
recorded as `TD-PPpoet-26080801` in the `Poetic-Poems/poetic` register.
