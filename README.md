# MyApp Updates Fixture (public demo feed)

Public, dependency-free update feed for testing the private app's updater.
The C++ service reads **only** this repo's Releases page — no server.

## What's here

* `images/` — 3 Pillow-generated welcome PNGs (`welcome-hero/banner/avatar.png`)
* `dist/myapp-v*.exe` — placeholder **TEXT** files with `.exe` extension (opaque bytes + SHA-256 exercise the real path)
* `dist/SHA256SUMS.txt`, `dist/images-manifest.json` — published as release assets alongside the images
* `CHANGELOG.md` — becomes the GitHub release `body` = welcome-screen changelog

## Publish (2 commands per release)

See `PUSH_INSTRUCTIONS.md` + `../docs/MANUAL_RELEASE.md` in the service repo.
