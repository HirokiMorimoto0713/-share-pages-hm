# Repository operating rules

## Purpose

This repository publishes small, shareable, mobile-friendly HTML pages through Cloudflare Workers. Keep it intentionally simple: static HTML, CSS, and JavaScript only unless the user explicitly requests a framework or build system.

## Deployment

- Production Worker: `share-pages-hm`
- Production URL: `https://share-pages-hm.tororousagi.workers.dev/`
- Cloudflare deploy command: `npx wrangler deploy --assets ./public/`
- Wrangler configuration: root `wrangler.jsonc`
- Merges to `main` trigger the production deployment automatically.
- Do not change the Worker name, deployment command, assets directory, or Cloudflare configuration unless required by the task and explicitly explained in the PR.

## Page layout

- The current root page is `public/index.html`.
- Add future standalone pages at `public/<slug>/index.html`.
- Use short, descriptive, lowercase ASCII kebab-case slugs.
- Do not overwrite, rename, or remove an existing page unless the user explicitly requests it.
- Keep page-specific CSS and JavaScript self-contained when practical.
- Do not add npm, a bundler, a framework, or unnecessary dependencies for a static page.

Example:

```text
public/
├── index.html
├── maine-coon/
│   └── index.html
└── quantum/
    └── index.html
```

## Content and design

- Preserve the meaning, tone, and factual nuance of the source material.
- Preserve useful citations and outbound source links when supplied.
- Use UTF-8 Japanese text and include a mobile viewport declaration.
- Design mobile-first, while keeping desktop layouts readable.
- Prefer semantic HTML, accessible contrast, readable font sizes, and clear tap targets.
- Avoid external assets when an inline or local solution is reasonable.
- Never commit secrets, credentials, private identifiers, or analytics keys.

## Validation

Before opening a PR:

1. Confirm every changed HTML file parses and is served with HTTP 200 from a local static server when possible.
2. Check for missing local assets and broken relative links.
3. Check JavaScript syntax when the page includes scripts.
4. Verify the page is readable at a mobile viewport and has no obvious horizontal overflow.
5. Confirm `wrangler.jsonc` still names `share-pages-hm` and contains a valid `compatibility_date`.
6. Confirm the existing deploy command still works with `./public/`.

## Git workflow

1. Start from the latest `main`.
2. Create a focused branch named `agent/<short-description>`.
3. Keep the diff limited to the requested page or operational change.
4. Commit with a concise message.
5. Open a PR targeting `main`.
6. In the PR body, explain what changed, the resulting URL path, and what was validated.
7. Do not merge until the user explicitly approves the merge.
8. After approval, squash-merge unless the user requests another method.
9. After the production deployment finishes, verify the public URL and report the exact link.

## Default behavior for a new page request

When the user asks to turn content into a shareable page and does not specify a path:

- infer a concise kebab-case slug from the topic;
- add the page under `public/<slug>/index.html`;
- leave existing pages unchanged;
- include the expected public URL in the PR summary;
- ask for merge approval after the PR is ready.
