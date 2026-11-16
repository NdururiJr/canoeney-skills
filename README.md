# canoeney-skills

Canoeney Labs: vetted, pinned copies of the skills our agents use. Owner-controlled.

Every skill here is a copy of an upstream skill, taken at one commit and edited by the rules below. HQ installs these copies, and only these, into each agent's folder: the owner pins one commit of this repo's default branch in **Settings → Skills** and confirms it with the PIN. HQ reads the pinned commit with `git` alone, keeps the files in its own store, and copies them into an agent's folder at every start. HQ never runs anything a skill contains.

## Layout

```
skills/<name>/SKILL.md          the skill
skills/<name>/...               the skill's other files
skills/<name>/UPSTREAM.json     { "repo", "commit", "path", "license" } of the copy's source
skills/<name>/LICENSE.upstream  the upstream licence, unchanged
```

- `<name>` matches `^[a-z0-9][a-z0-9-]{0,62}$` and is a name Windows can hold.
- HQ refuses the whole pin (SR-F-06) if any skill has:
  - a link (mode `120000`), a submodule (`160000`), or any mode other than `100644` and `100755`;
  - a path with a `.` or `..` segment, an empty segment, a leading `/`, a `\` or a NUL;
  - a path segment Windows cannot hold: `CON`, `PRN`, `AUX`, `NUL`, `COM0`–`COM9`, `LPT0`–`LPT9` in any case and with any extension, or one ending in `.` or a space;
  - two paths that differ only in letter case;
  - no `SKILL.md`;
  - more than 256 files or 2 MiB.
- The pin preview lists every script: a file with mode `100755`, or one ending in `.sh .bash .zsh .py .js .mjs .cjs .ts .mts .ps1 .rb .pl`.

## Skills in this repo

| Skill | Upstream | Roles | Scripts |
|---|---|---|---|
| `test-driven-development` | `obra/superpowers` (MIT) | builder | none |
| `systematic-debugging` | `obra/superpowers` (MIT) | builder | `find-polluter.sh`, `condition-based-waiting-example.ts` |
| `verification-before-completion` | `obra/superpowers` (MIT) | builder, ceo, cio | none |
| `executing-plans` | `obra/superpowers` (MIT) | builder | none |
| `writing-plans` | `obra/superpowers` (MIT) | cto | none |
| `requesting-code-review` | `obra/superpowers` (MIT) | cto | none |
| `brainstorming` | `obra/superpowers` (MIT) | cto | none |

Which role gets which skill is set in HQ (the role sets), not here. superpowers is the one skill family for agents' working practices; other families are not vendored for agents.

## How to vendor a skill

1. On a branch, copy the skill's folder from upstream at one commit, byte for byte and with its file modes, and add `UPSTREAM.json` and `LICENSE.upstream`. Commit that unedited copy on its own.
2. Apply the edit rules below in a second commit, so its diff is the review.
3. Open a pull request. Its body has one row per skill: the source and commit, each of rules 2–7 ticked or explained, the scripts, and every SR-F-07 removal.
4. The owner reviews and merges. Then the owner pins the merge commit in HQ.

Updating a skill to a newer upstream commit follows the same steps: copy the new upstream folder over the old, commit, then re-apply the rules.

## Edit rules

1. Copy the folder from upstream at one commit. Write `UPSTREAM.json` and `LICENSE.upstream`.
2. Replace "your human partner" and "the user" (where they mean the person directing the agent) with "your manager (message them with `send_message`)".
   - Where the skill stops and waits for a person, it says: "send your manager the question with `send_message` and wait for the reply".
3. Remove every reference to a skill outside the role's set and to work HQ owns. This covers:
   - using-git-worktrees, subagent-driven-development, dispatching-parallel-agents and finishing-a-development-branch;
   - dispatching subagents;
   - creating worktrees or branches outside the agent's own `<agent id>/…` names.

   Point to "your role file" instead.
4. No path that leaves the skill's folder (`../`), and no link. A script that reached another skill's folder is removed, or rewritten to stand alone.
5. **SR-F-07:** no company policy in a skill. That means no rule about authority, approvals, money, confidentiality, honesty toward the owner, or what needs the owner.
   - Such text is removed and, where it matters, written up for the owner to put into `Knowledge`.
   - The pull request lists each removal.
6. brainstorming is trimmed: the visual companion and its scripts are removed.
7. Stack skills: those of every stack set on a company project, read from HQ's store at the time of the pull request. Rules 2–5 apply.

A note on links: skill files name their own companion files as plain text (`writing-good-tests.md` in this skill's folder), not as Markdown links, and hold no web links.

## Licence

The repository is under the MIT licence in `LICENSE`. Each vendored skill also keeps its upstream licence in its own `LICENSE.upstream`; the copyright notices there still apply to the upstream parts.
