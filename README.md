# Go With The Flow — package repository

This repo hosts the built FreeBSD/OPNsense package(s) and pkg repository
catalog for [Go With The Flow](https://github.com/tobydoig/opnsense-gowiththeflow),
served over GitHub Pages so it can be added as a custom OPNsense firmware
repository (`Firmware > Plugins`).

Nothing is published here yet — the FreeBSD port and package build are still
in progress (Phase C of the source repo's `DESIGN.md`). Once a package is
built, this repo will contain the standard pkg repo layout:

```
All/                  built .pkg files
<abi>/latest/         packagesite.pkg + meta.conf for the pkg client
```

Source and issue tracking live in the private
[opnsense-gowiththeflow](https://github.com/tobydoig/opnsense-gowiththeflow) repo.
