# image/bootc — signpost (not the rule-set)

This submodule is the **bootc** (bootable container / OS image) family: a
single `charly.yml` that imports the main repo under the `charly` namespace and
`build.yml` flat.

**Load these skills FIRST (R0):**

- `/charly-distros:bootc-base`, `/charly-distros:bootc-config` — base composition for
  bootable images.
- `/charly-distros:bazzite`, `/charly-distros:aurora` — the ublue-derived bootc images
  (`bazzite` is the canonical bootc-VM worked example).
- `/charly-vm:vm` — booting a bootc image under libvirt/QEMU.

**Authoritative rules live in the `opencharly` superproject's root `CLAUDE.md`**
(R0–R10, hard-cutover, AI attribution, git-workflow). This file only signposts
and restates no rule. The multi-agent workflow is in `/charly-internals:agents`.
History lives in `CHANGELOG.md`.
