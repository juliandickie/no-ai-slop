# no-ai-slop fork - project guide

Julian Dickie's fork of [petergyang/no-ai-slop](https://github.com/petergyang/no-ai-slop), existing for exactly two reasons - to package the skill as an installable Claude Code plugin (upstream ships only a Codex plugin form built by `scripts/build_plugin.py`), and to carry deliberate customisations while still merging upstream's ongoing development.

## Layout contract

The whole fork discipline hangs on one rule - upstream files are never edited here.

- `SKILL.md` (repo root) - UPSTREAM'S source of truth. Never edit it in this fork. It changes only via merges from upstream.
- `skills/no-ai-slop/SKILL.md` - THE SHIPPING SKILL, what the Claude Code plugin actually loads. Starts as a byte-identical copy of root `SKILL.md`. All customisation happens HERE.
- `.claude-plugin/plugin.json` - the Claude plugin manifest (additive, upstream has no equivalent; version mirrors upstream's `.codex-plugin/plugin.json`).
- `CLAUDE.md` (this file) - additive, upstream has none.

The payoff - `diff SKILL.md skills/no-ai-slop/SKILL.md` is at all times the exact customisation ledger. An empty diff means pure upstream. Any hunk is either a deliberate local change (keep) or an upstream improvement not yet ported into the copy (port it), and hunks that improve on upstream are the contribution-candidate list for PRs back to petergyang/no-ai-slop.

## Sync ritual

1. `git fetch upstream` (remote already configured to petergyang/no-ai-slop)
2. `git merge upstream/main` - conflicts should be rare to impossible because upstream files are never edited here
3. `diff SKILL.md skills/no-ai-slop/SKILL.md` - review what upstream changed against the shipping copy, port improvements into the copy by hand, keep deliberate divergences
4. Bump `.claude-plugin/plugin.json` version to mirror upstream's `.codex-plugin/plugin.json` if it moved
5. Commit the port as its own commit, separate from the merge commit

## Relationship to anti-slop

This fork tracks Peter Yang's pattern set and edit voice as upstream evolves it. In Julian's copy pipeline, anti-slop is the read-only grade that runs last on AI-assisted drafts; this skill is a second opinion on how the prose reads. Keep them as separate plugins and do not blend them into one file.

## House style

Files authored in this fork (this file, plugin.json, any future additions) follow Julian's formatting rules - no em or en dashes, straight quotes, no colons in headings. Upstream files keep upstream's style untouched, em dashes included; they are not ours to reformat, and reformatting them would poison every future merge.
