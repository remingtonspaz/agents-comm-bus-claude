# agents-comm-bus-claude

Distribution endpoint for the **agents-comm-bus** plugin family on
Claude Code. This repo is intentionally thin: it only contains
`.claude-plugin/marketplace.json`. The actual plugin artifacts (daemon
bundle, comm adapter, hooks, MCP shim, skill) are built and stored in
the source monorepo at `plugins/claude/<comm>/`, and the marketplace
manifest references them via git-subdir.

## Layout

```
agents-comm-bus-claude/
└── .claude-plugin/
    └── marketplace.json
```

## How users install

```text
/plugin install agents-comm-bus-telegram
```

Claude Code resolves the entry in `marketplace.json`, fetches the
referenced subdirectory from the source monorepo at the pinned ref,
and extracts it into `~/.claude/plugins/agents-comm-bus-telegram/`.
First prompt after install triggers the plugin's install hook, which
bootstraps the per-user `agents-comm-bus` daemon and registers the
Telegram comm adapter.

## Plugins covered

| Plugin | Status |
|---|---|
| `agents-comm-bus-telegram` | v1 target (migration anchor) |
| `agents-comm-bus-matrix` | planned |
| `agents-comm-bus-discord` | planned |
| `agents-comm-bus-slack` | planned |

## Companion repos

- **Source monorepo:** `remingtonspaz/agents-comm-bus`. Holds daemon
  source, comm adapter sources, Claude host edge (`hosts/claude/`),
  and built artifacts under `plugins/claude/<comm>/`.
- **Codex marketplace:** `remingtonspaz/agents-comm-bus-codex`. Same
  shape as this repo, but with `.agents/plugins/marketplace.json` per
  Codex's plugin convention.

## Open scaffold notes (pre-first-push)

- **`source` schema.** The `source` block in `marketplace.json` uses
  `{"source": "github", "repo": "...", "path": "..."}`. Validate
  against the live Claude Code marketplace schema before publishing —
  the exact field names for git-subdir references should be checked
  against current Claude Code marketplace docs.
- **Pinned ref.** Once the source monorepo ships a tagged release with
  built artifacts under `plugins/claude/telegram/`, add a `ref` (tag
  or branch) field to the `source` block to pin the plugin version.
- **Version bumps.** `version` on each plugin entry must track the
  source monorepo's release tag for that `(agent, comm)` pair.

## Related docs (in source monorepo)

- `docs/research/install-model.md` — full distribution model rationale.
- `docs/research/dist-tree-plan.md` — canonical directory sketch this
  repo follows.
