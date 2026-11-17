# Installing the kit

The kit comes with two install scripts. They copy files to the places Copilot looks for them. Run both: one sets up Copilot for you, the other sets up one repo.

| Script | Run it | What it sets up | Where the files go |
|---|---|---|---|
| `install-user.py` | Once, on your computer | Your personal rules, agents and skills | Your Copilot folder, `~/.copilot`. Works in every repo you open. Nothing is committed. |
| `install-repo.py` | Once for each repo | The repo's notes (`AGENTS.md`), language guides and the safety hook; agents and skills too if you pick them | Inside that repo, in `AGENTS.md` and `.github/`. You decide whether to commit them. |

```
On your computer (~/.copilot)           In one repo
-----------------------------           -----------
your personal rules                     the repo's notes (AGENTS.md)
agents                                  language guides
skills                                  safety hook

set up by install-user.py, once         set up by install-repo.py, once per repo
```

Language guides go only into repos, because they belong to a repo's code. Your personal rules go only into your Copilot folder, because they are yours.

### Install each item at one level

Install agents and skills for yourself with `install-user.py`, and only the repo's notes, the language guides and the safety hook with `install-repo.py`:

```
python install-repo.py ~/code/my-service --agents none --packs java
```

Replace `java` with the repo's language pack (`java`, `node`, `scala` or `flutter`), or `none` if the kit has no guide for its language. A language pack also puts its skills (the code pack's, and sbt-and-play for Scala) into the repo; the script cannot install a guide without them.

If the same agents and skills are in both places, Copilot loads the agent lines twice in every session, which costs credits, and a skill in the repo's `.github/skills/` is used instead of yours in `~/.copilot/skills/`, even when the repo's copy is older. So install agents and skills into a repo only when your team wants them shared and versioned with the repo, and then update the repo's copies whenever you update the kit.

## What you will see

Each script lists what it can install and asks you to choose. **If you are unsure, press Enter at every question.** Enter takes the suggested answer.

A shortened example of `install-repo.py`:

```
$ python install-repo.py ~/code/my-service
Copilot kit: install into one repository.

Agents in the kit:
   1. builder              Use when a change is fully specified ...
   2. data-analyst         Use when a question about a CSV ...
   ...
Which agents? Enter for all, numbers or names such as 1,3 for some, or none:     <- press Enter

Packs in the kit:
   1. code         ...
   ...
Which packs? Enter for code, java, documents, planning; or numbers or names such as 1,3; or all; or none:     <- press Enter

Into ~/code/my-service:
  31 new file(s) to install
  0 file(s) already up to date
Install now? [Y/n]     <- press Enter

Done.
```

Ways to answer the "Which ...?" questions:

- **Enter** takes the suggestion.
- **Numbers or names**, such as `1,3` or `worker,reader`, pick only those.
- **`all`** or **`none`**.

What Enter picks:

- **Agents:** all of them.
- **Packs, in `install-user.py`:** all of them.
- **Packs, in `install-repo.py`:** code, documents, planning, and the language pack for each language it finds in the repo. It shows the files it used to decide.

In `install-repo.py`, Enter installs all agents and the suggested packs' skills into the repo, which is more than [one level per item](#install-each-item-at-one-level) needs. To follow that advice there, answer `none` at the agents question and only the repo's language pack at the packs question.

## Packs

A pack is a bundle of skills for one kind of work. A language pack also adds that language's guide to the repo.

| Pack | Skills | Language guide (repo only) |
|---|---|---|
| `code`, for any language | build-and-test, write-tests, dependency-upgrade, api-contract-check | none |
| `java` | the code pack's | Spring Boot |
| `node` | the code pack's | NestJS |
| `scala` | the code pack's, and sbt-and-play | Scala |
| `python` | the code pack's | none yet |
| `flutter` | the code pack's | Flutter |
| `documents` | read-documents, csv-analysis, pptx, docx, pdf | none |
| `planning`, for planning and writing | spec-interview, draft-from-examples, session-capture, setup-project | none |

[kit/skills/README.md](kit/skills/README.md) says what each skill does.

## If you have installed before

Each install records what it wrote in a small file, `.copilot-kit.json`: in your Copilot folder for `install-user.py`, and in the repo's `.github/` for `install-repo.py` (commit it with the other files). On the next run, the script compares each file with the kit's copy and with that record:

- **Same as the kit:** left alone.
- **Not changed since the last install:** replaced with the kit's new version without asking. This is how updates to the kit reach you, even if you press Enter or use `--yes`.
- **Changed by you,** or from an install made before the kit kept a record: the script asks before replacing it, and leaves it alone with `--yes` unless you add `--update`.

Your personal rules (`copilot-instructions.md`) and the repo's notes (`AGENTS.md`) are files you are meant to edit. If you have changed yours, or it is from an install made before the record, the script asks you to choose:

- **Merge** (the default): keeps your file and updates only the section that lists the installed agents. Other changes to the kit's rules do not reach a merged file; choose overwrite to get them.
- **Overwrite:** replaces your file with the kit's. For a repo, that means a blank template again.

Whenever a file is replaced, the old one is kept next to it with the date and time and `.bak` added to its name, such as `AGENTS.md.20261116-153000.bak`, so a second update does not overwrite the first backup. Don't commit the `.bak` files.

## Update or remove

- **Update:** download the new version of this repo and run the same scripts again. Files you have not changed since the last install are updated without asking; see [If you have installed before](#if-you-have-installed-before).
- **Remove:** delete the files the script listed when it installed them, any `.bak` files next to them, and `.copilot-kit.json`.

## Running without questions

Add flags to answer the questions in advance, for example to set up many repos at once. Run a script with `--help` to list them.

| Flag | What it does |
|---|---|
| `--agents <names>` | Which agents to install: names, `all` or `none`. |
| `--packs <names>` | Which packs to install: names, `all` or `none`. |
| `--skills <names>` | Single skills, by name. On its own, installs only those skills, with no pack and no language guide. With `--packs`, adds them to the packs' skills. |
| `--update` | Also replaces files you changed, or from an install before the record, without asking. Files you have not changed are replaced anyway. |
| `--rules merge` or `--rules overwrite` | Answers the merge-or-overwrite question for a rules file you changed. Merge is used if you leave it out. |
| `--yes` | Asks nothing and takes every suggestion, except where another flag says otherwise. |

A script run outside a terminal (for example by another program) also asks nothing, as with `--yes`.

Examples:

```
python install-user.py --agents worker,reader --packs documents --yes
python install-repo.py ~/code/my-service --packs java --skills csv-analysis --yes
```

To install into the repo you are in, run the script from that repo by the kit's path: `python /path/to/kit/install-repo.py .`. The script refuses to install into the kit's own repo.
