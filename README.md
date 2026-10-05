# IPCR Forge

A self-hosted forge whose releases live on IPFS: work in **Gitea** as usual, its Actions build your
images, the [IPCR](https://github.com/Yundera/ipcr) engine publishes them under one IPNS name (and
your ENS or DNS name), and every public repository is mirrored to **Radicle**.

```
git push origin v1.0.0
docker pull ipcr.localhost:4767/ipns/metadec.eth/ipcr-hello:1.0.0   # any host running IPCR
docker pull ipcr-<domain>/ipns/metadec.eth/ipcr-hello:1.0.0          # anywhere, through the forge
```

This repository is the add-on; [Yundera/ipcr](https://github.com/Yundera/ipcr) is the engine (the
registry gateway, IPFS import, IPNS naming and admin API), used here as the image
`ghcr.io/yundera/ipcr`.

- **[docs/forge.md](docs/forge.md)**: how it works, the trust model, findings, what was verified.
- **[Apps/IPCR-Forge](https://github.com/Yundera/ipcr/tree/main/Apps/IPCR-Forge)** (in the IPCR repository, with the
  rest of the IPCR app store): the Yundera AppStore listing (compose, rationale, seed). It pins this
  repository's image and the engine's.
- **[forge/](forge/)**: the forge service, `ghcr.io/yundera/ipcr-forge` (Go, standard library only):
  the Gitea → Radicle mirror, the forge's page and admin pages (Gitea login), the staging registry's
  gate and per-repository push credentials, and the install step.
- The standalone listings the forge is built from, [Gitea](https://github.com/Yundera/ipcr/tree/main/Apps/Gitea) and [Radicle](https://github.com/Yundera/ipcr/tree/main/Apps/Radicle), are in
  the same store.

## Develop

```sh
cd forge && go vet ./... && go test ./...
docker build -t ghcr.io/yundera/ipcr-forge:dev forge
```

Releases: a `vX.Y.Z` tag publishes `ghcr.io/yundera/ipcr-forge:X.Y.Z`
([.github/workflows/image.yml](.github/workflows/image.yml)). Then bump the version in the
listing, `Apps/IPCR-Forge/docker-compose.yml` in the IPCR repository (the service, `forge-gate`
and the `forge-setup` step).
