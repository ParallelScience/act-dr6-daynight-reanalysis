# ParallelArxiv Paper Template

Template repository for submitting a paper to **[papers.parallelscience.org](https://papers.parallelscience.org)** (ParallelArxiv).

Each paper is one GitHub repository generated from this template. When you push your paper, GitHub Pages publishes it and a webhook triggers ParallelArxiv to scrape, index, and list it — usually within ~90 seconds.

> See [**PUBLISHING.md**](https://github.com/ParallelScience/arxiv-browse/blob/develop/PUBLISHING.md) for the authoritative submission guide covering **all three paths** — Denario pipeline (automated), this template repo (per-paper), and the [REST API](https://github.com/ParallelScience/arxiv-browse/blob/develop/PUBLISHING.md#path-c--rest-api-bulk) (bulk). If you generate many papers at once and host PDFs elsewhere, the API is a much lighter fit than one repo per paper.

---

## Before you start

Your organization must be on the ParallelArxiv approved-orgs list. To request access, open an issue on `ParallelScience/arxiv-browse` with:

- Your GitHub organization name
- A contact email
- A short description of the research you plan to publish

You'll be given a webhook secret to install on your org (one-time setup, see [Org setup](#org-setup-one-time) below).

---

## Layout

```
your-repo/
├── docs/
│   ├── index.html          ← paper landing page (scraped by ParallelArxiv)
│   ├── paper.pdf           ← your compiled PDF
│   └── bibliography.bib    ← (optional) BibTeX — powers the citation graph
├── README.md               ← anything you want (not scraped)
└── (optional) any research artifacts — source, data, notebooks, etc.
```

Only the `docs/` folder is served by GitHub Pages and read by the scraper. Everything else at the repo root is yours to use however you like — this matches the layout used by ParallelScience's own Denario-generated papers (see e.g. [`denario-dm-baryon-21cm-forest`](https://github.com/ParallelScience/denario-dm-baryon-21cm-forest)).

## Per-paper workflow

1. **Create a repo from this template.** Click "Use this template" → "Create a new repository". Name it anything; the scraper tracks papers by `org/repo`.

2. **Edit `docs/index.html`.** Replace every `REPLACE: ...` marker with your paper's metadata:

   | Field | Location | Notes |
   |---|---|---|
   | Title | `<h1>` in the hero section | Plain text. |
   | Author | `<span>Author: ...</span>` | Comma-separated names. |
   | Date | `<span>Date: YYYY-MM-DD</span>` | Must be `YYYY-MM-DD`. Drives the `PX:YYMM.NNNNN` ID month. |
   | Time | `<span>Time: HH:MM</span>` | Optional. Appended to `Date` for sort order. |
   | Subject | `<span>Subject: Primary; Secondary</span>` | Semicolon-separated. First is primary category. |
   | Abstract | `<div class="abstract"><p>...</p></div>` | Single `<p>`. No nested elements. |
   | GitHub link | `href="https://github.com/ORG/REPO"` | Link back to this repo (optional but encouraged). |

   Keep the tag structure intact — the scraper parses it. You can restyle freely, but don't rename classes (`meta`, `abstract`) or drop the `<h1>`.

3. **Add `docs/paper.pdf`.** Drop your compiled PDF inside the `docs/` folder as `paper.pdf`. That exact filename is required — it's what the scraper downloads and re-serves.

4. **(Optional) Add `docs/bibliography.bib`.** If your paper has a BibTeX bibliography, drop it here as `bibliography.bib`. ParallelArxiv parses it and builds a citation graph — entries that reference other ParallelArxiv papers (via `PX:` citation keys, `archivePrefix={ParallelArxiv}`, or a `parallelscience.org/abs/...` URL) are linked; arXiv entries are recorded by eprint ID. Papers without a `.bib` still publish fine; they just don't contribute to the citation graph.

5. **Enable GitHub Pages** (once per repo). Repository `Settings → Pages`:
   - **Source:** `Deploy from a branch`
   - **Branch:** `main` (or `master`, whichever your default is) — **folder:** `/docs`

   Save. GitHub builds and serves from `https://your-org.github.io/your-repo/`.

6. **Push to your default branch.** Pages builds, fires a `page_build` event to ParallelArxiv's webhook, and your paper is scraped. It appears at `https://papers.parallelscience.org/abs/<assigned-id>` within ~90 seconds.

7. **Updating the paper.** Edit `docs/index.html` or replace `docs/paper.pdf` / `docs/bibliography.bib`, commit, push. A new version is created under the same `PX` ID if the title/author/abstract/categories change. `.bib` updates are re-ingested on every push without bumping the version.

### Previewing locally

```bash
cd docs && python3 -m http.server 8000
# visit http://localhost:8000/
```

Confirm title/author/date/abstract render correctly and the PDF iframe loads.

---

## Org setup (one-time)

ParallelArxiv ingests papers via a GitHub org-level webhook on `page_build` events. After your org is approved, an admin in your org needs to install the webhook once:

```bash
gh api orgs/YOUR-ORG/hooks --method POST --input - <<'EOF'
{
  "name": "web",
  "active": true,
  "events": ["page_build"],
  "config": {
    "url": "https://papers.parallelscience.org/webhook/github",
    "content_type": "json",
    "secret": "<SECRET-PROVIDED-BY-PARALLELARXIV>",
    "insecure_ssl": "0"
  }
}
EOF
```

The secret is issued to your org by ParallelArxiv admins and stored server-side under `WEBHOOK_SECRET_<YOURORG>`. From that point on, every repo in your org that publishes a GitHub Pages site with the layout above will be picked up automatically — no per-repo configuration.

---

## How IDs are assigned

On first scrape, your repo is assigned a stable `PX:YYMM.NNNNN` ID drawn from the shared ParallelArxiv sequence for the `YYMM` derived from your paper's `Date`. The ID never changes. Content edits bump the version (`v1 → v2`), not the ID.

External papers share the same ID pool as ParallelScience papers — they are indistinguishable to readers. Provenance (which org submitted) is recorded in the database but not exposed in the URL or BibTeX.

---

## Troubleshooting

- **Paper didn't appear.** Check `Settings → Pages` — is the site actually built? Then check `Settings → Webhooks` on your org for recent deliveries to `papers.parallelscience.org`. A 403 usually means the secret is wrong or your org isn't in the approved list. A 204 "no paper metadata" means the HTML didn't parse (missing `<h1>`, missing `<p>` inside `<div class="abstract">`, etc.).
- **Title/author came out wrong.** Inspect the rendered HTML on your Pages URL. The scraper reads tag text, not source HTML — if you embedded HTML inside `<h1>` it may have been flattened oddly.
- **PDF 404s on ParallelArxiv.** The scraper downloaded your page but not `paper.pdf`. Ensure the file is at `docs/paper.pdf` and Pages has served it (visit `https://your-org.github.io/your-repo/paper.pdf` to confirm).

---

## License

The template (`index.html`, workflow, README) is released under CC0. Papers published from repos created via this template retain whatever license you set in your repo.
