# turtle-agent

[![test](https://github.com/pjerra/turtle-agent/actions/workflows/test.yml/badge.svg)](https://github.com/pjerra/turtle-agent/actions/workflows/test.yml)

A headless World of Warcraft client for **vanilla 1.12 / Turtle WoW 1.18.1** servers (login build `7272`, world build
`5875`). One process logs one character in with no game window and exposes it over a local JSON-RPC API to drive,
record, and test [TortoiseBots](https://github.com/TortoiseBots/TortoiseBots) parties.

> [!WARNING]
> **Test servers only.** Use this only on servers you run or are explicitly allowed to test. It has no Warden emulation
> and does no rendering.

Testing TortoiseBots with it? See [`docs/TORTOISEBOTS-VISION.md`](docs/TORTOISEBOTS-VISION.md) and [`AGENTS.md`](AGENTS.md).

## Quick start

Requires **Go 1.27.1+** and a **C++17 compiler** (`CGO_ENABLED=0` builds without navmesh pathing).

```bash
make build                      # bin/turtle-agent
make test                       # gofmt, go vet, builds, go test -race ./...
```

Create `~/.config/turtle-agent/settings.json` (mode `0600`; `rpc` must be loopback — see
[settings reference](docs/okf/getting-started/settings-reference.md)):

```json
{
  "realmd": "<server>",
  "accounts": { "<account>": "<password>" },
  "rpc": "127.0.0.1:8765"
}
```

Log in and query the session over RPC (see [Drive the agent with RPC](docs/okf/getting-started/drive-rpc.md)):

```bash
bin/turtle-agent login --rpc 127.0.0.1:8765 <account> <character>
bin/turtle-agent rpc <slot-dir> status
```

## Documentation

The full manual lives under [`docs/okf/`](docs/okf/index.md):

| Area | Topics |
|---|---|
| [Getting started](docs/okf/getting-started/index.md) | [Settings reference](docs/okf/getting-started/settings-reference.md), [Drive the agent with RPC](docs/okf/getting-started/drive-rpc.md) |
| [Agent & RPC](docs/okf/agent/index.md) | [RPC reference](docs/okf/agent/rpc-reference.md) (including `gm_aura` and slot controls) |
| [Dungeons & raids](docs/okf/dungeons/index.md) | [Dungeon runs](docs/okf/dungeons/index.md), [Raids](docs/okf/raids/index.md), [Roster format](docs/okf/raids/roster-format.md), [Raid-walk reference](docs/okf/raids/raid-walk-reference.md), [Plans](docs/okf/plans/index.md) |
| [Navigation, combat & observability](docs/okf/navigation/index.md) | [Navigation](docs/okf/navigation/index.md), [Combat](docs/okf/combat/index.md), [Observability](docs/okf/observability/index.md), [Conventions & map](docs/okf/about/readme-map.md) |

## Development

- `make test` must stay green (`gofmt`, `go vet`, no-cgo build, `go test -race ./...`, cgo build, `staticcheck ./...`; see [`docs/CI.md`](docs/CI.md)).
- Open PRs against **`dev`** (`main` only takes `dev` after review).

## Licence

Licensed under **AGPL-3.0-or-later** ([`LICENSE`](LICENSE)). Earlier MIT-licensed versions remain under MIT
([`LICENSES/MIT-turtle-agent-before-agpl.txt`](LICENSES/MIT-turtle-agent-before-agpl.txt)). Third-party notices
(`agent-wow`, Detour/recastnavigation, `mod-dungeon-clear`) are listed in [`NOTICE`](NOTICE).

This is an unofficial community project and is not affiliated with Blizzard Entertainment.
