# Tenant PostgreSQL: the official PostgreSQL image plus pgBackRest.
#
# pgBackRest runs as PostgreSQL's archive_command, so it must live in the same
# container as the server. Everything else is the unmodified official image.
#
# Pins (update together, through a reviewed PR):
#   - base image by digest (postgres:17.12-trixie, linux/amd64+arm64 index)
#   - pgBackRest by exact PGDG package version. 2.60.1 is the first release
#     after the 2.59.3 fix for weak encryption subkeys/salts; do not go below.
#
# The resulting image is referenced from the signed release catalog by digest
# (components.postgres), never by tag.
FROM postgres:17.12-trixie@sha256:b60e90042aa6b8dbecd58e224b3e36114dbf8ad754465264c2476ad58a86e45c

ARG PGBACKREST_VERSION=2.60.1-1.pgdg13+1

# The official image already trusts the PGDG apt repository.
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends "pgbackrest=${PGBACKREST_VERSION}"; \
    rm -rf /var/lib/apt/lists/*; \
    pgbackrest version | grep -q '^pgBackRest 2\.60\.1$'; \
    # Mount points owned by postgres, so fresh named volumes inherit it.
    install -d -o postgres -g postgres -m 0750 /var/spool/pgbackrest /var/lib/idenplane-dumps /var/log/pgbackrest /etc/pgbackrest

LABEL org.opencontainers.image.title="idenplane-node-postgres" \
      org.opencontainers.image.description="PostgreSQL 17 with pgBackRest for Idenplane Cloud tenant nodes" \
      org.opencontainers.image.source="https://github.com/idenplane/node"
