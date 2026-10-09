# zllm-scripts

Zero-dependency Java 25 single-file scripts that LLM skills call as shell commands. See [README.md](README.md) for the focus and the script list.

## Rules

- follow the `/java-cli-script` skill: shebang `#!/usr/bin/env -S java --source 25`, no `.java` extension, camelCase filename, `chmod +x`, `java.base` only
- bump `VERSION` (`YYYY-MM-DD.N`) on every change
- keep the focus: read-only where possible, no network, no spawned processes, confined to the root, secrets denied, bounded output, documented exit codes
- the guard code in `filesystem/` (`root`, `starts`, `walk`, `visit`, `Scope`, `Skip`, `Walk`, `Glob`, `Selection`) is copied verbatim between scripts — change every copy together
- add new scripts to the README

## Test

Run the script from a repository and from a plain directory; check `-help`, `-version`, the exit codes, and the denied cases (secrets, `.git/`, paths and symlinks leaving the root).
