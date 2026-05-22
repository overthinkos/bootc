# overthinkos/bootc

The **bootc bootable-container image family** for [Overthink](https://github.com/overthinkos/overthink),
split into its own repository and mounted as a git submodule at `image/bootc`
of the main repo.

## What's here

| Kind | Entries |
|---|---|
| `image:` | `selkies-desktop-bootc` (Fedora bootc + Selkies streamed desktop — the canonical worked example), `bazzite` (ublue Bazzite NVIDIA AI/dev workstation), `aurora` (ublue Aurora DX + ov toolchain), `openclaw-browser-bootc` (Fedora bootc + OpenClaw gateway) |
| `vm:` | the matching `kind: vm` bootc entities (`selkies-desktop-bootc-bootc`, `bazzite-bootc`, `aurora-bootc`, `openclaw-browser-bootc-bootc`) |

All images ship `enabled: false` — build them with `--include-disabled`.

## Composition by reference — nothing is vendored

This repo contains **no layers, no build-config, and no base of its own**.
Everything is pulled from `github.com/overthinkos/overthink` by **github
reference**:

- every layer in `image.yml` is an `@github.com/overthinkos/overthink/layers/<name>:<tag>` ref;
- the shared build-config (`build.yml` — distro/builder/init, including the
  `fedora` distro definition + the `rpm` format template) is a remote `include:`;
- `fedora-base.yml` is a remote `include:` solely to bring `fedora-builder` into
  scope (the pixi/npm builds in `selkies-desktop`/`openclaw` need a builder, and
  bootc images root on external bases so they inherit no builder map).

### Three tag pins, each with a reason

| Pinned | Tag | Why |
|---|---|---|
| Every layer except `os-system-files`/`ujust`; `build.yml` | `v2026.141.1600` | the ecosystem layer tag (must match `fedora-base.yml`'s internal layer pins) |
| `fedora-base.yml` file include | `v2026.141.2308` | the tag where the file first exists; its internal layer refs are `v2026.141.1600` |
| `os-system-files` + `ujust` | `v2026.142.0552` | the fresh tag carrying the renamed `/usr/share/bazzite/` paths (bazzite-ai → bazzite) |

## The debian/ubuntu pattern — no base in main, no coupling

Unlike fedora/arch (whose in-repo bases ~40 main images depend on), **every bootc
image roots on an EXTERNAL upstream base URL** (`quay.io/fedora/fedora-bootc:43`,
`ghcr.io/ublue-os/…`). So there is **no in-repo bootc base image** to keep, and
nothing in main consumes any image here. The only edge is `bootc → main`; main
remote-includes nothing from this repo (**zero main ↔ bootc coupling**, like
debian/ubuntu).

## Build

```bash
# Inside the submodule (the build verb defaults to overthink.yml):
ov image build selkies-desktop-bootc --include-disabled

# From the parent overthink repo:
ov -C image/bootc image build selkies-desktop-bootc --include-disabled

# Standalone, against the published repo:
ov --repo overthinkos/bootc image build selkies-desktop-bootc --include-disabled
```

The first build resolves the upstream github references into `~/.cache/ov/repos/`
and materializes the referenced layers under `.build/_layers/`. A `kind: vm`
entity then turns a built bootc image into a QCOW2 disk via
`ov vm build <name> --transport containers-storage`.

## Requirements

A build of any image here fetches from the upstream repo, so it needs network
access and an `ov` recent enough to understand the config's schema version.

---
*Assisted-by: Claude*
