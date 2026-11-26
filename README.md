# nings-skills

Reusable, evidence-driven engineering workflows for Codex Desktop and CLI, including GPU systems, distributed computing and LLM workloads.

## Skills

| Skill | Trigger |
| --- | --- |
| `plan-first` | Non-trivial implementation, architecture changes and refactoring |
| `systematic-debug` | Bugs, incorrect results, numerical errors, hangs and failures |
| `performance-profile` | Latency, throughput, memory, GPU kernels and communication bottlenecks |

Each directory under `skills/` contains a standalone `SKILL.md`. The root `AGENTS.md` is a suggested global routing and engineering policy; review it before installing globally.

## Install (macOS/Linux)

Clone once on each machine:

```bash
git clone https://github.com/zincsabian/nings-skills.git "$HOME/nings-skills"
mkdir -p "$HOME/.agents/skills"
for skill in plan-first systematic-debug performance-profile; do
  src="$HOME/nings-skills/skills/$skill"
  dst="$HOME/.agents/skills/$skill"
  if [ ! -f "$src/SKILL.md" ]; then
    printf 'Missing source: %s\n' "$src" >&2
    continue
  fi
  if [ -e "$dst" ] || [ -L "$dst" ]; then
    printf 'Skipped existing path: %s\n' "$dst"
  else
    ln -s "$src" "$dst"
  fi
done
```

Review skipped paths; do not overwrite existing skills. Alternatively copy the skill directories into `~/.agents/skills/` (copies need to be refreshed manually). For project-only installation, use `.agents/skills/` inside the project.

## Optional global instructions

Review `AGENTS.md` in this repository and merge it with your existing `${CODEX_HOME:-$HOME/.codex}/AGENTS.md`. If no global instructions exist, copy it:

```bash
codex_home="${CODEX_HOME:-$HOME/.codex}"
mkdir -p "$codex_home"
if [ -e "$codex_home/AGENTS.override.md" ] || [ -L "$codex_home/AGENTS.override.md" ]; then
  printf 'Review AGENTS.override.md first; it may take precedence.\n'
elif [ -e "$codex_home/AGENTS.md" ] || [ -L "$codex_home/AGENTS.md" ]; then
  printf 'Existing AGENTS.md: merge manually.\n'
else
  cp "$HOME/nings-skills/AGENTS.md" "$codex_home/AGENTS.md"
fi
```

Do not sync the entire Codex home directory or credentials.

## Update and sync

Run on each machine with a clean checkout:

```bash
git -C "$HOME/nings-skills" pull --ff-only
```

Symlinked skills reference the updated checkout. Manually merged or copied global instructions must be updated separately. Desktop and CLI on the same host/user normally share user-scope skills; remote servers need their own installation. Start a new Codex session to check discovery.

## Verify

In a fresh Codex CLI session, use the skill picker (`$`) or `/skills` where supported. Test explicit invocation:

- `$plan-first` — design a change without editing code.
- `$systematic-debug` — reproduce and isolate a numerical discrepancy.
- `$performance-profile` — establish a baseline and analyze the critical path.

No GPU tools are bundled or installed by these skills. Availability and actual execution must be checked in the target environment.

## References

- [Codex skills](https://developers.openai.com/codex/skills/)
- [Codex AGENTS.md](https://developers.openai.com/codex/guides/agents-md/)
