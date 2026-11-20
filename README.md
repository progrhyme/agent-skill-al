# al - alias launcher for prompts and skills

An agent skill that lets you register prompts or skills under short names in a
`.al.yaml` file and run them by name.

```
/al                   # list the registered entries
/al review            # run the "review" entry
/al review src/       # extra text is passed to the entry as arguments
/al review: Review the staged diff and point out risks before I commit.
                      # register (or replace) an entry
```

In Codex, the prefix is `$al` instead of `/al`.

## Configuration

Entries are kept in `.al.yaml`:

- **Project file**: `.al.yaml` in the project root
- **Global file**: `~/.al.yaml`

```yaml
aliases:
  review: Review the staged diff and point out risks before I commit.
  daily-report: ~/prompts/daily-report.md
```

A value is either instruction text, or a path (e.g. `~/...`, `./...`, `*.md`)
to a file or skill directory whose contents the agent reads and follows.

If the same name exists in both files, the project entry wins.
Use `--global` to list, resolve or register in `~/.al.yaml` only:

```
/al --global
/al --global daily-report
/al --global notes: Summarize today's changes as bullet points.
```

See [al/SKILL.md](al/SKILL.md) for the full specification.

## Installation

Copy the `al` directory into the skills directory of your AI tool.
The location depends on the tool; for example:

```sh
git clone https://github.com/progrhyme/agent-skill-al.git
cp -R agent-skill-al/al ~/.claude/skills/al    # Claude Code (user-wide)
```

For other tools, copy it to the user-wide or project-level skills directory
described in that tool's documentation.

## License

[MIT](LICENSE)
