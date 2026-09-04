# Push this folder as your PUBLIC test repo

The service itself stays private. Only this folder becomes public.

> `v1.0.0` + `v1.1.0` are already published. Below is the repeatable flow
> for every future version. `CHANGELOG.md` always holds the *next* release's
> notes only — old notes live on their GitHub release pages, no history files needed.

```bash
# 1. From the fixture-repo/ directory (first time only):
git init -b main
git add README.md CHANGELOG.md images dist PUSH_INSTRUCTIONS.md .github
git commit -m "feat: public update fixture"
gh repo create YOUR_GITHUB_ACCOUNT/YOUR_UPDATES_REPO --public --source=. --push

# 2. Fix manifest URLs to your account (one sed, then amend):
sed -i 's|OWNER/REPO|YOUR_GITHUB_ACCOUNT/YOUR_UPDATES_REPO|g' dist/images-manifest.json
git commit -am "fix: manifest URLs" && git push

# 3. Publish each release (example for v1.2.0):
gh release create v1.2.0 --title "v1.2.0" --notes-file CHANGELOG.md \
  dist/myapp-v1.2.0.exe dist/SHA256SUMS.txt dist/images-manifest.json \
  images/welcome-hero.png images/welcome-banner.png images/welcome-avatar.png

# 4. Verify the service sees it (no token needed):
./build/welcome_demo --owner YOUR_GITHUB_ACCOUNT --repo YOUR_UPDATES_REPO \
  --current 1.0.0 --cache ./cache --force 1
# expect: UPDATE AVAILABLE: v1.1.0 + changelog + 5 assets, images cached
```

To demo "image changed": regenerate one PNG with new `--hero-text`, publish `v1.1.1`,
re-run — only that PNG downloads (`1 downloaded, 2 up-to-date`).
