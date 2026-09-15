---
name: translations
description: How this site handles languages — English at /, Spanish at /es/, and the rule that a content change is not finished until it exists in both files. Apply whenever editing page content, adding a page or a project, or adding a language.
---

# Translations

## The site is bilingual and nothing enforces it

English is served at `/` and Spanish at `/es/`. They are two hand-written HTML files with no
shared templating, because the site has no build step and no JavaScript, and I want to keep it
that way.

The whole cost of that decision lands in one place: **a content change is not finished until it
exists in both files.** Nothing fails, nothing warns. Editing the bio in `index.html` and
stopping there leaves the site telling two different stories depending on which link someone
opened, and I will not notice until somebody tells me.

So: any edit to prose, any new project panel, any new link — make it twice, in the same commit.
Never leave a translation for "the next commit". A half-translated site that is shipped is worse
than one that was never touched, because the English side looks finished.

## What is allowed to differ between the two files

Only these. Everything else — tags, nesting, class names, `href`s to external sites, SVG paths —
must be identical, and that is what makes drift detectable:

| Thing | `index.html` | `es/index.html` |
| --- | --- | --- |
| `<html lang>` | `en` | `es` |
| `<link rel="canonical">` | `https://www.bastian.cl/` | `https://www.bastian.cl/es/` |
| `<summary>` of the dropdown | `EN` | `ES` |
| The dropdown's two `<li>` | `English` is a plain `<span aria-current="true">`, `Español` is the link | mirrored |
| `aria-label` on `nav.lang` | `Language` | `Idioma` |
| `aria-label` on the mail link | `Email` | `Correo` |
| `<meta name="description">` | English | Spanish |
| Visible prose | English | Spanish |

Which gives the drift check:

```sh
diff index.html es/index.html
```

A clean run shows those differences and nothing else. Anything beyond them is either a structural
mistake or a translation that has not been mirrored yet. Run it before committing a content
change.

## Translating

Chilean Spanish, written as I would write it, not as a translation reads. Some specifics that have
already come up:

- **Do not translate proper nouns or project names.** "Movie History" stays "Movie History".
  "The Movie Database", "Enerlink", "Universidad de Chile" likewise.
- **Do not translate developer jargon that is used in English in Chile.** backend, frontend,
  endpoints, driver, Team Lead, and the `Back` / `Front` / `Demo` button labels all stay as they
  are. Translating them reads as stilted, not as more Spanish.
- **My degree is `Ingeniero Civil en Computación`.** The English page says "Computer Science
  Engineer"; the Spanish page uses the real Chilean title, not a back-translation of the English.
- Keep the tone of the English: plain, first person, no marketing register.

## The 404 page is one bilingual file, not two

GitHub Pages only documents a custom 404 at the **root** of the site. A `es/404.html` is not
reliably served for a missing page under `/es/`, so there is a single `404.html` carrying both
languages, two lines, English first. Add a language there by adding a line, not a file.

## Adding a language

1. Copy `es/` to the new code (`fr/`, `de/`, …) and translate the prose.
2. Set `<html lang>` and the `canonical` in the new file.
3. Add one `<link rel="alternate" hreflang="…">` line to **every** page, including the new one.
   The set must be identical in all of them, must be self-referencing, and keeps
   `hreflang="x-default"` pointing at `/`.
4. Add one `<li>` to the dropdown in **every** page, keeping the languages in the same order
   everywhere. It is a plain `<span aria-current="true">` on its own page and a link everywhere
   else, with `lang` and `hreflang` on the links.
5. Add a line to `404.html`.
6. Update the file table and the Languages section in `README.md`.

Steps 3 and 4 are per-page and grow with the square of the number of languages, so this is where
the two-file approach starts to cost real effort. A third language is survivable; a fourth is the
moment to reconsider whether a build step is worth it.

## The dropdown is a `<details>`, and that is deliberate

The language menu is a `<details>`/`<summary>`, the same element the project panel uses. A native
`<select>` would need a script to navigate on change, and a CSS hover menu would be unusable on a
phone, so `<details>` is the only dropdown that works here without JavaScript.

It inherits one wart from that: clicking elsewhere on the page does not close it, only clicking the
summary again does. The project panel behaves the same way. That is accepted, not overlooked — do
not add a script to fix it.

## Do not "fix" this with JavaScript

The obvious refactor is to collapse both files into one and swap the text with a script. It has
been considered and rejected: it would mean a single URL, and social scrapers do not run
JavaScript, so every link shared in Chile would preview in English. The duplication is deliberate.
Do not propose it again unless I ask for it.

## Checklist before committing a content change

- [ ] The change exists in `index.html` **and** `es/index.html`.
- [ ] `diff index.html es/index.html` shows only the rows in the table above.
- [ ] If a page was added: `hreflang` set, toggle, `canonical`, `README.md`.
- [ ] Previewed through `python3 -m http.server`, not by opening the file — asset paths are
      root-relative and will not resolve from disk.
