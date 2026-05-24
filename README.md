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

- **Pinned ref.** Once the source monorepo ships a tagged release with
  built artifacts under `plugins/claude/telegram/`, add a `ref` field
  (branch or tag, e.g. `"ref": "v0.1.0"`) and/or `sha` (full 40-char
  commit SHA) to each plugin's `source` block to pin the install.
- **Version bumps.** `version` on each plugin entry must track the
  source monorepo's release tag for that `(agent, comm)` pair. Note:
  when set on a marketplace entry, this string overrides any `version`
  declared in the plugin's own `plugin.json` at install time.
- **Validate before push.** Run `claude plugin validate .` (or
  `/plugin validate .` inside Claude) to catch schema errors before
  publishing.

## Related docs (in source monorepo)

- `docs/research/install-model.md` — full distribution model rationale.
- `docs/research/dist-tree-plan.md` — canonical directory sketch this
  repo follows.
