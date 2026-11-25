# arch-packages

A small Arch pacman repository built by GitHub Actions. It rebuilds the AUR
packages listed below and publishes them from the `packages` release, so this
host gets them without building on the AUR each time.

## Packages

| Package | Version | Upstream source | AUR maintainer | AUR last updated |
| --- | --- | --- | --- | --- |
| `ai-usagebar-bin` | 1.53.0-1 | [ai-usagebar releases](https://github.com/akitaonrails/ai-usagebar/releases) | akitaonrails | 2026-11-22 |
| `betterbird-bin` | 153.7.0esr-1 | [betterbird.eu downloads](https://www.betterbird.eu/downloads/) | Posi | 2026-11-11 |
| `figcraft-bin` | 0.18.1-1 | [figcraft releases](https://github.com/storytold/figcraft/releases) | elonux | 2026-11-12 |
| `frida-server-android-bin` | 17.27.4-1 | [frida releases](https://github.com/frida/frida/releases) | lapsus | 2026-11-21 |
| `helium-browser-bin` | 0.22.2.1-1 | [helium-linux releases](https://github.com/imputnet/helium-linux/releases) | playleft | 2026-11-21 |
| `morphe-desktop` | 2.3.0-1 | [morphe-desktop releases](https://github.com/MorpheApp/morphe-desktop/releases) | lapsus | 2026-11-21 |
| `nuvio-desktop-bin` | 0.1.38alpha-1 | [NuvioDesktop releases](https://github.com/NuvioMedia/NuvioDesktop/releases) | glorious-yellow | 2026-11-19 |
| `rstudio-desktop-bin` | 2026.11.0.203-1 | [RStudio download](https://download1.rstudio.org/electron/jammy/amd64/) | cbrnr | 2026-11-13 |
| `telegram-desktop-bin` | 7.4.6-1 | [tdesktop releases](https://github.com/telegramdesktop/tdesktop/releases) | debendraoli | 2026-11-24 |
| `vectorcraft-bin` | 0.24.1-1 | [vectorcraft releases](https://github.com/storytold/vectorcraft/releases) | modinfo | 2026-11-12 |
| `zotero-bin` | 10.0.8-2 | [zotero.org download](https://www.zotero.org/download) | juanmah | 2026-11-14 |

The PKGBUILDs start as copies of the AUR packages, kept in
`aurpkgbuilds/<pkgname>/`. Credit for them goes to the AUR maintainers above.
`zotero-bin` also credits contributor Matthias Kurz, and `helium-browser-bin`
credits contributors Pujan Modha and init_harsh. `rstudio-desktop-bin` drops the
bundled Copilot language server and the development files, which the IDE does
not need at runtime. The rest are unchanged repackagings of upstream binaries.

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
