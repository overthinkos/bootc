# overthinkos/bootc

The **bootc bootable-container image family** for [Overthink](https://github.com/overthinkos/overthink),
split into its own repository and mounted as a git submodule at `image/bootc`
of the main repo.

## What's here

| Kind | Entries |
|---|---|
| `image:` | `bazzite` (ublue Bazzite NVIDIA AI/dev workstation — the canonical worked example), `aurora` (ublue Aurora DX + ov toolchain) |
| `vm:` | the matching `kind: vm` bootc entities (`bazzite-bootc`, `aurora-bootc`) |

All images ship `enabled: false` — build them with `--include-disabled`.

## Composition — vendored bootc-exclusive layers + shared refs

This repo **vendors its bootc-exclusive layers locally** under `candy/`
(resolved via the `discover:` block) and pulls everything else from
`github.com/overthinkos/overthink` by **github reference**:

- the bootc-exclusive layers (`bootc-base`, `bootc-config`, `copr-desktop`,
  `desktop-apps`, `os-config`, `os-system-files`, `ujust`, `vr-streaming`) are
  vendored here and carry no tag — bare layer names resolve locally;
- every shared layer in `box.yml` is an
  `@github.com/overthinkos/overthink/candy/<name>:<tag>` ref;
- the shared build-config (`build.yml` — distro/builder/init, including the
  `fedora` distro definition + the `rpm` format template) is a flat `import:`;
- the main repo is mounted under the `ov` namespace via `import:`, bringing
  `ov.fedora-builder` into scope — bootc images root on external bases so they
  inherit no builder map and fall through to `defaults.builder → ov.fedora-builder`.

### Tag pins, each with a reason

| Pinned | Tag | Why |
|---|---|---|
| Every shared `@github` layer; `build.yml`; the `ov` namespace import | `v2026.141.1600` (layers, `build.yml`) / `v2026.143.844` (`ov` namespace) | the ecosystem layer tag the shared refs pin to |
| Vendored bootc-exclusive layers | _(none)_ | resolved locally via `discover:`, so no git pin |

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
ov box build bazzite --include-disabled

# From the parent overthink repo:
ov -C image/bootc image build bazzite --include-disabled

# Standalone, against the published repo:
ov --repo overthinkos/bootc image build bazzite --include-disabled
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
