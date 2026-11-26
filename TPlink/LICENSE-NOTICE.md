# License notice

`tplink-2026.11.3-f4dff39.zip` bundles third-party code. Each part keeps its own license.

| Component | Source | License |
|---|---|---|
| python-kasa (the `kasa` library, vendored under `_vendor/`) | [python-kasa PR #1592](https://github.com/python-kasa/python-kasa/pull/1592), commit `f4dff399016a9daa47316ffb209f6b74a989b6dc` | GPL-3.0-or-later |
| Home Assistant `tplink` integration (copied from Core 2026.11.3, with a small patch to `__init__.py` to load the vendored library) | [home-assistant/core](https://github.com/home-assistant/core) | Apache-2.0 |
| `ecdsa` (dependency of the PR) | [tlsfuzzer/python-ecdsa](https://github.com/tlsfuzzer/python-ecdsa) | MIT |
| `six` (dependency of `ecdsa`) | [benjaminp/six](https://github.com/benjaminp/six) | MIT |

The vendored python-kasa code is unchanged from the commit above. The corresponding source is available at that commit
in the pull request's branch. The only change to the Home Assistant integration is the patch that adds the vendored
folder to the import path and logs that the override is active.

This package is provided as is, with no warranty. It is not affiliated with or endorsed by TP-Link, Home Assistant
or the python-kasa project.
