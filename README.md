# adversarial-review

A Claude Code skill for hostile code review. It assumes the code under review is
broken and hunts for the proof, rather than offering balanced feedback.

The skill walks eight bug categories (logic, boundaries, error handling, state and
concurrency, cross-boundary data flow, security, data integrity, resource
management), reports each finding with a concrete trigger and a minimal fix, and
stays silent about anything it considers correct.

## Install

Clone the repo and symlink it into your skills directory:

```sh
git clone https://github.com/slowernet/claude-adversarial-review.git ~/code/claude-adversarial-review
ln -s ~/code/claude-adversarial-review ~/.claude/skills/adversarial-review
```

## Use

Ask for a review in any of the phrasings the skill's description matches, or
invoke it directly:

```
/adversarial-review
```
