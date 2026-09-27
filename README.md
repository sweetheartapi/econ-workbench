# Econometrics Workbench

An interactive self-study site for econometrics: intuition first, live R apps, then the maths with every symbol translated. Everything runs in the browser (Shinylive + webR), so the site is plain static files: no server, no R needed by students.

## What's inside

```
index.qmd, about.qmd          home page and "how to use this site"
lessons/                      3 lessons, each with Shinylive apps (8 apps in total)
  01-regression.qmd             least squares, confidence intervals, omitted variable bias
  02-likelihood.qmd             coin likelihood, Challenger O-rings, sampling distribution of the MLE
  03-bootstrap.qmd              bootstrap machine, when the bootstrap fails
labs/                         3 labs with live, auto-checked R exercises (16 in total)
styles/theme.scss             the visual design
_extensions/                  quarto-ext/shinylive and r-wasm/quarto-live (vendored, don't edit)
.github/workflows/publish.yml builds and publishes to GitHub Pages on every push
```

## Quickest way to put it online (no installation)

The `econ-workbench-site.zip` download is the finished website. Unzip it and drag the folder onto https://app.netlify.com/drop, or upload its contents to any static web host. It must be served over http(s); opening `index.html` straight from your disk won't start the R engine.

To preview it locally, run this inside the unzipped folder and open http://localhost:8000:

```
python3 -m http.server 8000
```

## Editing and rebuilding

You need Quarto (1.5 or newer) and R with these packages:

```r
install.packages(c("shinylive", "shiny", "bslib", "knitr", "rmarkdown"))
```

Then from this folder:

```
quarto preview      # live preview while you edit
quarto render       # build the site into _site/
```

### Publishing with GitHub Pages

1. Put this folder in a GitHub repository (the `.gitignore` keeps `_site/` out).
2. Run `quarto publish gh-pages` once from your computer. This creates the `gh-pages` branch.
3. In the repository settings, set Pages to deploy from the `gh-pages` branch.
4. From then on, every push to `main` rebuilds and republishes automatically via the included workflow.

## Adding a new topic

Copy a lesson and its lab, and add both to the `sidebar` in `_quarto.yml`.

- **Apps** are `{shinylive-r}` blocks with `#| standalone: true`. Each is a complete Shiny app. Develop it in RStudio first with `shiny::runApp()`, then paste it in. Keep to base graphics, shiny and bslib where you can: every extra package adds download time for students.
- **Exercises** are `{webr}` blocks. An exercise has up to four parts sharing one `exercise:` id: a `setup: true` block, the student's block (with `______` blanks), a `check: true` block, and `.hint` / `.solution` divs. The check block sees the student's last value as `.result` and their environment as `.envir_result`, and returns `list(correct = TRUE/FALSE, message = "...")`.
- **Styling helpers** used in the lessons:
  - `::: {.bench .column-page-right}` frames an app on graph paper at full width.
  - `::: {.try-this}` is the challenge list under each app.
  - `::: {.decoder}` is the symbol-by-symbol formula table; start it with `[Formula decoder]{.decoder-title}`.
  - `[text]{.scribble}` in a `::: {.column-margin}` block is a handwritten margin note (add `.blue` for blue ink).
  - `<span class="key">…</span>` is the highlighter for the one sentence that matters most.

## Notes

- The first app or code cell on a page takes a few seconds to load R (around 20–30 MB, cached afterwards).
- Content follows the lecture notes *Regression basics*, *Maximum likelihood* and *The bootstrap* by Lukáš Lafférs (Matej Bel University). The footer credits them; if you publish publicly and aren't the author, check with him first.
