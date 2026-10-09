# zllm-scripts

Zero-dependency Java 25 single-file scripts that LLM skills call as shell commands. See [README.md](README.md) for the focus and the script list.

## Rules

- follow the `/java-cli-script` skill: shebang `#!/usr/bin/env -S java --source 25`, no `.java` extension, camelCase filename, `chmod +x`, `java.base` only
- bump `VERSION` (`YYYY-MM-DD.N`) on every change
- keep the focus: read-only where possible (the invocation log is the only write), no network, no spawned processes, confined to the root, secrets denied, bounded output, documented exit codes
- the guard code in `filesystem/` (`root`, `starts`, `walk`, `visit`, `Scope`, `Skip`, `Walk`, `Glob`, `Selection`) is copied verbatim between scripts — change every copy together
- every script logs each invocation to `~/zllm-scripts/zllm-scripts-invocations.log` through the `Trace` interface, copied verbatim into all scripts — change every copy together; call `Trace.invocation(...)` first in `main`/the boundary method and exit through `Trace.exit(code)` (`Log.exit(Exit)` in the BCE scripts) so the exit code is logged; never log argument content that may be secret or bulky (`clip` logs the length only)
- add new scripts to the README

## Test

Run the script from a repository and from a plain directory; check `-help`, `-version`, the exit codes, the denied cases (secrets, `.git/`, paths and symlinks leaving the root), and that each run appended one line with the right exit code to `~/zllm-scripts/zllm-scripts-invocations.log`.
