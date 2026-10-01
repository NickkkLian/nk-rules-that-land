# nk-rules-that-land

An agent skill for [Claude Code](https://code.claude.com) and [OpenAI Codex](https://developers.openai.com/codex). Write rules for an AI agent that actually change what it does.

**What you get.** One real run of nk-rules-that-land 0.1.4, copied from the terminal on 2026-09-30:

```text
$ python3 scripts/rules_check.py demo-rules.md
demo-rules.md: 2 directives · NO-INCIDENT 1 · NO-ENFORCEMENT 1
  NO-INCIDENT     L1    - Never push on a Friday.
  NO-ENFORCEMENT  L1    - Never push on a Friday.
```

![nk-rules-that-land](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/social/nk-rules-that-land.png)

Part of [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) — skills that stop an AI coding agent's
"done, tested, safe" from being taken on faith.

## Try it

Nothing is installed and nothing under `~/.claude` changes: clone, run the self-test, run the example (it only writes inside the clone).

```bash
git clone https://github.com/NickkkLian/nk-rules-that-land && cd nk-rules-that-land
python3 scripts/rules_check.py --selftest
printf -- '- Never push on a Friday.\n- Always run the gate before publishing. Why: a page went public unasked. Enforced by: scripts/rules_check.py\n' > demo-rules.md
python3 scripts/rules_check.py demo-rules.md
```

The self-test prints:

```text
rules_check selftest · 13/13 passed
```

The last command prints the block at the top of this page; its last line is the one below, and its exit code is 1 (non-zero on purpose: it found something).

```text
  NO-ENFORCEMENT  L1    - Never push on a Friday.
```

![nk-rules-that-land demo: before and after](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/nk-rules-that-land.gif)

The demo above is a rendering of an earlier run and cuts its longest lines short; the block at the top of this page is a full run of this version.

## What it does

- Three tests: says why (its incident, when there is one) · enforced by something that exists and runs · a checkable step where the default action happens, not a prohibition.
- `scripts/rules_check.py CLAUDE.md [--run]` checks one convention: does each rule carry a reason tag (`Incident:` / `Why:`) and an enforcement tag (`Enforced by:`) that points at something that exists. On a file that never used the tags every rule is listed, and the output says so; `--why-tags` / `--enforced-tags` add your own words. It does not judge whether a rule is good.
- What to do when the same mistake repeats with the rule in place, and when the rule list keeps growing.

The full procedure, the boundaries and where the rules came from are in [SKILL.md](SKILL.md).

## How it works

1. It says why. A rule whose reason nobody can state is an imagined risk; imagined risks fire on normal work, and a few false alarms teach the reader to ignore all alarms.
2. It is enforced by something that exists and runs.
3. It is a positive, checkable step at the point where the default happens, not a "never".

## Why it is built this way

**The idea.** A prohibition only works at the moment someone remembers it; the mistake happens during the default action, when nobody is remembering anything.

**Where it came from.** The habit of reading published prompt-engineering advice ("write prohibitions", "explain why") came first; the three tests above were rebuilt from what actually changed behaviour in daily use: incident-backed rules, machine enforcement on the real path, and a checkable step instead of a prohibition.

**Evidence.** What was broken on purpose to show that the self-tests can fail is under [Verify](#verify); what was run end to end, and in which agent, is under [Compatibility](#compatibility).

## Install

Pick one of four ways: three for Claude Code, one for OpenAI Codex. Skills load when a session starts, so open a **new** session after installing.

### 1 · Terminal, one command

```bash
git clone https://github.com/NickkkLian/nk-rules-that-land ~/.claude/skills/nk-rules-that-land
```

1. Run the command above (for one project only, clone into `.claude/skills/nk-rules-that-land` inside that project).
2. Start a new Claude Code session.
3. Check it loaded: type `/nk-rules-that-land` — it appears in the slash-command menu. Or just ask for the task; the skill triggers on its own.

### 2 · Claude Code in a terminal session (plugin)

The plugin route goes through the [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) marketplace. Add it once; after that each skill is one command.

```
/plugin marketplace add NickkkLian/nickkk-skills
/plugin install nk-rules-that-land@nickkk-skills
```

1. In a Claude Code session, run the first line (once per machine).
2. Run the second line.
3. Start a new session (or run `/reload-plugins`). The skill shows up as `nk-rules-that-land:nk-rules-that-land`.

Without opening a session, the same two steps work from a shell: `claude plugin marketplace add NickkkLian/nickkk-skills` then `claude plugin install nk-rules-that-land@nickkk-skills`.

### 3 · Claude desktop app (Code tab)

**Add the marketplace first — Discover only searches marketplaces you have already added.**

<img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/panel-route.gif" alt="Adding the marketplace and installing a skill in the desktop app" width="640">

<sub>Recorded on 2026-09-16, when the marketplace listed ten skills, all at version 0.1.0; it lists more now. The repository list in this recording shows the recorder's own repositories because a GitHub account is connected; yours will show yours. Type the full name as in step 4.</sub>

1. In the chat box, type `/plugin marketplace` and press Enter (or open **Settings → Customize → Plugins**). The **Plugins** panel opens.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step1-type-plugin-marketplace.png" alt="/plugin marketplace typed in the chat box" width="480">
2. Top right, open **Add ▾** and choose **Add marketplace**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step2-add-menu.png" alt="The Add menu with Add marketplace" width="480">
3. Choose **Add from a repository**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step3-add-from-repository.png" alt="Add marketplace dialog: Add from a repository" width="480">
4. In **URL**, type the full `NickkkLian/nickkk-skills`. At the bottom of the list choose the row **Use "NickkkLian/nickkk-skills"**, then press **Sync**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step4-url-then-sync.png" alt="URL filled in, Sync button" width="480">
5. You land on **Discover**, filtered to the new marketplace (**Filter · 1**). Find **Nk rules that land** and press **Add**. Installed ones show **✓ Added**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step5-discover-add.png" alt="Discover list with Added and Add buttons" width="480">
6. Close the panel and start a new session.

To try it for one session without installing anything: `claude --plugin-dir ./nk-rules-that-land` from a clone.

### 4 · OpenAI Codex CLI

```bash
git clone https://github.com/NickkkLian/nk-rules-that-land.git ~/.agents/skills/nk-rules-that-land
```

1. Run the command above (for one project only, clone into `.agents/skills/nk-rules-that-land` inside that project).
2. Start a new Codex session.
3. Check it loaded, without spending a model call: `codex debug prompt-input | grep -o -- '- nk-rules-that-land[a-z0-9:-]*' | sort -u` prints `- nk-rules-that-land:nk-rules-that-land:`. Codex adds the `nk-rules-that-land:` prefix because this repository also carries a Claude Code plugin manifest. Ask for the task and the skill triggers on its own, or type `$` and pick it from the list.

## Compatibility

| Agent | Tested | What was checked |
|---|---|---|
| Claude Code (CLI 2.1.173, macOS) | yes | In a fresh project with an isolated Claude config, inside a macOS sandbox that blocked reading the tester's ~/.claude folder (settings, session history, memory), Desktop, Documents and Downloads, SSH keys and git identity, a plain request that never names the skill triggered it and it ran its bundled script. The route 2 plugin commands were also run from a shell with an isolated config: marketplace add, install, list. |
| OpenAI Codex CLI (0.154.0-alpha.6.2, gpt-5.6-sol, low reasoning, macOS) | yes | Copied into `~/.agents/skills` of a temporary home (the folder route 4 clones into), in a fresh project, without the user's Codex config. From a plain request that never names the skill, Codex read SKILL.md, ran `scripts/rules_check.py --run` on the rules file (3 directives, none with an incident or an enforcement) and said what each rule needs to actually stop an agent. |
| Cursor, Gemini CLI | no | Not tested. Their documentation says both read `~/.agents/skills`, the folder route 4 clones into; Gemini CLI asks before it activates a skill. |

In this skill's Codex run, every call into the skill folder's scripts/ used that folder's absolute path. Route 4 was checked for this repository: cloned from GitHub into a temporary home's `~/.agents/skills`, it was listed by the step 3 command. This skill's frontmatter uses only name, description, license and metadata.

## Verify

```bash
python3 scripts/rules_check.py --selftest
```

Standard library only, Python 3.9+. On 2026-09-30 every self-test above passed, and
`breakcheck.py` from [nk-breakable-selftest](https://github.com/NickkkLian/nk-breakable-selftest) broke each script on purpose in a sandbox copy:

- `rules_check.py`: 9 lines broken one at a time; each turned the self-test red without a traceback.

The unmutated control stayed green every time. Only lines that record a finding, raise, or return a failing exit code
were broken (the tool's pattern, or the hand-written list); a line number refers to the script as shipped in this version.
This shows those lines are covered. It does not show that nothing else can fail.

## Limits

- The audit finds tags, not truth: a fake `Enforced by:` passes. `--run` catches enforcement that is broken, not enforcement that is off the path.
- Directive detection is keyword-based; a rule phrased without must/never/always is not counted.

## License

MIT. Read a script before letting it run in your environment.
