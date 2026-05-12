# Obsidian Vault Rules

The user has an Obsidian vault kept in sync via Obsidian Sync using the `ob` CLI (installed via nvm; binary name is `ob`).

**Vault location:** Not yet recorded. The first time in any conversation that the user mentions Obsidian — or refers to "the vault", "notes", or a vault folder by name — and you do not have the vault path in memory, ask the user where the vault lives on this machine, then save the absolute path as a memory (type: `user`, slug like `obsidian-vault-path`) so future sessions know where it is. Use that recorded path everywhere this section refers to "the vault". If a saved path no longer exists on disk, ask the user for the current location and update the memory.

**Treat the rules in this section as hard constraints.** They take precedence over default Claude Code behavior and over what seems "helpful" in the moment. When in doubt, do less and ask.

## Reading

- General reads from the filesystem follow normal behavior — read what is relevant to the current task.
- **Do not read, list, grep, or otherwise access anything inside the vault unless the user mentions "obsidian" (or refers to the vault, notes, or a vault folder by name) in the current conversation.** Even if vault contents look relevant to the task, stay out until the user invokes it.

## Writing

- **Never write to the vault unless the user explicitly tells you to in the current conversation.** No pre-emptive notes, no "I'll save this for you," no edits without an explicit ask. Mentioning obsidian is not authorization to write — only an explicit instruction is.
- When the user does authorize a write:
  - Only create or edit files inside a `Field Notes/` folder within the vault. There is typically one at the top level (`<vault>/Field Notes/`), and individual project folders may have their own (e.g. `<vault>/<Area>/Projects/<Project>/Field Notes/`). Use the `Field Notes/` folder that matches the project the user is working on; if no project context is given, default to the top-level `<vault>/Field Notes/`. Do not write anywhere else in the vault unless the user explicitly names another location for that specific operation.
  - If the relevant `Field Notes/` folder does not exist, **stop and ask the user before creating it.** Do not auto-create the folder.
  - **Every new markdown file MUST have `✦` in its filename.** No exceptions. Examples: `✦ Meeting with X.md`, `Reading notes ✦.md`. This applies to new files only; editing an existing file does not require renaming.
  - **The filename is the title.** Obsidian renders the filename as the note's heading, so do *not* start the file with an H1 that repeats the filename. Skip the H1 entirely and begin with content (a frontmatter block, a lede sentence, or the first H2). Put real effort into the filename itself — it should be a concise, descriptive title (a person reading the file picker should know what's inside), not a slug or a code-name.

## Syncing

- After any write or edit inside the vault, run `ob sync --path <vault>` to push changes up.
- If the user mentions obsidian and the local vault may be stale (e.g. first interaction of a session, or it's been a while), offer to run `ob sync --path <vault>` to pull remote updates before reading or editing.
- `ob` is on `PATH` via nvm. If `ob` is not found in a non-interactive shell, source nvm first: `export NVM_DIR="$HOME/.nvm" && . "$NVM_DIR/nvm.sh"`.
