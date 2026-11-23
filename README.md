# arch-packages

A small Arch pacman repository built by GitHub Actions. It rebuilds three AUR
packages, `zotero-bin`, `helium-browser-bin`, and `rstudio-desktop-bin`, and
publishes them from the `packages` release, so this host gets them without
building on the AUR each time.

## Packages

| Package | Version | Upstream source | AUR maintainer | AUR last updated |
| --- | --- | --- | --- | --- |
| `zotero-bin` | 10.0.8-2 | [zotero.org download](https://www.zotero.org/download) | juanmah | 2026-11-14 |
| `helium-browser-bin` | 0.22.2.1-1 | [helium-linux releases](https://github.com/imputnet/helium-linux/releases) | playleft | 2026-11-21 |
| `rstudio-desktop-bin` | 2026.11.0.203-1 | [RStudio download](https://download1.rstudio.org/electron/jammy/amd64/) | cbrnr | 2026-11-13 |

The PKGBUILDs start as copies of the AUR packages, kept in
`aurpkgbuilds/<pkgname>/`. Credit for them goes to the AUR maintainers above.
`zotero-bin` also credits contributor Matthias Kurz, and `helium-browser-bin`
credits contributors Pujan Modha and init_harsh. `rstudio-desktop-bin` drops the
bundled Copilot language server and the development files, which the IDE does
not need at runtime.

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

If the package is already installed from the AUR, remove it first with
`sudo pacman -Rns <pkgname>`. Your user data in `~` is not touched, and the
install that follows comes from this repo.

## Updating a package

Copy the current AUR snapshot over the vendored files, commit, and push. The
workflow rebuilds both packages and overwrites the release assets.

```bash
curl -fsSL "https://aur.archlinux.org/cgit/aur.git/snapshot/zotero-bin.tar.gz" | \
  tar xz --strip-components=1 -C aurpkgbuilds/zotero-bin
```

Running the same command for `helium-browser-bin` updates the other package.
Check the snapshot for a `.gitignore` containing `*`, which would hide the copied
files from git.
