# Copilot agent kit

This kit makes GitHub Copilot more careful and more useful in your work. Once it is installed:

- Copilot says a change is done only after it has run it and seen it work, and it never skips a test to get a pass.
- In a repo set up with `install-repo.py`, a safety hook stops Copilot from force-pushing, running `git reset --hard`, running `rm -rf` on the root, your home or the whole working folder, and reading `.env` and private key files. It stops only those; see [kit/templates/README.md](kit/templates/README.md#the-safety-hook).
- You can ask Copilot to review your changes before a pull request, check a change works, answer questions about a spreadsheet, or read a PDF or Word file.

Installing it is optional and up to you. Nothing is added to any repo unless you run the repo script.

## Quick start

You need Python 3.8 or later. In the commands below, type `python3` or `py` instead of `python` if that is how Python starts on your machine.

1. Download this repo and open a terminal in its main folder.
2. Set up Copilot for yourself. You only do this once. Press Enter at every question.
   ```
   python install-user.py
   ```
3. Set up a repo. Do this once for each repo, with the path to that repo. Your agents and skills already come from step 2, so this installs only the repo's notes, the safety hook and the language guide. Replace `java` with the repo's language (`java`, `node`, `scala` or `flutter`), or with `none` if it has none of these. Press Enter at every question.
   ```
   python install-repo.py ~/code/my-service --agents none --packs java
   ```
   Agents and skills installed in both places load twice, and a repo's copy of a skill is used instead of yours even when it is older. Install them into a repo only if your team wants them shared and versioned with it: see [INSTALL.md](INSTALL.md#install-each-item-at-one-level).
4. Open that repo with Copilot and type: **set up this repo for Copilot**. Copilot fills in the repo's notes by reading its code, and asks you what it can't work out.
5. Start a new Copilot session, so Copilot reads the new files.
6. Commit the repo's new files if your team should have them too, but not any `.bak` files.

Then ask Copilot in plain words, as you would ask a colleague: "verify my change", "review my changes", "summarise report.pdf".

## Find your way around

| To find out | Read |
|---|---|
| What the words mean (agent, skill, pack, session) | [GLOSSARY.md](GLOSSARY.md) |
| What the install scripts ask, and how to update or remove the kit | [INSTALL.md](INSTALL.md) |
| Where the kit works, how to check it, and what to do if something seems wrong | [CHECK.md](CHECK.md) |
| What each agent does and what to ask it | [kit/agents/README.md](kit/agents/README.md) |
| What each skill does and what to ask it | [kit/skills/README.md](kit/skills/README.md) |
| The coding advice for Java, Scala, NestJS and Flutter | [kit/instructions/README.md](kit/instructions/README.md) |
| Your personal rules, the repo's notes and the safety hook | [kit/templates/README.md](kit/templates/README.md) |
