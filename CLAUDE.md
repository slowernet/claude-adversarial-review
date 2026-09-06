# Agent instructions

This repo is a Claude Code skill, so the "code" is prose that gets loaded into a
reviewing agent's context. Every line of SKILL.md costs tokens in every review.
Cut before you add.

## What things are

- `SKILL.md` is the whole skill. There are no references, templates, or scripts;
  if one becomes necessary, the checklist has probably grown past what it should
  carry inline.
- `README.md` describes the skill for people browsing GitHub. When it and
  SKILL.md disagree, that is a bug: fix both in the same change. README.md
  summarizes behavior, so it goes stale silently.
- `~/.claude/skills/adversarial-review` is a symlink to this repo, so an edit
  here changes the installed skill immediately, with no install step.

## Rules that must not be weakened

These are the lines a future editor will be tempted to soften into ordinary code
review. Softening any of them turns the skill back into the thing it exists to
replace.

- **Silence is approval.** The skill reports only what is wrong. Adding a
  "what's good here" section, a summary of correct behavior, or any acknowledgment
  of code that passed reverses the token economics and buries the findings.
- **No hedging vocabulary.** "Potential issue", "might be worth checking",
  "consider whether" all let a real bug read as optional. If the reviewer believes
  something is wrong, it says so.
- **Every finding carries a concrete trigger.** A category name and a line number
  are an accusation, not a bug report. The trigger is what makes a finding
  checkable, and it is what stops the reviewer from padding.
- **No manufactured findings.** "No bugs found" is a valid, complete output. A
  reviewer that must produce something will produce noise, and noise trains the
  reader to skim.
- **Scope stays narrow.** Not style, not features, not test coverage. Each of
  these is a whole other review, and each one dilutes the severity ordering that
  makes this output worth reading top-down.
- **The severity ladder is anchored to consequences**, not to how surprising the
  bug is. CRITICAL means data loss, a vulnerability, or a production crash. Keep
  the rungs describing what happens to users.

## Downstream consumer

`claude-swarm`'s plan reviewer and code reviewer both instruct spawned agents to
"load the adversarial-review skill" and work this checklist, then add four
lenses of their own (`references/templates.md:65` and `:219`). Two consequences:
renaming the skill silently breaks those spawns, and anything added here is paid
for in every swarm reviewer. Neither failure surfaces in this repo, so read
`~/code/claude-swarm/references/templates.md` before renaming the skill, changing
the severity levels, or growing the checklist.

## Style

- Sentence case headers, no emojis.
- Facts about Claude Code's own behavior are perishable. Where one must be
  stated, date it inline (*Current as of <month year>*) and write the surrounding
  rule so it holds either way.

## Verification

There is no test suite, and a skill that reads well proves nothing. Before
claiming a change works, run the skill on real code with a known defect and
confirm three things: it finds the planted bug, it reports a trigger you can
actually execute, and it stays silent about the correct code around it. A change
that makes the checklist longer without changing what a live run finds is a
regression.
