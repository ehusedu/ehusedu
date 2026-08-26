# ehusedu

Personal blog, built with [Astro](https://astro.build/) and the
[AstroPaper](https://github.com/satnaing/astro-paper) theme, deployed to
GitHub Pages.

## Development

```bash
pnpm install
pnpm dev       # http://localhost:4321
pnpm build     # production build to dist/
pnpm preview   # preview the production build
pnpm format    # format with Prettier
pnpm lint      # lint with ESLint
```

## Content

- Blog posts live in `src/content/posts/`.
- Standalone pages (e.g. About) live in `src/content/pages/`.
- Site metadata (title, description, socials, etc.) is configured in
  `astro-paper.config.ts`.

## Deployment

Pushes to `main` are built and deployed to GitHub Pages automatically via
`.github/workflows/deploy.yml`. Enable Pages for this repo under
**Settings → Pages → Source: GitHub Actions**.

Since this repo is `ehusedu/ehusedu` rather than `ehusedu/ehusedu.github.io`,
it deploys as a project page at `https://ehusedu.github.io/ehusedu/`
(configured via `base: "/ehusedu"` in `astro.config.ts`). Rename the repo to
`ehusedu.github.io` and drop the `base` option if you'd rather serve from the
root domain.

## License

Theme code is based on AstroPaper, licensed under the [MIT License](LICENSE).
