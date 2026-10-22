# Deployment preparation

Work in ~/InvaSkog-deploy, the clean copy. Keep the research workspace intact.

## 1. Record dependencies on the tested machine

From a terminal in the deployment folder:

```bash
Rscript --vanilla -e 'if (!requireNamespace("renv", quietly = TRUE)) install.packages("renv", repos = "https://cloud.r-project.org"); renv::init()'
```

Review any prompts. renv discovers dependencies, creates a project library and records package versions. Do not invent a lockfile on another machine.

After initialization, start a new session that loads the project .Rprofile:

```bash
Rscript -e 'renv::status(); shiny::runApp(".", launch.browser = TRUE)'
```

Do not use --vanilla for this second command: it skips project activation.

Check species switching, all model layers and background variants, GBIF refresh, opacity, location lookup, panel scrolling, photo enlargement, and help/about dialogs. Stop the app with Ctrl+C.

If package changes were needed, record them with:

```bash
Rscript -e 'renv::snapshot()'
```

Commit renv.lock, .Rprofile, renv/activate.R and renv/settings.json, plus renv/.gitignore if generated. Do not commit the installed package library or cache.

## 2. Initialize local Git history

After the clean app passes:

```bash
git init -b main
git add app.R data README.md CREDITS.md DEPLOYMENT.md .gitignore .Rprofile renv.lock renv
git status --short
git diff --cached --stat
```

Review the staged list. It should contain the app, its deployment data, documentation and dependency configuration, not raw climate archives or installed package directories.

Then:

```bash
git commit -m "Prepare InvaSkog beta deployment"
```

If Git requests an author identity, configure your intended author name and GitHub-associated email locally for this repository, then retry.

## 3. Remaining release tasks

- Create and connect the GitHub repository; no remote is created by these commands.
- Select the code licence, with explicit treatment of third-party assets.
- Document the source/version of the Sweden boundary.
- Complete the outstanding training climate-input provenance check.
- Configure Posit hosting, verify its current runtime and system dependencies, and test the deployed app.
- Add the working deployment URL to README.md after deployment.

Official renv documentation: https://rstudio.github.io/renv/reference/init.html
