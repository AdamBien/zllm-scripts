# zllm-scripts

Zero-dependency Java 25 scripts that LLM skills call as shell commands. Each script is a single executable file with a shebang, needs only a JDK, and can be shared by any number of skills.

## Focus

The scripts give agents predictable, bounded tools in place of general-purpose shell commands:

- read-only wherever the task allows — no writes, no network, no spawned processes
- file access confined to a root — the nearest directory containing `.git`, otherwise the current directory; paths and symlinks leaving it are refused
- secrets denied: `.env`, `*.pem`, `*.key`, `.npmrc`, `id_rsa*`, `.git/` and similar are never read
- bounded output: entry and line caps, long lines cut, truncation reported on stderr with a continuation hint
- results on stdout, diagnostics on stderr, non-zero exit on failure — the filesystem scripts use 0 found, 1 nothing found, 2 usage error, 3 denied
- `-help` and `-version` on every script that takes arguments

## Prerequisites

Java 25+

## Scripts

### [filesystem](filesystem)

Read-only replacements for `find`, `grep` and `cat`, for repositories and plain directories alike. Generated and dependency directories (`node_modules`, `dist`, `target`, ...) are skipped unless `-all`.

- `files [dir...] [glob...]` — sorted root-relative paths; `-type d`, `-long` (size and line count), `-count`, `-exclude`
- `search <regex> [path...]` — `path:line:text`; `-glob`, `-exclude`, `-i`, `-fixed`, `-context`, `-count`, `-files`
- `show <file...>` — numbered lines; `-lines 120-180`, `-max`

### [clipboard](clipboard)

- `clip <text>` — copies the argument or stdin to the system clipboard
- `paste` — prints the clipboard to stdout

### [skills](skills)

- `parseFrontmatter <skill-file-or-directory>` — prints the parsed frontmatter of a `SKILL.md`
- `quickValidate <skill-directory>` — validates `SKILL.md` frontmatter (allowed keys, `name`, `description`, kebab-case name); exit 0 valid, 1 invalid

## Install

```
chmod +x filesystem/* clipboard/* skills/*
sudo cp filesystem/* clipboard/* skills/* /usr/local/bin/
```

Symlink instead of copying to pick up edits:

```
for s in filesystem/* clipboard/* skills/*; do sudo ln -s "$(pwd)/$s" /usr/local/bin/; done
```

Allow the read-only scripts in Claude Code permissions (e.g. `Bash(files:*)`, `Bash(search:*)`, `Bash(show:*)`) so skills use them without per-command approval.
