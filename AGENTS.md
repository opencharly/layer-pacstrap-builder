# AGENTS.md — layer-pacstrap-builder

Standalone candy repo for the `pacstrap-builder` layer — the toolchain that
bootstraps a bootable Arch/CachyOS rootfs via pacstrap, partitions a VM disk, and
installs grub. It is the layer composition for the pacstrap-builder images that
run as privileged containers under `charly box build` / `charly vm build` for
`kind: bootstrap` source kinds. The candy lives in `charly.yml` at the repo root:
the `distro:` package arm and the `plan:` `check:` assertions.

This repo has **no `skill:` entity** in `charly.yml`, so there is no dedicated
owning skill projected into the marketplace corpus. The gap is recorded against
`opencharly/opencharly#291` (the batch that authors missing `skill:` entities).

Canonical files:

- `charly.yml` — the `pacstrap-builder:` candy entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:cachyos-pacstrap-builder` — the closest owning procedure: the
  privileged builder image that composes this layer, and the pacstrap rootfs
  bootstrap flow. Load before editing or troubleshooting.
- `/charly-vm:vm` — the `kind: vm` / `kind: bootstrap` source kinds this builder
  serves.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, and service declarations). Load before editing any entity field or
  plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps assert every bootstrap tool (pacstrap,
  genfstab/arch-chroot, parted, qemu-img, mkfs tools, grub-install, efibootmgr,
  mkinitcpio) is present.

## Modify this repo

- There is no `skill:` entity to keep in sync; if one is added (per #291), it
  must be edited together with the candy entity in the same change.
- Keep the full tool set complete: a missing tool breaks the rootfs bootstrap
  downstream, not the build.
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo — read it
  before landing.
- Release history lives in `CHANGELOG/`.
