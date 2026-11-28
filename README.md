# arch-packages

A small Arch pacman repository built by GitHub Actions. Each workflow run fetches
the latest AUR recipes for the packages below and publishes versions not already
in the `packages` release.

## Packages

| Package | Vendored snapshot | Upstream source | AUR maintainer in snapshot | AUR last updated in snapshot |
| --- | --- | --- | --- | --- |
| `ai-usagebar-bin` | 1.53.0-1 | [ai-usagebar releases](https://github.com/akitaonrails/ai-usagebar/releases) | akitaonrails | 2026-11-22 |
| `betterbird-bin` | 153.7.0esr-1 | [betterbird.eu downloads](https://www.betterbird.eu/downloads/) | Posi | 2026-11-11 |
| `figcraft-bin` | 0.18.1-1 | [figcraft releases](https://github.com/storytold/figcraft/releases) | elonux | 2026-11-12 |
| `frida-server-android-bin` | 17.27.4-1 | [frida releases](https://github.com/frida/frida/releases) | lapsus | 2026-11-21 |
| `helium-browser-bin` | 0.22.2.1-1 | [helium-linux releases](https://github.com/imputnet/helium-linux/releases) | playleft | 2026-11-21 |
| `localsend-bin` | 1.19.0-2 | [localsend releases](https://github.com/localsend/localsend/releases) | noureddinex | 2026-11-25 |
| `morphe-desktop` | 2.3.0-1 | [morphe-desktop releases](https://github.com/MorpheApp/morphe-desktop/releases) | lapsus | 2026-11-21 |
| `nuvio-desktop-bin` | 0.1.38alpha-1 | [NuvioDesktop releases](https://github.com/NuvioMedia/NuvioDesktop/releases) | glorious-yellow | 2026-11-19 |
| `rstudio-desktop-bin` | 2026.11.0.203-1 | [RStudio download](https://download1.rstudio.org/electron/jammy/amd64/) | cbrnr | 2026-11-13 |
| `telegram-desktop-bin` | 7.4.6-1 | [tdesktop releases](https://github.com/telegramdesktop/tdesktop/releases) | debendraoli | 2026-11-24 |
| `vectorcraft-bin` | 0.24.1-1 | [vectorcraft releases](https://github.com/storytold/vectorcraft/releases) | modinfo | 2026-11-12 |
| `zotero-bin` | 10.0.8-2 | [zotero.org download](https://www.zotero.org/download) | juanmah | 2026-11-14 |

The checked-in recipes are AUR snapshots; the table versions and maintainer
credits describe those snapshots. CI fetches each current AUR recipe at run time,
keeps local-only assets, and applies `aur-update.patch` when present. The RStudio
overlay drops its bundled Copilot language server and unused development files.
`zotero-bin` also credits contributor Matthias Kurz, and `helium-browser-bin`
credits contributors Pujan Modha and init_harsh.

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
sudo pacman -S arch-packages/<pkgname>   # any name from the table, e.g. zotero-bin
```

If the package is already installed from the AUR, remove it first with
`sudo pacman -Rns <pkgname>`. Your user data in `~` is not touched, and the
install that follows comes from this repo.

## Package updates

Each run fetches the current AUR recipe and publishes any version not already
in the release. It does not commit refreshed AUR snapshots to this repository.
CI uses AUR's `PKGBUILD` and `.SRCINFO`, overlays local-only assets from
`aurpkgbuilds/<pkgname>/`, and applies an optional `aur-update.patch`. Put recipe
customizations in that patch; edits to the vendored `PKGBUILD` alone are not used
by CI. This executes upstream AUR build scripts automatically; the build job is
read-only, and a separate write-enabled job publishes its package artifacts.
