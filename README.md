# ai-kit

The public map of how the AI tooling is laid out on a machine: one shared
memory bank, a config workbench per model, and where each one deploys. For
someone who wants to understand the structure before touching anything.

## Layout, as it actually is

```
ai-kit/
├── ai-memory-bank/         # submodule, PRIVATE repo; the one shared brain
├── claude-workbench/       # submodule, PUBLIC; generalized mirror of the Claude config
├── opencode-workbench/     # submodule, PUBLIC; deploys to ~/.config/opencode
└── n8n-workbench/          # submodule, PUBLIC; n8n workflows + config
```

The children are **submodules**, not loose clones, so a pointer here names one
commit of each. Clone the whole thing with
`git clone --recursive https://github.com/iasolb/ai-kit.git`; an existing clone
picks them up with `git submodule update --init --recursive`.

**A submodule checkout is DETACHED by default, and that is the trap.** On
2026-09-01 every child here came up detached, one of them 18 commits behind the
branch it mirrors, while `git status` looked perfectly clean. Check with
`git submodule status` and look for a branch name in the trailing parentheses.
`ai-memory-bank/tools/sync-pin.py` is the one command that puts a child back on
its branch, brings it current, and moves the pointer here.

`ai-kit` is a sibling of `dev-kit`, `cloud-kit`, `data-kit` and `research-kit`;
on a machine they sit together under one directory (`~/dev` on the Mac).

## Not built yet, and deliberately listed as such

An earlier version of this file drew a `state/`, `dropoffs/`, `bin/` and `work/`
tree as though it existed. **None of it does**, on any machine, which made this
map worse than no map: a reader cannot tell a plan from a fact. The intent
below is still good, and it is intent.

- **`state/`, a landing zone** where automation writes what it observes, so a
  session reads one prepared file instead of rediscovering the machine every
  time: git state per repo, queue counts, live and leaked ports, running
  processes. It would sit **inside no git repository** (not here, because a
  gitignored directory in a public repo fails silently; not in the memory bank,
  because it is rewritten constantly and one machine's ports say nothing about
  the other's). The rule behind it holds regardless: **if it can be rebuilt on
  demand, it does not belong in git.**
- **`dropoffs/`, `bin/` and `work/` per tool.** The drop-off inbox is parked on
  purpose as of 2026-09-01, pending a decision about where session files land.

## Rules

- Memory bank is cloned **once per machine**, never once per tool.
- **The memory bank installs ITSELF** (`ai-memory-bank/install/mac.sh`). The
  workbench installer deploys the workbench, which is the generalized public
  mirror; running it against a personal setup would replace real config with
  template placeholders.

## GitHub map

| Repo | Visibility | Purpose |
|---|---|---|
| `ai-kit` | public | top-level map / storage umbrella |
| `ai-memory-bank` | private | shared memory, all models reference it |
| `claude-workbench` | public | Claude config + install scripts |
| `opencode-workbench` | public | opencode config + install scripts |
| `n8n-workbench` | public | n8n workflows + config |

## Start here

Read the layout tree above, then run `git submodule status` and confirm every
child names a branch. If one is detached, it can be arbitrarily far behind while
looking healthy, and nothing else you read here will be trustworthy.
