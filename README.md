# HariManga Extension Repo for Tachimanga (iOS)

A Tachimanga-compatible extension repository for **[harimanga.me](https://harimanga.me)**, structured like keiyoushi.

> ✅ **This repo works with [Tachimanga on the iOS App Store](https://apps.apple.com/us/app/tachimanga/id6447486175)**
> Tachimanga fully supports Mihon/Tachiyomi extension repos — this is that format.

---

## Add to Tachimanga (iOS)

In Tachimanga, go to:

> **More → Extensions → Extension Repositories → (＋ icon)**

Paste your GitHub Pages URL:

```
https://<YOUR_GITHUB_USERNAME>.github.io/<THIS_REPO_NAME>/index.min.json
```

Then go to **Browse → Extensions**, find **HariManga**, and tap **Install**.

---

## One-time GitHub Setup

### 1. Create & push the repo

```bash
cd harimanga-repo
git init && git add . && git commit -m "feat: initial HariManga extension"
gh repo create harimanga-repo --public --push
```

### 2. Create a signing keystore (once)

```bash
keytool -genkeypair -v \
  -keystore harimanga.keystore -alias harimanga \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -storepass YOUR_STORE_PASS -keypass YOUR_KEY_PASS \
  -dname "CN=HariManga, O=HariManga, C=US"

# Base64-encode it for the GitHub secret
base64 harimanga.keystore | tr -d '\n'
```

### 3. Add GitHub Secrets

Settings → Secrets and variables → Actions → New repository secret

| Secret name        | Value                          |
|--------------------|--------------------------------|
| `SIGNING_KEY`      | base64 output of the keystore  |
| `ALIAS`            | `harimanga`                    |
| `KEY_STORE_PASSWORD` | your `--storepass` value     |
| `KEY_PASSWORD`     | your `--keypass` value         |

### 4. Enable Pages & Actions permissions

- **Settings → Pages** → Source: `gh-pages` branch, `/ (root)`
- **Settings → Actions → General** → Workflow permissions: **Read and write**

### 5. Trigger the first build

```bash
git commit --allow-empty -m "ci: trigger build" && git push
```

Build takes ~5 min. Once done, paste the Pages URL into Tachimanga.

---

## Features

| Feature | Supported |
|---|---|
| Popular manga | ✅ |
| Latest updates | ✅ |
| Search by title | ✅ |
| Filter by genre | ✅ |
| Filter by status | ✅ |
| Filter by sort order | ✅ |
| Manga details | ✅ |
| Chapter list | ✅ |
| Page images | ✅ |

---

## Updating

Bump `extVersionCode` in `src/en/HariManga/build.gradle`, commit & push. The Action rebuilds automatically.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| URL not accepted by Tachimanga | Ensure it ends with `index.min.json` |
| Images not loading | Open a chapter in WebView once (Cloudflare cookie) |
| Build fails on signing step | Double-check all 4 secrets are set correctly |
