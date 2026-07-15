# find-tested-skills

A Claude Code skill that finds, installs, and runs **other** Claude skills for you — but only ones that
have actually been tested. Tell it your goal ("help me improve this project's SEO", "what should I install
here?") and it looks at your repo, queries the [SkillProof](https://skillproof.dev) catalog of
installed-and-scored skills, shows you the 2–4 best matches with verdicts and scores, **asks which to
install**, sets them up, and then uses them to do the work.

Three beats: **find & confirm → install → orchestrate.** It never installs anything without your yes.

## Why

There are thousands of Claude skills on GitHub. Half don't install, and some make Claude's output *worse*
than using no skill at all. Picking by star count doesn't tell you any of that. SkillProof installs each
skill, runs it on a real task against a no-skill baseline, and publishes an honest verdict (`pass` /
`setup` / `fails`) with a score out of 10. This skill puts those results in your session.

## Install

Copy the skill into your Claude Code skills directory:

```bash
git clone https://github.com/Skillproofdev/find-tested-skills.git
cp -r find-tested-skills ~/.claude/skills/find-tested-skills
```

Or drop the `find-tested-skills/` folder into any project's `.claude/skills/`.

## Use

Just ask, in Claude Code:

- "What skills should I install for this project?"
- "Find me a tested skill for writing SQL migrations."
- "Are there any good skills for cleaning up messy CSVs?"
- "Is there a tested skill for PDF generation?"

The skill reads your project, queries `https://skillproof.dev/api/skills.json`, and replies with matches
like:

> **[Changelog Generator](https://skillproof.dev/skills/changelog-generator)** — 9.2/10, pass
> Turns git history into a categorized changelog. In testing it beat a plain-Claude baseline on a real
> repo. Install: `...`

## How it works

The skill calls SkillProof's open, machine-readable catalog endpoint (CORS-open, no key):

```
GET https://skillproof.dev/api/skills.json?category=coding&verdict=pass&q=sql&limit=20
```

Every result carries the real test verdict, the /10 score, the per-criterion breakdown, the notes from
what actually happened during testing, and the exact install command. The skill ranks by fit and score,
never invents a skill, and never recommends anything that scored `fails`.

## Data & license

Catalog data comes from [SkillProof](https://skillproof.dev) under **CC BY 4.0** — attribution required.
This skill is MIT-licensed.
