# nk-rules-that-land

A [Claude Code](https://code.claude.com) skill. Write rules for an AI agent that actually change what it does.

Part of [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) — skills that stop an AI coding agent's
"done, tested, safe" from being taken on faith.

## What it does

- Three tests: names its incident · enforced by something that exists and runs · a checkable step where the default action happens, not a prohibition.
- `scripts/rules_check.py CLAUDE.md [--run]` audits a rules file: NO-INCIDENT, NO-ENFORCEMENT, MISSING-TARGET, FAILED-RUN, OVERFIT.
- What to do when the same mistake repeats with the rule in place, and when the rule list keeps growing.

The full procedure, the boundaries and where the rules came from are in [SKILL.md](SKILL.md).

## Install

Copy the folder into your skills directory (the skill is the repository root):

```bash
git clone https://github.com/NickkkLian/nk-rules-that-land ~/.claude/skills/nk-rules-that-land
```

or inside one project: `git clone … .claude/skills/nk-rules-that-land`.

As a plugin, through the marketplace in the index repository:

```
/plugin marketplace add NickkkLian/nickkk-skills
/plugin install nk-rules-that-land@nickkk-skills
```

To try it for one session without installing: `claude --plugin-dir ./nk-rules-that-land`.

## Verify

```bash
python3 scripts/rules_check.py --selftest
```

Standard library only, Python 3.9+. Before publishing, the guarded lines of each script were
mutated one at a time in a sandbox copy and the self-test was confirmed to go red on the named
assertion, without a traceback; the unmutated control stayed green.

## License

MIT. Read a script before letting it run in your environment.
