# Instructions for coding agents

This repo is the Hugging Face Space of the blog post "Natural Language Processing before LLMs" (chapter 0 of the series "Foundations of Large Language Models: Learning by Doing", based on David Louapre's MVA course). It is a separate git repo cloned inside the course repo, at `blog/00-nlp-before-llms/space/`. The parent folder holds the outline (`../outline.md`) and the code of the exercises and figures (`../code/`). The course repo has its own `CLAUDE.md` and `README.md`.

The Space is `dlouapre/nlp-before-llms` (renamed from `dlouapre/nlp-before-transformers` on 2026-10-19). The post was published on 2026-10-21: the Space is public, and chapter 0 has its `url` in `series.json`. Every push to `main` now changes the public page.

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

## Components of the post

The post has its own components in `app/src/components/post/`, on top of the template's. Each one says in its header how to use it.

| Element of the outline | Component |
|------|-------|
| Header of the page | `SeriesHeader.astro`, rendered by `pages/index.astro` in place of the template's hero because `article.mdx` has `chapter: 0` in its frontmatter. It shows a band with the name of the series, the author and the map of the chapters, then "Chapter 0", the title, the `description`, and the chapter illustration `embeds/banner.html`. The list of chapters is `series.json` in the public dataset `dlouapre/foundations-of-llms`, shared by all the posts of the series and described in its dataset card: the header reads it at build time and again when the page opens. A chapter is a link only when it has a `url`. Its styles also color the headings of the post: sections (`##`) in the orange of the series, subsections (`###`) in the blue of the exercises. |
| `{{TAG: ...}}` not implemented yet | `<Placeholder tag="FIGURE (recurring)">...</Placeholder>`: the text of the tag, verbatim. A button at the bottom right of the page hides all placeholders. |
| EXERCISE | `<Exercise number="2.1" title="..." data="WikiText-2" nb="02-1-markov-chain">` (`star` for ★; `nb` shows the compute time in the header), containing in order: the statement (markdown), `<Starter nb="..." />`, `<Hints>` with one `<Hint>` per hint, `<Checkpoint nb="...">`, `<Solution nb="..." />`, `<WhatToNotice>`. The box is folded when the page opens, and the Show/Hide button of its header unfolds it. A link to the exercise opens it. In the starter code, and in the first cell of the solution, which repeats it, a script of `Exercise.astro` folds the code under each marker `# [Copied from ...]`, which sits right under the comment that describes the copied code (see `blog/exercise-guidelines.md`), with a button "Show the N copied lines". The folded lines stay in the code block, so the copy button still copies the whole cell, and the line numbers count them. |
| BONUS | `<Bonus>` (`<Bonus extra>` for BONUS (extra)). Consecutive bonuses form one list. |
| PAPER, margin note (also for the tags that say "card": the post has no paper cards) | `<PaperNote id="shannon1948" look="Section 3, ...">`, right after the paragraph or block it belongs to. Its layout is fixed: "Authors · year", the title (linked to the paper, not bold) and the venue, all from `papers.json`; then the note (the slot, David's text); then `look`, optional: the figure, table or section of the paper worth looking at; last, a link to the paper in the reference list. |
| PAPER, citation | `[@key]`, with the entry in `bibliography.bib`. In a figure caption (`desc`, HTML), write the reference as a link to the reference list: `<a href='#bib-key'>Author (year)</a>`. |
| FIGURE | `<HtmlEmbed src="d3-....html" data="....json" title="..." desc="..." />`, with the embed in `app/src/content/embeds/` and its data in `app/src/content/assets/data/`. Data computed from the exercises come from their notebooks (see "Exercises"). |
| Scoreboard update | `<HtmlEmbed src="d3-scoreboard.html" data="scoreboard-wikitext2.json" config={{ upTo: "2.3", highlight: ["2.3"] }} ... />`: the rows that enter the post up to the section `upTo`, sorted by score, with the rows of the sections in `highlight` marked "new". Since 2026-10-21, the scoreboards are not shown in the post: the interlude of section 6 gives the best scores in a markdown table, written by hand. The embed and its data files are kept. |
| Hard reviews | `<HtmlEmbed src="d3-hard-reviews.html" data="hard-reviews.json" config={{ upTo: "5.5" }} ... />`: one row per hard review, one column per model that enters the post up to the section `upTo`, each cell the probability of being positive. Not shown in the post since 2026-10-21; the notebooks still add their models to `hard-reviews.json`, which `banner.html` reads. |
| A markdown table with phrases in its cells | wrap it in `<WrapTable>`: the template keeps each cell on one line. |
| INSET | `<Inset title="..." id="...">`. |
| Optional reading: a tangent that most readers can skip | `<Optional title="Latent Semantic Analysis" subtitle="another way to get dense vectors">` (markdown inside). Folded when the page opens, it shows one line: a book icon, the tag "Optional", and "**title**: subtitle". Its border is green (`--optional-color`, defined in `Optional.astro`), a third color next to the orange of the post and the blue of the exercises. A margin note about the same topic goes right after `</Optional>`, not inside, so that it stays visible when folded. Consecutive optional readings form one list. |
| References | `<ReferenceList />`: every paper of `papers.json`, by year, with links to the sections where it appears. |

`app/src/content/assets/data/papers.json` lists every paper of the PAPER tags: year, title, venue, link, and the sections where it appears and how (note, cite). It feeds the margin notes and the reference list. Its `key` is the key of the paper in `bibliography.bib`. The template's automatic bibliography is turned off (`suppressBibliography: true` in `astro.config.mjs`): the in-text citations `[@key]` link to the item `bib-<key>` of the reference list, so a cited paper must be in both `bibliography.bib` and `papers.json`. When a PAPER tag is added or moved in the outline, update `papers.json`.

`scoreboard-<board>.json` (`wikitext2` and `imdb`) holds the rows of a scoreboard. Rows with a `notebook` field are written by the exercise notebooks; the other rows are published results, written by hand with their `source`, their `key` in `papers.json` and the `section` where they enter the post.

The two recurring figures of the outline (the encoder–head diagram and the scoreboards) are kept as an MDX comment in `chapters/00-introduction.mdx`, with the checked reference rows of the scoreboards. The encoder–head diagram is `embeds/d3-encoder-head.html`, first shown in section 3.1 with an encoder marked "?"; a later section shows its own encoder with `config={{ encoder: "AVERAGE", sub: "" }}`. Section 4.1 has its own version of it, `embeds/d3-embedding-aggregation.html`, where the encoder is split into the embeddings and an aggregation (average, weighted average, or concatenation of the last 3 embeddings), with a toggle between the three. Section 5 has three figures of the same visual language: `d3-word2vec-models.html` (Bengio's model, CBOW, skip-gram on one sentence), `d3-analogy-directions.html` (data from `05-4-analogies`) and `d3-transfer.html` (the embedding matrix from scratch, frozen or fine-tuned). Section 7 opens with `d3-order-encoders.html`, the encoder of section 4.1 with a convolution or a recurrent network as its aggregation (a toggle between the two, made-up values); `d3-lstm-reading.html` shows the trained LSTM of exercise 7.2b reading the two hard reviews with the same words, and `d3-char-samples.html` the samples of the character-level LSTM during its training. Section 8 opens with `d3-pretrain-encoder.html`, in the visual language of `d3-transfer.html`: a language model on the left, the classifier of section 7.2 on the right, and a toggle between copying the embeddings only or the embeddings and the LSTM (made-up values). Section 8.2 has `d3-contextual-vectors.html`, schematic: the vector of "bank" as an arrow, one per word (GloVe) or one per occurrence (ELMo). Section 9 has `d3-seq2seq.html`, in the visual language of `d3-order-encoders.html`: an encoder RNN reads "I loved this great movie" and a decoder RNN writes "j'ai adoré ce film génial" (made-up values). With `config={{ mode: "encoder-decoder" }}` (9.1), the last state of the encoder is the only link; with `mode: "attention"` (9.2), links from all the states of the encoder show the weights of the attention for the word under the mouse. Section 10 has `d3-self-attention.html`, in the same visual language: a layer of self-attention reads the review B of section 7 (input vectors = word + position), with faint links from every word to every word and the weights of the token under the mouse (made-up values). With `config={{ mode: "attention" }}` (10.1), one layer and no head; with `mode: "bert"` (10.2), the tokens [CLS] and [SEP] are added, and the new vector of [CLS] goes to the representation, the head and the probabilities: the encoder–head diagram one last time.

The chapters are in `app/src/content/chapters/`, one file per section of the outline, imported in order by `article.mdx`.

## Exercises

- Each exercise has a notebook in `../code/` (course repo), named after its number: `02-1-markov-chain.ipynb` for exercise 2.1, `02-3a-ngram-counts.ipynb` for exercise 2.3a. An exercise has the number of the section that holds it, so that "exercise 4.3" and "section 4.3" are the same place. When a section holds two exercises, a letter tells them apart: exercises 2.3a and 2.3b are both in section 2.3. The exercise of chapter 6, which has no sections, is exercise 6. Sections without an exercise leave gaps: there is no exercise 3.1. This scheme replaced the numbering in order within a chapter on 2026-10-21. Write the notebooks following `blog/exercise-guidelines.md` in the course repo. Their cells carry tags: `starter`, `solution`, `checkpoint` (see the docstring of `tools/nb2mdx` in the course repo).
- `tools/nb2mdx --execute blog/00-nlp-before-llms/code/02-1-markov-chain.ipynb`, run from the root of the course repo, runs the notebook and writes its tagged cells to `app/src/content/exercises/02-1-markov-chain/` (`starter.mdx`, `solution.mdx`, `checkpoint.mdx`, and the PNG outputs). `solution.mdx` also exports `seconds`, the run time of the solution cells measured by `--execute`: the header of the exercise shows it as the compute time, rounded to the minute ("< 1 min" below 60 s). Never edit these files by hand. The `nb` prop of the exercise components is the name of the notebook.
- The solution cells must run as is, in order, in a fresh notebook (imports, data loading, seeds). Keep code lines under about 70 characters, so that they fit the exercise box without scrolling.
- The data of the figures that come from an exercise (samples, curves, counts) and its scoreboard rows are written by untagged cells at the end of its notebook, with `../code/post_data.py`: `post_data.save(name, data)` and `post_data.save_scoreboard_rows(board, notebook, rows)`. Running the notebook updates the figures; never edit these data files by hand. So far: `02-2-perplexity` writes the unigram row of the WikiText-2 scoreboard; `02-3a-ngram-counts` writes `ngram-samples.json` and `ngram-explorer.json`; `02-3b-smoothing` writes `ngram-curves.json` and the interpolated 5-gram row; `03-2-bag-of-words` writes `bow-weights.json` (the figure "A review as a bag of words") and the counts row of the IMDB scoreboard; `03-3-tf-idf` writes the presence and TF-IDF rows; `03-4-hard-reviews` writes `hard-reviews.json`, the hard reviews and the predictions of the bag of words, with `post_data.save_hard_reviews(notebook, reviews, models)`. `04-2-neural-language-model` writes the row of Bengio's model in the WikiText-2 scoreboard; `04-3-deep-averaging` writes the row of the deep averaging network in the IMDB scoreboard, and adds its predictions to `hard-reviews.json`; `04-4-neighbors` writes nothing. `05-2-word2vec` writes nothing; `05-4-analogies` writes `analogy-directions.json` (the figure "Relations are directions": 2D PCA projections of GloVe vectors, 9 pairs per relation, chosen so that points and arrows do not overlap); `05-5a-pretrained-dan` writes the two GloVe rows (frozen, fine-tuned) of the IMDB scoreboard and adds their predictions to `hard-reviews.json`; `05-5b-label-efficiency` writes nothing (its chart is a matplotlib output of the solution). `06-polysemy` writes nothing. `07-2a-vanishing-gradients` writes nothing (its chart is a matplotlib output of the solution); `07-2b-lstm-classifier` writes the LSTM row of the IMDB scoreboard, adds its predictions to `hard-reviews.json`, and writes `lstm-reading.json` (the figure "The LSTM reads the two reviews"); `07-3-lstm-language-model` writes the LSTM row of the WikiText-2 scoreboard. `08-1-pretrained-lstm` writes nothing: its classifiers are trained on 1,000 labels, so they are not scoreboard rows (the reference rows of section 8 are written by hand). `09-1-encoder-decoder` and `09-2-attention` write nothing: their charts (accuracy against the length, the map of the attention) are matplotlib outputs of the solutions. `10-2a-contextual-embeddings` writes nothing (the matrices of similarities of 10.2a are a matplotlib output); `10-2b-bert-fine-tuning` adds the predictions of BERT fine-tuned on 1,000 reviews to `hard-reviews.json` (not a scoreboard row: the BERT rows of the IMDB scoreboard are the published results of Sun et al. 2019, written by hand). The exercises of section 10 load `bert-base-uncased` from the Hub with `transformers`. On 2026-10-21, David dropped two exercises of section 10, self-attention without positions and the perplexity of GPT-2 on WikiText-2: their notebooks were deleted (they are in commit `db57999` of the course repo), the row of GPT-2 left the WikiText-2 scoreboard, and the two remaining exercises became 10.2a and 10.2b (both in section 10.2). `07-char-lstm-samples` is not an exercise (no tagged cells, run it with `jupyter execute --inplace`, not `nb2mdx`): it trains a character-level LSTM on TinyShakespeare and writes `char-lstm-samples.json` (the figure "A character-level LSTM learns to write a play"). The exercises of section 5 load the GloVe vectors (6B, 300d) from `sentence-transformers/average_word_embeddings_glove.6B.300d` (row 0 of the file is a padding token). Later notebooks that run the hard reviews add their model to this file.
- The visible text of the exercise (statement, hints, checkpoint) and the figure captions are drafted by Claude and edited by David. "What to notice" and the margin notes are David's.

## Key rules

- Embeds are self-contained `.html` files: one `<div>`, a scoped `<style>`, and an IIFE `<script>`. Colors come from `window.ColorPalettes`, theming from CSS variables (`--text-color`, `--surface-bg`, `--border-color`). Never hardcode colors.
- Citations: `[@key]`, with the entry in `bibliography.bib`. Do not put BibTeX comments in that file: braces in comments break the MDX parser.
- Math: `$...$` inline, `$$...$$` for blocks (KaTeX).
- `banner.html` in `embeds/` is the chapter illustration, under the title: a hard review of section 3.4 and the probability that a bag-of-words model gives it. It reads the probability from `hard-reviews.json` (the model "Bag of words, counts", written by the notebook of exercise 3.4), and falls back on a value written in the file. The file must exist: it is also hard-coded in `pages/dataviz.astro` and in the `paper` layout.
- The post is public. Before you reuse a picture from a course deck, check where it comes from: pictures from the web may not be reusable. Figures from the course `visuals/` folders and equations from `.eqn` files are David's own.
- Images, CSV and JSON files are stored with Git LFS (see `.gitattributes`). Hugging Face rejects binary files pushed outside LFS.

## Development

```bash
cd app
npm install
npm run dev        # dev server at http://localhost:4321
npm run build      # production build
```

In Claude Code's sandbox, Node reaches the Hub only through the proxy: build with `NODE_USE_ENV_PROXY=1 npx astro build`, or the map of the header falls back to the current chapter alone. Three things need to run outside the sandbox: Jupyter kernels (they bind local ports, so `nb2mdx --execute` fails), Chromium (Playwright screenshots of the built page), and `npm run dev`. Datasets download inside the sandbox with `HF_HUB_DISABLE_XET=1`.

## Deploy

`origin` is the Space, so `git push` deploys (the build takes a few minutes). Git has no credential helper for huggingface.co on David's machine. Pass the token from the `HF_TOKEN` environment variable for a single command, without saving it in `.git/config`:

```bash
GIT_TERMINAL_PROMPT=0 git -c credential.helper= -c credential.helper='!f() { echo username=dlouapre; echo "password=$HF_TOKEN"; }; f' push
```

Commit and push only when David asks.

The drafts of the post were squashed into one commit before publication, on top of the template's commits. Their history is kept in the local branch `drafts`, which is not pushed.

## Template updates

`npm run sync:template` copies every template file outside `app/src/content/`, `README.md` and `.gitignore`. It brings back the demo files removed at initialization and overwrites `CLAUDE.md` and `.gitattributes`. Always run `npm run sync:template:dry` first, then restore these two files and delete the demo files again. The sync also overwrites `app/src/pages/index.astro`: restore its three changes for the series header (the import of `SeriesHeader`, the `chapter` constant, and the branch that renders `<SeriesHeader>`). It also overwrites `app/src/components/HtmlEmbed.astro`: restore the figure title, which the post puts inside the card (a `div`, not a `figcaption`) and styles in bold, 1.1rem, in `--primary-color`. Never use `sync:template:force`: it would also overwrite `astro.config.mjs`, where the automatic bibliography is turned off.
