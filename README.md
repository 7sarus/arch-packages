# arch-packages

Custom Arch pacman repo publishing `zotero-bin` and `helium-browser-bin` from
vendored AUR PKGBUILDs via GitHub Actions.

## Client setup

Add to `/etc/pacman.conf`:

```ini
[arch-packages]
SigLevel = Never
Server = https://github.com/7sarus/arch-packages/releases/latest/download/
```

Then:

```bash
sudo pacman -Syu
sudo pacman -S arch-packages/zotero-bin arch-packages/helium-browser-bin
```

## Update a package

Replace the files in `aurpkgbuilds/<pkgname>/` with the current AUR snapshot,
commit and push. The workflow rebuilds and overwrites the `packages` release
assets.

```bash
curl -fsSL "https://aur.archlinux.org/cgit/aur.git/snapshot/zotero-bin.tar.gz" | \
  tar xz --strip-components=1 -C aurpkgbuilds/zotero-bin
```
