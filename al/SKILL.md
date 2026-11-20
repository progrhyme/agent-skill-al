---
name: al
description: Register and run named prompts or skills defined in a .al.yaml file. Use when the user invokes this skill to run a registered entry ("/al <name>" in Claude Code, "$al <name>" in Codex), to register one ("<name>: <content>" following the invocation), to list the registered entries (the bare invocation, or with --global), or asks to manage entries in .al.yaml. Entries live in .al.yaml in the project root and in ~/.al.yaml; the project file wins unless --global is given.
---

# al — named prompt and skill launcher

`al` lets the user register arbitrary prompts or skills under short names in a
`.al.yaml` file and run them by name.

## Configuration files

Two files may exist:

- **Project file**: `.al.yaml` in the project root (the top-level directory of
  the current workspace or repository).
- **Global file**: `.al.yaml` in the user's home directory (`~/.al.yaml`).

### File format

Each file is a flat YAML mapping under `aliases` from a registered alias to content:

```yaml
aliases:
  review: Review the staged diff and point out risks before I commit.
  daily-report: ~/prompts/daily-report.md
```

The value is either:

- **Instruction text** — a prompt or instruction to follow as-is, or
- **A path** — a file (or a skill directory) whose contents define what to do.
  If the value looks like a path (starts with `~/`, `/`, `./`, or `../`, or
  ends with a file extension such as `.md`), read it and follow its contents.

Write single string value per alias.
Multi-line values are allowed but nested structures are not.

## Invocation forms

The invocation prefix belongs to the host: `/al` in Claude Code, `$al` in
Codex, and other hosts may differ. The grammar after the skill name is the
same everywhere; the forms below are written with `/al`. Handle these forms:

### List: `/al`

With no name and no arguments, list the current configuration instead of
running anything:

1. Collect the entries that are in effect (see "Resolution").
2. Show each alias with its value.
3. If there are none, say so.

### Run: `/al <name> [arguments]`

1. Resolve `<name>` (see "Resolution" below).
2. If the value is a path, read the target first.
3. Carry out the instruction or skill. Treat any text after `<name>` as
   arguments or additional instructions for that entry, and apply them to
   the task.
4. If `<name>` is not found in the applicable file(s), say so and list the
   registered names that are available.

### Register: `/al <name>: <content>`

1. Add or replace the mapping `<name>: <content>` in the project `.al.yaml`
   (create the file if it does not exist).
2. Preserve all existing entries and the flat format defined above. If the
   content contains characters that would break plain YAML (such as a colon
   followed by a space), quote the value.
3. Confirm what was registered and in which file.

### The `--global` flag

- `/al --global` — list the entries in the global file only.
- `/al --global <name> [arguments]` — run `<name>` resolved from the global
  file only (see "Resolution").
- `/al --global <name>: <content>` — register into `~/.al.yaml` instead of
  the project file.

## Resolution

- By default, look in the project file first, then the global file.
- If the same name exists in both, the project entry wins.
- With `--global`, only the global file is consulted.
