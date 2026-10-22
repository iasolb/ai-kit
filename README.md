# ai-kit

Small standalone tools for working with AI coding agents, taken from a setup
that runs Claude Code and opencode agents every day. Each file in `tools/` runs
on its own with plain Python 3 (tested on 3.13) and carries a `--self-test`.
These are the working copies, rough edges included, kept in sync with the
private originals automatically.

## Tools

| tool | what it does for you |
|---|---|
| `gatefloor.py` | Runs a test gate and fails it unless at least N checks passed, so an agent cannot go green by deleting tests. |
| `gate-selftest.py` | Counts the assertions a self-test really ran; one that printed nothing counts as zero, not as a pass. |
| `check-confined.py` | Checks that a branch's diff stayed inside the files a task was allowed to touch. |
| `check-tool-contracts.py` | Finds scripts that parse another script's human-readable output, and assertions hard-coded to pass. |
| `summarize-log.py` | Summarizes a long log with a cheap hosted model over plain HTTP (needs `ZEN_API_KEY`). |
| `prose-stats.py` | Word and dash counts for prose files, for keeping agent instructions short. |
| `toon.py` | Encodes JSON as TOON, a compact text format for handing structured data to a model (a list of same-shaped records becomes one header plus rows), decodes it back, and `check` proves a file round-trips without dropping a key. One file: `from toon import encode, decode`. |
| `find-duplicates.py` | Finds the same paragraph written in two places across your markdown (agent instructions, docs, notes), skipping code blocks, tables and text marked superseded. Run it with `--root <repo> --paths docs rules`; needs `toon.py` beside it. |
| `litter-check.py` | Lists the files that said they were temporary and outlived it: an `expires: YYYY-MM-DD` line near the top, a handoff brief superseded by a newer one, a stray script at the repo root. Reports, never deletes. `--root <repo>`; its lane check needs the setup it came from and says so when skipped. |

Try one: `python tools/gatefloor.py --self-test`

## The layout this repo maps

One shared memory bank (private) plus a public config workbench per tool, as
git submodules: `claude-workbench` (Claude Code config template),
`opencode-workbench` (opencode config template), `n8n-workbench` (n8n
workflows and their compose file). `ai-memory-bank` is private, so fetch only
the public children:

```
git clone https://github.com/iasolb/ai-kit.git
git -C ai-kit submodule update --init claude-workbench opencode-workbench n8n-workbench
```

A submodule checks out detached; `git submodule status` shows each child's
commit.

Siblings: `data-kit` (data tools), `cloud-kit` (machine and git sync),
`dev-kit` (editor config), `research-kit` (the research libraries).
