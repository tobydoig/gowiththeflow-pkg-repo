# Go With The Flow — package repository

This repo hosts the built FreeBSD/OPNsense package(s) and pkg repository
catalog for [Go With The Flow](https://github.com/tobydoig/opnsense-gowiththeflow),
served over GitHub Pages so it can be added as a custom OPNsense firmware
repository (`Firmware > Plugins`).

## Using this repo

Add a custom pkg repo config on the OPNsense box (e.g.
`/usr/local/etc/pkg/repos/gowiththeflow.conf`):

```
gowiththeflow: {
  url: "https://tobyandzuzka.com/gowiththeflow-pkg-repo",
  enabled: true,
  signature_type: "none"
}
```

Not a typo: this GitHub Pages site 301-redirects `tobydoig.github.io/...` to
`tobyandzuzka.com/...` since that domain is already the custom domain on the
`tobydoig.github.io` user-pages site, which becomes canonical for all
project-page URLs under the account too. Use the direct URL to skip the
redirect hop.

Then `pkg update && pkg install os-gowiththeflow`, or find it under
Firmware > Plugins once core is up to date (plugin installs are gated on
that regardless of repo).

`signature_type: "none"` is a stopgap for a single-maintainer repo during
early testing — revisit before relying on this for anything that matters.

## Layout

A flat catalog at the repo root (single plugin, single ABI, no need for
the `<abi>/<version>/` nesting the official OPNsense repo uses):

```
os-gowiththeflow-<version>.pkg   the built package(s)
meta / meta.conf                 pkg repo metadata (pkg repo(8) output)
packagesite.pkg / data.pkg       the catalog itself
```

Regenerated with `pkg repo .` (see `net/gowiththeflow/pkg/build-pkg.sh`
in the source repo) after each new build.

Source and issue tracking live in the private
[opnsense-gowiththeflow](https://github.com/tobydoig/opnsense-gowiththeflow) repo.
