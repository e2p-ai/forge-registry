# forge community registry

The discovery index for [forge](https://github.com/e2p-ai/forge) extensions,
skills, personas, and commands. Think of it as a Homebrew tap: a single
`registry.toml` that `forge registry` reads to search and one-command-install
community contributions.

```
forge registry search <query>     # find something
forge registry install <name>     # install it
forge registry list               # installed + available
```

By default forge reads this repo (`e2p-ai/forge-registry`). Point it elsewhere
with the `FORGE_REGISTRY` environment variable or the `[registry] default` key in
`~/.forge/config.toml`.

## What's in here

| name                 | kind      | what it does |
|----------------------|-----------|--------------|
| `hello-forge`        | extension | A hello-world command — proof the install path works end to end. |
| `reviewer`           | persona   | A skeptical code-review persona. |
| `pr-review`          | extension | A two-step recipe that reviews a GitHub PR. |
| `hello-forge-plugin` | plugin    | The native sample dylib plugin (needs `--trust`). |

## Registry format

`registry.toml` is a list of `[[entry]]` tables:

```toml
[[entry]]
name               = "my-thing"        # unique slug [a-zA-Z0-9_-]
kind               = "extension"       # extension | plugin | skill | persona | command
description        = "one line"
repo               = "you/your-repo"   # owner/name on GitHub
path               = "path/in/repo"    # file for command/persona, dir otherwise
license            = "Apache-2.0"      # SPDX id — shown before install
sha256             = "..."             # optional: pins the entry's primary file
min_forge_version  = "0.4.0"           # optional
```

### Where each kind installs

| kind      | source `path`                         | installs to |
|-----------|---------------------------------------|-------------|
| extension | dir with a `forge-extension.toml`     | recipes/commands/plugins per the manifest |
| command   | a single `.md` file                   | `~/.claude/commands/<name>.md` |
| persona   | a single `.md` file                   | `~/.claude/agents/<name>.md` |
| skill     | a dir with `SKILL.md`                 | `~/.claude/skills/<name>/` |
| plugin    | a dir (native dylib source)           | `~/.forge/plugins/<name>/` — **only with `--trust`** |

## Trust model

Installing runs on your machine, so the registry is conservative:

- **Allowlist.** `forge registry install` only fetches from repos matching your
  `[registry] allow` globs. The default is the official org (`e2p-ai/*`). Adding
  others prints a warning — only allow repos you trust.
- **License + repo shown first.** Every install prints the entry's repo and
  license before fetching anything.
- **sha256 pinning.** When an entry sets `sha256`, forge verifies the fetched
  primary file against it and refuses on mismatch.
- **Native plugins are gated.** A `plugin` is compiled code that forge dlopens
  and runs in-process with no sandbox. Installing one requires an explicit
  `--trust` flag after you've reviewed the source.

## Submit an entry

1. Fork this repo.
2. Add your asset (in this repo under `extensions/`, `personas/`, etc., or leave
   it in your own repo and point `repo`/`path` at it).
3. Append an `[[entry]]` to `registry.toml`.
4. Open a PR. Keep the description to one line; make sure `license` is accurate.

Contributions that pin `sha256` and set `min_forge_version` are preferred.
