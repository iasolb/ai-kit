# ai-kit

The public map of how the AI tooling is laid out on a machine: one shared
memory bank, a config workbench per model, and where each one deploys. For
someone who wants to understand the structure before touching anything.

## Layout

```
ai/
├── ai-memory-bank/        # clone of PRIVATE repo; the one shared brain
├── state/                 # THE LANDING ZONE: machine state, in NO repo
├── claude/
│   ├── claude-workbench/  # clone of PUBLIC config repo -> deploys to ~/.claude
│   ├── dropoffs/          # local inbox: screenshots, phone files, uploads
│   ├── bin/               # reference scripts/binaries Claude invokes
│   └── work/              # active project checkouts
└── opencode/
    ├── opencode-workbench/ # clone of PUBLIC config repo -> deploys to ~/.config/opencode
    ├── dropoffs/          # local inbox
    ├── bin/               # reference scripts/binaries opencode invokes
    └── work/              # active project checkouts
```

## The landing zone, `ai/state/`

Where automation writes what it observes, so a session reads one prepared
file instead of rediscovering the machine every time. Git state per repo,
queue counts, live and leaked ports, running processes, allowance health.

**It is deliberately inside no git repository.** Not here, because this repo
is public and a gitignored directory in a public repo fails silently. Not in
the memory bank, because it is rewritten on every trigger and that means
permanent churn plus conflicts between machines over facts that are not even
shared: this machine's ports say nothing about the other one.

The rule: **if it can be rebuilt on demand, it does not belong in git.**

Every artifact is TOON and carries `produced_at`, so a reader treats it as
true as of a moment rather than as live.

## Rules

- Memory bank is cloned **once per machine**, never once per tool.
- Workbenches are the only thing that writes to config locations
  (`~/.claude`, `~/.config/opencode`); they contain install scripts for
  Windows and macOS.

## GitHub map

| Repo | Visibility | Purpose |
|---|---|---|
| `ai-kit` | public | top-level map / storage umbrella |
| `ai-memory-bank` | private | shared memory, all models reference it |
| `claude-workbench` | public | Claude config + install scripts |
| `opencode-workbench` | public | opencode config + install scripts |
| `n8n-workbench` | public | n8n workflows + config |

## Start here

Read the layout tree above, then `ai/state/`, the landing zone, to see
what this machine looks like right now.
