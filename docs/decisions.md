# Decisions

What was decided about the kit, when and why. Read the entry for a subject before changing it.

## The installers record a hash of every file they write (2026-11-16)

**Decision.** Each install writes a manifest holding a SHA-256 hash per installed file: `.copilot-kit.json` in the personal Copilot folder for `install-user.py`, and `.github/.copilot-kit.json` in the repo for `install-repo.py`. On a later run:

- A file whose content still matches its recorded hash has not been edited, so it is replaced with the kit's new version without asking. The old copy is kept as a timestamped `.bak`.
- A file that was edited, or that has no manifest entry (an install made before the manifest existed), is handled as before: the replace question or `--update`, and merge by default for the rules files.
- A file the installer merged is never recorded, because it holds the user's own text.

**Why.** Before this, a re-run compared the installed file only with the kit's current version. An older kit's personal rules file always differed, so the default was merge, which keeps the whole old file and refreshes only the "Handing work to agents" section. Anyone who pressed Enter or used `--yes` never received a rule change. The installer could not tell "an old kit file" from "a file the user edited"; the hash tells them apart.

**Why merged files are left out.** Recording a merged file's hash would make the next run treat the user's edits as untouched kit text and replace them silently. This happened in testing before the rule was added.

**Why merge stays the default for an edited repo `AGENTS.md`.** Teams fill in and edit that file, so their text must survive an update.

## Agents and skills per user; rules, language guide and hook per repo (2026-11-16)

**Decision.** The Quick start and `INSTALL.md` recommend one level for each item:

- Agents and skills are installed per user with `install-user.py`.
- A repo gets only `AGENTS.md`, its language guide and the safety hook from `install-repo.py`.
- Installing agents and skills into a repo is the choice for a team that wants them shared and versioned with the repo.

**Why.** With both installers run in full, agents and skills exist in two places. The lines that say when to hand work to each agent then load twice in every session, once from the personal rules and once from the repo's `AGENTS.md`, and Copilot bills that text on every model call. A repo's `.github/skills/` also takes priority over the personal copy, so a committed copy that has gone stale silently overrides a newer personal one.

**Why this is in the docs and not in the installer.** The alternative was for `install-repo.py` to skip whatever the person running it already has installed per user. That would make what gets committed to a shared repo depend on whose machine ran the script.

**Left as it is.** Pressing Enter in `install-repo.py` still selects all agents and the skill packs, so the default does not yet match the recommendation. A language guide cannot be installed without its pack's skills, so matching it means separating guides from skills in the installer. Until then the recommended command is `--agents none --packs <language>`.
