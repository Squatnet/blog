# blog

Hugo site for Matt's blog, using the PaperMod theme.

- Source of truth: Forgejo `squatnet/blog`
- Mirror: GitHub `Squatnet/blog`
- Public URL: https://squatnet.github.io/blog/
- Publishing: GitHub Pages via `.github/workflows/pages.yml`

## Writing

Posts live in `content/posts/`. Create a new one with:

```sh
hugo new content posts/my-post.md
```

It is created as a draft. Set `draft: false` when it is ready to publish.

Project pages live in `content/projects/` and are edited in place rather than
being re-dated.

## Local preview

```sh
hugo server -D
```

## Build

```sh
hugo --minify --gc
```

Output goes to `public/`, which is not committed.

## Publishing

Push to `main` on the mirrored GitHub repo. The workflow builds with Hugo
extended and deploys to GitHub Pages. In the GitHub repo settings, Pages must be
set to deploy from GitHub Actions.
