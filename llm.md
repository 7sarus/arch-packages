# arch-packages

Custom pacman repository built in GitHub Actions for the two high-churn AUR
packages kept on this host: `zotero-bin` and `helium-browser-bin`. Replaces
`vscodium-bin` as the managed-app approach. Host: Arch laptop, Wayland/niri.

## Layout

- `aurpkgbuilds/<pkgname>/` — vendored AUR snapshot (PKGBUILD, .SRCINFO, patches,
  desktop files). This is the build source of truth, not `aur.archlinux.org`.
- `.github/workflows/build.yml` — builds both packages in an Arch container,
  runs `repo-add`, publishes assets to the `packages` GitHub Release.
- `.ai/state.json` — live task state (keep under 250 tokens).

## Build / publish

Workflow runs on `main` push, `workflow_dispatch`, and weekly cron. Local check:

```bash
cd aurpkgbuilds/zotero-bin && makepkg -s --noconfirm --nocheck --skipverify
```

`--skipverify` skips PGP but keeps sha256 checks; needed because Helium's
PKGBUILD verifies an external `.asc` that is unreliable in CI.

Manual trigger: `gh workflow run build.yml -R 7sarus/arch-packages`.

## Client setup

Once the first release exists, add to `/etc/pacman.conf` (requires root):

```ini
[arch-packages]
SigLevel = Never
Server = https://github.com/7sarus/arch-packages/releases/latest/download/
```

Then `sudo pacman -Syu`. Repo database name is `arch-packages.db`, so the
`Server` and section name must stay in sync with `repo-add` in the workflow.

## Updating packages

Copy fresh files from AUR into `aurpkgbuilds/<pkgname>/`, commit, push. The
workflow rebuilds and replaces the release assets. Do not edit checksums by hand.

## Conventions

- Repo files are user-owned; no root needed. Only `pacman.conf` and package
  removal need sudo (password required on this host; agent cannot do it).
- Keep the workflow self-contained; it does not read any local machine state.
- Release tag is always `packages`, with `overwrite_files: true`.
