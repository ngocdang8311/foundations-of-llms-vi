# Instructions for coding agents

This repo is the Hugging Face Space of the blog post "NLP before Transformers" (post 0 of the series based on David Louapre's MVA course "Foundations of Large Language Models"). It is a separate git repo cloned inside the course repo, at `blog/00-nlp-before-transformers/space/`. The parent folder holds the outline (`../outline.md`) and the code of the exercises and figures (`../code/`). The course repo has its own `CLAUDE.md` and `README.md`.

The Space is `dlouapre/nlp-before-transformers`. It is private until the post is published.

## How we work on this post

- David writes the prose. Do not write or rewrite sentences of the post, even as a draft, unless he asks for it. You may fix typos, grammar and syntax in his text, and you should point out passages that are unclear or wrong.
- Your part: the outline and the progression of ideas, the choice of papers, the exercises and their solutions, the interactive figures, the layout and the UI.
- When a section needs text that David has not written yet, leave a visible placeholder (for example `<Note>TODO (David): explain why counts fail</Note>`) instead of writing it.

## Template documentation

The post is built with the [research article template](https://huggingface.co/spaces/tfrere/research-article-template) (Astro 4 + MDX, D3 embeds, KaTeX).

- `HOW_TO.md`: all template features, components and syntax. It was extracted from the template's demo chapters, which were deleted.
- `.ai/skills/`: guides maintained with the template, linked in `.claude/skills/` so that Claude Code loads them as skills:
  - `create-html-embed`: read `SKILL.md` and `directives.md` before writing any D3 embed;
  - `article-frontmatter`: fields of `article.mdx`;
  - `deploy-to-hf-spaces`: deployment. Ignore its SSH and `huggingface-cli` instructions, see "Deploy" below.

## Key paths

| What | Where |
|------|-------|
| Main article (metadata + chapter imports) | `app/src/content/article.mdx` |
| Chapters | `app/src/content/chapters/` |
| Bibliography | `app/src/content/bibliography.bib` |
| HTML embeds (D3) | `app/src/content/embeds/` |
| Data files for embeds (CSV/JSON) | `app/src/content/assets/data/` |
| Images | `app/src/content/assets/image/` |
| Components | `app/src/components/` |
| Styles and CSS variables | `app/src/styles/` |

## Key rules

- Embeds are self-contained `.html` files: one `<div>`, a scoped `<style>`, and an IIFE `<script>`. Colors come from `window.ColorPalettes`, theming from CSS variables (`--text-color`, `--surface-bg`, `--border-color`). Never hardcode colors.
- Citations: `[@key]`, with the entry in `bibliography.bib`. Do not put BibTeX comments in that file: braces in comments break the MDX parser.
- Math: `$...$` inline, `$$...$$` for blocks (KaTeX).
- `banner.html` must exist in `embeds/`: it is hard-coded in `pages/dataviz.astro` and in the `paper` layout.
- The post is public once published. Before you reuse a picture from a course deck, check where it comes from: pictures from the web may not be reusable. Figures from the course `visuals/` folders and equations from `.eqn` files are David's own.
- Images, CSV and JSON files are stored with Git LFS (see `.gitattributes`). Hugging Face rejects binary files pushed outside LFS.

## Development

```bash
cd app
npm install
npm run dev        # dev server at http://localhost:4321
npm run build      # production build
```

## Deploy

`origin` is the Space, so `git push` deploys (the build takes a few minutes). Git has no credential helper for huggingface.co on David's machine. Pass the token from the `HF_TOKEN` environment variable for a single command, without saving it in `.git/config`:

```bash
GIT_TERMINAL_PROMPT=0 git -c credential.helper= -c credential.helper='!f() { echo username=dlouapre; echo "password=$HF_TOKEN"; }; f' push
```

Commit and push only when David asks.

## Template updates

`npm run sync:template` copies every template file outside `app/src/content/`, `README.md` and `.gitignore`. It brings back the demo files removed at initialization and overwrites `CLAUDE.md` and `.gitattributes`. Always run `npm run sync:template:dry` first, then restore these two files and delete the demo files again.
