# AxisNet.work website

Static site. No framework and no runtime dependencies. Everything that gets deployed is in `public/`.

```
public/
  index.html            home: network globe, partners, projects, Suloon, members, contact
  projects.html         in-site project browser (README, languages, files)
  terms.html            Terms of Service
  acceptable-use.html   Acceptable Use Policy
  privacy.html          Privacy & Data Security
  404.html
  assets/css/site.css
  assets/js/            common.js, home.js, globe.js, suloon.js, projects.js
  assets/img/           logo sizes + Suloon tree
  data/land.json        land dot grid for the globe (generated once from Natural Earth 50m)
  data/projects.json    GitHub snapshot, written by scripts/sync-projects.mjs
scripts/sync-projects.mjs
vercel.json
```

## Run locally

```
npm run dev        # serves public/ on http://localhost:3000
```

## Deploying on Vercel

Import the repo and leave the framework preset as **Other**. `vercel.json` already sets:

- build command: `node scripts/sync-projects.mjs`, which refreshes the project data from GitHub
- output directory: `public`
- clean URLs (`/projects`, `/terms` and so on)

Add a `GITHUB_TOKEN` environment variable in Vercel (a fine-grained token with public read access is enough). Without it, the build uses GitHub's anonymous limit of 60 requests an hour, which Vercel's shared build machines can run out of. If the sync fails, the build keeps the committed `data/projects.json` and still deploys.

Projects show up on their own: any public, non-fork repo owned by a member appears on the next deploy. To hide one, add `"owner/name"` to `EXCLUDE` in `scripts/sync-projects.mjs`. To add a member, add their GitHub username to `MEMBERS` in the same file, and add them to the Members section in `index.html`.

## Things to update later

- **Discord**: once the server is ready, replace the "Opening soon" button in the `#contact` section of `index.html` with `<a class="btn" href="https://discord.gg/...">Join the Discord</a>`. Also update the first step under "How working with us goes".
- **Legal pages**: these are plain-language drafts. Read them and change anything that doesn't match how you actually run things, especially logging and data retention in `privacy.html`. Update the "Last updated" date whenever you edit one.
- **Server names**: the `SYD-01` to `SYD-03` labels are placeholders. Change them in `index.html` if your servers have real names.
