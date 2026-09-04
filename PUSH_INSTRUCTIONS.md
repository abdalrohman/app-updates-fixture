# Push this folder as your PUBLIC test repo

The service itself stays private. Only this folder becomes public.

```bash
# 1. From the fixture-repo/ directory:
git init -b main
git add README.md CHANGELOG.md CHANGELOG-v1.0.0.md images dist PUSH_INSTRUCTIONS.md .github
git commit -m "feat: public update fixture v1.0.0 + v1.1.0"
gh repo create YOUR_GITHUB_ACCOUNT/YOUR_UPDATES_REPO --public --source=. --push

# 2. Fix manifest URLs to your account (one sed, then amend):
sed -i 's|OWNER/REPO|YOUR_GITHUB_ACCOUNT/YOUR_UPDATES_REPO|g' dist/images-manifest.json
git commit -am "fix: manifest URLs" && git push

# 3. Publish the two demo releases (order matters):
gh release create v1.0.0 --title "v1.0.0" --notes-file CHANGELOG-v1.0.0.md \
  dist/myapp-v1.0.0.exe dist/SHA256SUMS.txt dist/images-manifest.json \
  images/welcome-hero.png images/welcome-banner.png images/welcome-avatar.png

gh release create v1.1.0 --title "v1.1.0" --notes-file CHANGELOG.md \
  dist/myapp-v1.1.0.exe dist/SHA256SUMS.txt dist/images-manifest.json \
  images/welcome-hero.png images/welcome-banner.png images/welcome-avatar.png

# 4. Verify the service sees it (no token needed):
./build/welcome_demo --owner YOUR_GITHUB_ACCOUNT --repo YOUR_UPDATES_REPO \
  --current 1.0.0 --cache ./cache --force 1
# expect: UPDATE AVAILABLE: v1.1.0 + changelog + 5 assets, images cached
```

To demo "image changed": regenerate one PNG with new `--hero-text`, publish `v1.1.1`,
re-run — only that PNG downloads (`1 downloaded, 2 up-to-date`).
