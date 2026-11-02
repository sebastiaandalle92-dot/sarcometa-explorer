# SarcoMeta Explorer: website and hosting guide

SarcoMeta Explorer is a static website: one `index.html`, a favicon and four JSON data files. There is no server, database or build step, so it can be hosted for free anywhere that serves files.

```
sarcometa-explorer/
├── index.html          the whole app (HTML, CSS, JavaScript)
├── favicon.svg
├── .nojekyll           tells GitHub Pages to serve files as-is
├── README.md           this guide
└── data/
    ├── meta.json        gene-level meta-analysis (14,107 genes), differences, mouse
    ├── studies.json     17 cohorts with per-gene log2FC and SE
    ├── pathways.json    fgsea results (Hallmark, Reactome, KEGG, GO:BP)
    └── composition.json cell-composition differences (pooled and per cohort)
```

Total size is about 6.7 MB, or 1–2 MB as served with compression.

## What the site does

- **Gene:** look up any gene; cards per condition, a forest plot of all 17 cohorts, between-condition difference tests and mouse results. Every gene has its own link, e.g. `…/#gene/NOS1`, which you can put in the paper or send to a colleague.
- **Top genes / Pathways:** ranked, filterable tables, each with a CSV download of the current filter.
- **Cell composition** and **Datasets:** as in the manuscript.
- **Download:** full result tables as CSV, generated in the browser.
- **About & cite:** summary, methods, caveats and a copyable citation.
- Light and dark themes; works on phones.

## Try it on your own computer

Opening `index.html` by double-clicking does not work, because browsers block reading the data files from disk. Start a small local server in the folder instead:

```
cd sarcometa-explorer
python -m http.server 8000
```

Then open http://localhost:8000. (Alternatives: in R, `servr::httd()`; in VS Code, the "Live Server" extension.)

## Option A (recommended): GitHub Pages

Free, stable, and gives a clean URL such as `https://<username>.github.io/sarcometa-explorer/`.

1. Create a GitHub account if you don't have one, and sign in.
2. Create a new **public** repository called `sarcometa-explorer` (New repository → Public → Create). GitHub Pages on a free account needs a public repository.
3. On the repository page, click **Add file → Upload files** and drag in the *contents* of this folder (`index.html`, `favicon.svg`, `README.md`, `.nojekyll` and the `data` folder). Commit.
   - Windows hides `.nojekyll` sometimes; it is not strictly needed, so don't worry if it doesn't upload.
4. Go to **Settings → Pages**. Under "Build and deployment", choose **Deploy from a branch**, branch **main**, folder **/ (root)**, then **Save**.
5. After about a minute the page shows "Your site is live at …". Open it and check a gene link, e.g. `…/#gene/ADIPOQ`.

To update later, upload the new files to the same repository (overwrite), and the site refreshes within a minute or two.

## Option B: make it citable with a DOI (Zenodo)

Journals and reviewers like a permanent, versioned record. Zenodo can archive the GitHub repository automatically:

1. Sign in at zenodo.org with your GitHub account.
2. In Zenodo, go to **GitHub** settings and switch the `sarcometa-explorer` repository **on**.
3. On GitHub, create a **Release** (Releases → Draft a new release → tag `v1.0`). Zenodo archives it and issues a DOI.
4. Put the DOI in the manuscript's Data Availability statement and in the "How to cite" block of `index.html` (search for `Manuscript in preparation`).

Each new release (v1.1, v2.0 …) gets its own DOI, plus a "concept DOI" that always points to the latest.

## Option C: University of Antwerp hosting or a custom domain

- **Custom domain** (e.g. `sarcometa.org`): buy the domain, then in GitHub **Settings → Pages → Custom domain** enter it and follow GitHub's DNS instructions. HTTPS is free.
- **University web space:** ask UAntwerp ICT whether your group has static web hosting; upload the same files there. No server-side software is needed.
- Other free static hosts that work the same way: Netlify (drag-and-drop the folder at app.netlify.com/drop), Cloudflare Pages, GitLab Pages.

## Before you make it public

- **Timing:** the site shows unpublished results. You may want to keep the repository private (or not enable Pages) until submission or acceptance. Many authors make the resource live at submission so reviewers can use it; check JCSM's policy and your co-authors' views.
- **Citation and contact:** edit the "How to cite", "Contact" and footer text in `index.html` (search for `Manuscript in preparation` and `University of Antwerp`). Once published, add the journal reference and DOI.
- **Licence:** add a `LICENSE` file. For summary results, CC BY 4.0 is common (GitHub: Add file → Create new file → name it `LICENSE` → "Choose a licence template").
- **Analytics (optional):** if you want to report usage, add a privacy-friendly counter such as GoatCounter (one line in `index.html`).

## Updating the data after a re-analysis

The site reads only the four JSON files, so re-running the pipeline and re-exporting these files is all an update needs. The formats are:

**`meta.json`** (all arrays have one entry per gene, in the same order as `genes`; missing values are `null`):

```json
{
  "genes": ["A1BG", "..."],
  "class": ["ageing_specific_down", "other", "..."],
  "sarc": {"lfc": [], "se": [], "z": [], "fdr": [], "i2": [], "k": [], "zadj": []},
  "age":  { "...same keys..." },
  "dis":  { "...same keys..." },
  "diff_sarc_age": {"z": [], "fdr": []},
  "diff_sarc_dis": {"z": [], "fdr": []},
  "diff_dis_age":  {"z": [], "fdr": []},
  "mouse": {
    "ageing":       {"lfc": [], "z": [], "fdr": [], "k": [], "i2": []},
    "disuse_adult": { "...same keys..." },
    "disuse_aged":  { "...same keys..." }
  }
}
```

**`studies.json`**: a list of cohorts, each `{"id", "gse", "cond" ("sarc" | "age" | "dis"), "contrast", "n_case", "n_ctrl", "lfc": [...], "se": [...]}`, with `lfc` and `se` in the same gene order as `meta.json`.

**`pathways.json`**: column arrays `pathway`, `collection` (`hallmark`, `reactome`, `kegg`, `go_bp`), and `NES_` / `padj_` for `sarcopenia`, `ageing`, `disuse`, `zdiff_sarc_age`, `zdiff_sarc_dis`.

**`composition.json`**: `{"meta": [{method, condition, type, diff, se, p, padj, k, I2}], "study": [{method, analysis, condition, type, diff, se}]}`, with differences as proportions (0.01 = 1 percentage point).

Round numbers to 3–4 significant digits when exporting; it keeps the files small without visible loss. In R, `jsonlite::write_json(x, "meta.json", digits = 4, na = "null", auto_unbox = TRUE)` produces this format from lists of vectors.

The numbers of cohorts and samples in the header (17, 677) and the group sizes on the About page are written in `index.html`; update them there if the cohort set changes.

## When to move beyond a static site

The static design handles this dataset comfortably. Consider a Shiny app (shinyapps.io or a university Shiny server) only if you later want server-side computation, such as users uploading their own gene signatures to score against the meta-analysis, or on-the-fly re-analysis with different cohort selections. Everything currently on the site works without that.
