# Rochses

The "coming soon" landing page for Rochses — an independent fashion house.

Built with [Astro](https://astro.build) and [Tailwind CSS](https://tailwindcss.com).

## Project structure

```
/
├── public/
│   ├── CNAME              # custom domain for GitHub Pages (rochses.com)
│   ├── favicon.ico
│   └── favicon.svg
├── src/
│   ├── layouts/
│   │   └── Layout.astro  # shared <head>, fonts, global styles
│   ├── pages/
│   │   └── index.astro   # the coming-soon page
│   └── styles/
│       └── global.css    # Tailwind + theme tokens (colors, fonts)
├── .github/workflows/
│   └── deploy.yml         # GitHub Actions deployment to GitHub Pages
└── astro.config.mjs
```

## Local development

```sh
npm install
npm run dev
```

This starts a dev server, usually at `http://localhost:4321`. Astro will hot-reload as you edit files in `src/`.

Other useful commands:

| Command             | What it does                                  |
| -------------------- | ---------------------------------------------- |
| `npm run build`     | Builds the production site into `dist/`        |
| `npm run preview`   | Serves the built `dist/` folder locally         |

---

## Deploying the site

This section walks through everything needed to get this project from "a folder on your laptop" to "live at rochses.com", deployed automatically every time you push to `main`. It assumes no prior git/GitHub experience.

### 1. Initialize git in this project

From the project root (this folder), run:

```sh
git init
git add .
git commit -m "Initial commit: Rochses coming-soon page"
```

This turns the folder into a git repository and creates your first commit (a saved snapshot of the code).

### 2. Create a new, empty repository on GitHub

1. Go to [github.com/new](https://github.com/new).
2. Name the repository (e.g. `rochses` or `rochses-website`).
3. Leave it **empty** — do **not** check "Add a README", "Add .gitignore", or "Choose a license". This project already has those files, and an empty repo avoids merge conflicts when you push.
4. Choose Public or Private (either works — GitHub Pages supports both on paid/education plans; public repos get Pages for free).
5. Click **Create repository**.

GitHub will show you a page with setup instructions and a repository URL that looks like:

```
https://github.com/<your-username>/<your-repo-name>.git
```

### 3. Link your local project to the GitHub repository

Back in your terminal, run (replacing the URL with your own):

```sh
git remote add origin https://github.com/<your-username>/<your-repo-name>.git
git branch -M main
git push -u origin main
```

- `git remote add origin ...` tells your local repo where its "remote" (GitHub) home is.
- `git branch -M main` makes sure your default branch is named `main` (matches the GitHub Actions workflow in this project).
- `git push -u origin main` uploads your commit(s) to GitHub and links your local `main` branch to GitHub's `main` branch, so future pushes just need `git push`.

Refresh the GitHub repository page — your files should now appear there.

### 4. Enable GitHub Pages with GitHub Actions

This repository already includes a deployment workflow at `.github/workflows/deploy.yml`, which builds the Astro site and publishes it whenever you push to `main`. You just need to tell GitHub to use it:

1. On GitHub, open your repository.
2. Go to **Settings** (top tab bar of the repo) → **Pages** (left sidebar, under "Code and automation").
3. Under **Build and deployment** → **Source**, select **GitHub Actions** (not "Deploy from a branch").
4. That's it — no further configuration needed here. GitHub will now run the workflow automatically on every push to `main`.

To trigger the first deployment, either push a new commit, or:

1. Go to the **Actions** tab of your repository.
2. Select the **Deploy to GitHub Pages** workflow on the left.
3. Click **Run workflow** → **Run workflow** (this uses the `workflow_dispatch` trigger built into the workflow file).

Watch the run in the **Actions** tab — once both the `build` and `deploy` jobs show green checkmarks, your site is live.

### 5. Connect the custom domain (rochses.com)

The `public/CNAME` file in this project already contains `rochses.com`, which tells GitHub Pages which custom domain to serve the site on once it's built. To finish connecting the domain:

1. At your domain registrar (wherever `rochses.com` was purchased), add the following DNS records:
   - **A records** for the apex domain (`rochses.com`) pointing to GitHub's Pages IP addresses:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - Optionally, a **CNAME record** for `www.rochses.com` pointing to `<your-username>.github.io`, if you want `www.rochses.com` to work too.
2. Back in **Settings → Pages** on GitHub, under **Custom domain**, confirm it shows `rochses.com` (it should pick this up automatically from the `CNAME` file after your first deploy). Once GitHub verifies the DNS, check **Enforce HTTPS**.

DNS changes can take anywhere from a few minutes to a few hours to propagate.

### Everyday workflow after setup

Once the above is done, shipping changes is simple:

```sh
git add .
git commit -m "Describe what changed"
git push
```

Every push to `main` re-triggers the GitHub Actions workflow, which rebuilds and redeploys the site automatically. No manual build/upload steps required.
