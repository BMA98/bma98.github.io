# bastian.cl

Personal site, served at <https://www.bastian.cl> by GitHub Pages from the `master` branch.

Plain HTML and CSS. No build step, no dependencies, no JavaScript.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The English page |
| `es/index.html` | The Spanish page |
| `404.html` | Not-found page, bilingual |
| `style.css` | Shared styles |
| `profile.jpg` | Profile photo |
| `favicon.ico` | Browser tab icon |
| `CNAME` | Custom domain for GitHub Pages |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is |

## Languages

English is served at `/` and Spanish at `/es/`. They are two separate HTML files
with no shared templating, which is what keeps the site free of a build step and
of JavaScript. A dropdown in the corner — a `<details>`, like the project panel,
so it opens without a script — lists the languages, and matching `hreflang` tags
in both heads tell search engines they are translations rather than duplicates.

**Editing one language means editing the other.** Nothing enforces it, so a
change to the bio in `index.html` that is not mirrored into `es/index.html`
leaves the site quietly disagreeing with itself. Apart from the prose, the two
files differ only in their `canonical` tag and in which entry of the dropdown is
the link, so `diff index.html es/index.html` is a practical way to see what is out
of sync. The `translations` skill in `.claude/skills/` has the full list.

Adding a third language means copying `es/` to a new directory, translating it,
and adding one `hreflang` line to every other page.

## Editing

Change the HTML, commit to `master`, push. GitHub Pages redeploys in about a minute.

Preview with a local server. Every asset path is root-relative, so opening the
file directly from disk loads the page without its stylesheet:

```sh
python3 -m http.server 8000
```

## Images

Strip metadata before committing a photo. Phone cameras embed GPS coordinates
and a capture timestamp, and anything committed here is served publicly.
