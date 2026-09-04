# Go With The Flow — package repository

This repo hosts the built FreeBSD/OPNsense package(s) and pkg repository
catalog for Go With The Flow, an OPNsense plugin for per-host connection
and bandwidth tracking with hostname resolution. The plugin's source is
currently private, so here's what it actually does:

## Features

- **Live per-host traffic view** — which remote hosts each device on your
  network is talking to right now, with live bandwidth and a running
  "Top Talkers" ranking, built from pf's own connection-state table (no
  extra packet capture load for this part).
- **Hostname resolution without a DNS proxy** — passively watches DNS
  answers and TLS ClientHello SNI as traffic crosses the firewall to put
  a real hostname (`netflix.com`, not just an IP) on HTTPS/QUIC
  connections plain firewall logs can't label, with reverse-DNS (PTR) as
  a last-resort fallback. Nothing is redirected or intercepted.
- **History with configurable retention** — a raw connection log plus
  automatic hourly/daily rollups, so long-term bandwidth trends stay
  queryable long after the raw detail ages out.
- **DNS Queries page** — what each device is actually looking up, and
  what came back.
- **Automatic category classification** — destinations are tagged
  (social media, video streaming, ads, etc.) from a maintained public
  category list, with manual overrides for anything you want classified
  differently.
- **Optional deep packet inspection** — an opt-in nDPI-based pass for
  finer-grained protocol classification beyond what hostname/port alone
  can tell you.
- **Block a device, or just specific domains for one device** — full
  blocks ride your existing pf ruleset; domain-only blocks ride
  Unbound's own DNS blocklist feature, with automatic subdomain coverage
  and no separate blocking engine of its own.
- **Blocking on a weekly schedule** — recurring windows (e.g. "block the
  kids' devices 10pm–7am on school nights"), correctly spanning
  midnight, with instant manual override (block/unblock right now) that
  resumes the normal schedule automatically once the current window
  ends.
- **Devices identified by name, not just IP** — local hosts are labeled
  from DHCP reservations/observed hostnames rather than raw addresses,
  so a device stays recognizable even as its IP changes.

Everything above runs as a single lightweight Python daemon integrated
into OPNsense's own service and plugin framework — no separate database
server, no cloud dependency, nothing phoning home.

## Using this repo

On the OPNsense box, as root:

```
curl -o /usr/local/etc/pkg/repos/gowiththeflow.conf https://tobyandzuzka.com/gowiththeflow-pkg-repo/gowiththeflow.conf
```

Then either `pkg update && pkg install os-gowiththeflow`, or go to
Firmware > Plugins in the GUI, "Check for updates", and install from
there (plugin installs are gated on core being up to date regardless of
repo).

(The URL isn't a typo for `tobydoig.github.io` -- this GitHub Pages site
301-redirects there since that domain is already the custom domain on the
`tobydoig.github.io` user-pages site, which becomes canonical for all
project-page URLs under the account too.)

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
