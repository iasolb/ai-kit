# ai

Single home for all AI tooling on this machine. Mirrors the public `ai-toolkit`
repo view on GitHub; one machine, one brain.

## Layout

```
ai/
├── ai-memory-bank/        # clone of PRIVATE repo; the one shared brain
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

## Rules

- Memory bank is cloned **once per machine**, never once per tool.
- Workbenches are the only thing that writes to config locations
  (`~/.claude`, `~/.config/opencode`); they contain install scripts for
  Windows and macOS.
- This README is the local mirror of the public `ai-toolkit` map repo.

## GitHub map

| Repo | Visibility | Purpose |
|---|---|---|
| `ai-toolkit` | public | top-level map / storage umbrella |
| `ai-memory-bank` | private | shared memory, all models reference it |
| `claude-workbench` | public | Claude config + install scripts |
| `opencode-workbench` | public | opencode config + install scripts |
