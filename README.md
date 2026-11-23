# Idenplane Cloud node artifacts

What a new Idenplane Cloud tenant server downloads while it provisions itself:

| Artifact | Where | Built from |
|---|---|---|
| The node agent (`idenplane-agent-linux-amd64`, `-arm64`) | [Releases](https://github.com/idenplane/node/releases), tag `agent-v<version>` | the private control-panel repository, `scripts/release/build-agent.sh` (reproducible: two builds of the same source give the same bytes) |
| The tenant PostgreSQL image (official PostgreSQL + pgBackRest) | `ghcr.io/idenplane/node-postgres` | [`images/postgres/Dockerfile`](images/postgres/Dockerfile), by the `postgres-image` workflow |

Nothing here is secret. Public on purpose: servers download these without
credentials. Integrity does not come from where they are hosted: a server runs
the agent only if its SHA-256 matches the one it was configured with, and the
PostgreSQL image only by the digest listed in a release catalog signed offline.
A changed file is refused.

Each release lists the SHA-256 of every binary (`SHA256SUMS`) and the entry
for the signed agent releases document (`release.json`).
