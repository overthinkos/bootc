# image/bootc — signpost (not the rule-set)

This submodule is the **bootc** (bootable container / OS image) family: a
single `overthink.yml` that imports the main repo under the `ov` namespace and
`build.yml` flat.

**Load these skills FIRST (R0):**

- `/ov-distros:bootc-base`, `/ov-distros:bootc-config` — base composition for
  bootable images.
- `/ov-distros:bazzite`, `/ov-distros:aurora` — the ublue-derived bootc images
  (`bazzite` is the canonical bootc-VM worked example).
- `/ov-vm:vm` — booting a bootc image under libvirt/QEMU.

**Authoritative rules live in the `overthink` superproject's root `CLAUDE.md`**
(R0–R10, hard-cutover, AI attribution, git-workflow). This file only signposts
and restates no rule. The multi-agent workflow is in `/ov-internals:agents`.
History lives in `CHANGELOG.md`.
