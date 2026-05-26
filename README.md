# Li Lab Website

Source code for the [Li Lab](https://dlilab.com) website. Built with [Hugo](https://gohugo.io/) using the [hugo-ivy](https://github.com/yihui/hugo-ivy) theme.

```bash
git clone --recursive https://github.com/dlilab/lab_website.git
```

## Local preview

```bash
hugo server
# open http://localhost:1313
```

## Site structure

```
content/          # Page content (Markdown)
  _index.md       # Homepage — news items go here
  publications/   # Publications page intro text
  research/       # One .md file per research topic
  people/         # People page (content only; data is in data/)
  teaching.md
  opportunities.md
data/
  papers.json     # All publications (CSL JSON, exported from Zotero)
  people/
    current.toml  # Current lab members
    past.toml     # Alumni
static/
  mugshots/       # Profile photos (160×160 px recommended)
  images/         # Lab photos and other images
  pdf/            # CVs and paper PDFs
layouts/          # Hugo templates (override theme defaults)
themes/hugo-ivy/  # Base theme (git submodule)
config.toml       # Site-wide settings
```

---

## How to update

### Add a news item

Edit `content/_index.md`. News items are plain Markdown under the relevant year heading.

---

### Add a publication

1. Export the paper from Zotero as **CSL JSON** (File → Export Library → CSL JSON).
2. Copy the new entry from the exported file and append it to `data/papers.json`.
3. The Publications page and year navigation update automatically.

Optional fields recognized by the template (add to the paper's JSON object if needed):

| Field | Purpose |
|-------|---------|
| `github` | Link to code repository |
| `dryad` | Link to Dryad data repository |
| `figshare` | Link to Figshare data repository |
| `biorxiv` | Link to bioRxiv preprint |
| `coverImage` | Link to journal cover image |
| `EditorChoice` | Link to Editor's Choice page |
| `pressrelease` | Array of `{ "presslink": "..." }` objects |

Author name formatting on the Publications page is automatic — do **not** add bold or `§` markup manually to `papers.json`. See below.

---

### Add a current lab member

Add an entry to `data/people/current.toml`:

```toml
[UniqueKey]
  name = "First Last"
  mugshot = "mugshots/filename.jpg"   # place photo in static/mugshots/
  text = [
    "**Role**. Short bio sentence."
  ]
  role = "PhD student"   # Postdoc | Research Associate | PhD student | Undergraduate student
  when = "2025-"
  cite_given  = "First"              # given name exactly as it appears in paper citations
  cite_family = ["Last"]             # family name(s) exactly as they appear in citations
                                     # list multiple if the person uses variant spellings
  [UniqueKey.social]
    email = "email@wisc.edu"
    www = "https://..."
    github = "githubusername"
    twitter = "twitterhandle"
    orcid = "https://orcid.org/..."
    cv = "/pdf/filename.pdf"
```

Place the profile photo in `static/mugshots/`. All social fields are optional.

The `cite_given` / `cite_family` fields drive automatic author formatting on the Publications page:

- All lab members: name rendered in **bold**
- `role = "Postdoc"`: <sup>§</sup> prepended
- `role = "PhD student"`: <sup>§§</sup> prepended

If a person's citation name differs from their display name (hyphenated surnames, middle names, nicknames), set `cite_given` / `cite_family` to match exactly what appears in the paper's author list. Multiple family name variants can be listed: `cite_family = ["Reyes", "Reyes-Mendez"]`.

Display order on the People page: PI → Postdoc → Research Associate → PhD student → Undergraduate student.

---

### Move a member to alumni

1. Remove their entry from `data/people/current.toml`.
2. Add an entry to `data/people/past.toml`, keeping the `cite_given` / `cite_family` fields:

```toml
[UniqueKey]
  name = "First Last"
  mugshot = "mugshots/filename.jpg"
  text = ["**Role**. Bio."]
  role = "PhD student"
  when = "2021-2025"
  currentPosition = "Postdoc, Some University"
  cite_given  = "First"
  cite_family = ["Last"]
```

---

### Add a lab photo

1. Place the image in `static/images/`.
2. Add a `<div>` block in `layouts/people/list.html` before the closing `</main>`:

```html
<div class="widerimg">
  <img src="/images/your_photo.jpeg">
  <p class="condensedlines">Caption text here.</p>
</div>
```

---

### Add a research topic

Create a new Markdown file in `content/research/`, e.g. `content/research/new-topic.md`:

```markdown
---
title: Topic Title
image: /topics/image.jpg
weight: 7
papers:
  - citationKey1
  - citationKey2
Summary: One paragraph summary of this research theme.
---

Extended description (optional).
```

Place the topic image in `static/topics/`. The `papers` list uses citation keys from `data/papers.json`.

---

### Update the Google Analytics ID

Change `googleAnalyticsID` in `config.toml`.
