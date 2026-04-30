# Learning Technology Tools Documentation

![docs](https://github.com/Unity-Environmental-University/lt-tools-documentation/actions/workflows/deploy.yml/badge.svg)

This repo is the central hub that publishes the [LXD Tools Portal](https://unity-environmental-university.github.io/lt-tools-documentation/). It uses [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and the [mkdocs-multirepo-plugin](https://github.com/jdoiro3/mkdocs-multirepo-plugin) to pull documentation from individual tool repositories and combine them into a single site.

## How it works

The site is defined in [mkdocs.yml](mkdocs.yml). Each tool's docs live in its own GitHub repository under a `docs/` folder. The `multirepo` plugin fetches those docs at build time using `!import` entries in the `nav` section. GitHub Actions builds and deploys the site to GitHub Pages on every push to `main`.

## Adding a new tool

1. **Prepare the tool's repository** — Add a `docs/` folder to the tool's repo. MkDocs expects at least one `.md` file there. A `docs/index.md` is a good starting point.

2. **Register the tool here** — Open [mkdocs.yml](mkdocs.yml) and add an entry to the `nav` section:

   ```yaml
   nav:
     - Home: index.md
     - My New Tool: "!import https://github.com/your-org/your-repo?branch=main"
   ```

   Replace the URL and branch with the tool repo's actual values.

3. **Update the portal homepage** — Edit [docs/index.md](docs/index.md) to add a row for the new tool in the "What's here" table.

4. **Push to `main`** — The GitHub Actions workflow triggers automatically and redeploys the site.

## Writing docs for a tool

Documentation lives inside each tool's own repository, not this one. To update or add content for a tool:

- Navigate to that tool's repo and edit files under its `docs/` folder.
- Use standard Markdown. MkDocs Material supports [extended syntax](https://squidfunk.github.io/mkdocs-material/reference/).
- The portal rebuilds automatically when this repo is pushed. To trigger a rebuild from a tool repo, send a `repository_dispatch` event of type `rebuild-docs` to this repo (see the workflow in [.github/workflows/deploy.yml](.github/workflows/deploy.yml)).

## Local development

```bash
pip install mkdocs-material mkdocs-multirepo-plugin
mkdocs serve
```

The site is available at `http://127.0.0.1:8000`. Changes to `docs/index.md` or `mkdocs.yml` reload automatically; changes in imported repos require a restart.

## Private repositories

If a tool repo is private, export a GitHub personal access token before building:

```bash
export GithubAccessToken=your_token_here
mkdocs serve
```

For the GitHub Actions workflow, store the token as a repository secret named `GH_TOKEN` and uncomment the corresponding line in [.github/workflows/deploy.yml](.github/workflows/deploy.yml).
