# pacstrap-builder

The pacstrap toolchain as a charly layer — the tools needed to bootstrap a
bootable Arch/CachyOS rootfs from scratch, partition a VM disk, and install grub.

The `pacstrap-builder` candy installs `arch-install-scripts` (pacstrap,
genfstab, arch-chroot), `qemu-img`, the filesystem tools (`dosfstools`,
`e2fsprogs`, `xfsprogs`, `btrfs-progs`), `parted`, `util-linux`, `grub`,
`efibootmgr` and `mkinitcpio`. It is the layer composition for the
`arch-pacstrap-builder` / `cachyos-pacstrap-builder` images, which run as
privileged containers under `charly box build` and `charly vm build` for
`kind: bootstrap` source kinds.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `pacstrap-builder` |
| Tools | `pacstrap`, `genfstab`, `arch-chroot`, `qemu-img`, `parted`, `grub-install`, `mkinitcpio`, `efibootmgr` |
| Filesystems | `e2fsprogs` (ext4), `dosfstools` (FAT), `xfsprogs`, `btrfs-progs` |
| Service / port | none (a privileged builder image) |

## How to use it

Compose the layer in a builder box's `candy:` list. The canonical consumers are
the pacstrap-builder images:

```yaml
arch-pacstrap-builder:
  candy:
    base: arch
    candy:
      - '@github.com/opencharly/layer-pacstrap-builder:v2026.239.1633'
```

It is used as the builder for the `arch-pacstrap`, `cachyos-pacstrap` and
`omarchy-pacstrap` VM `kind: bootstrap` source kinds.

## Layout

- `charly.yml` — the `pacstrap-builder:` candy entity (the `distro:` package arm
  and the `plan:` `check:` assertions).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- `/charly-distros:cachyos-pacstrap-builder` — a canonical consumer image.
- `/charly-vm:cachyos-bootstrap-vm` — the VM that uses this builder.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/opencharly](https://github.com/opencharly/opencharly) — the umbrella.
