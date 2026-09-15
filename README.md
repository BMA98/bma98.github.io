# bastian.cl

Personal site, served at <https://www.bastian.cl> by GitHub Pages from the `master` branch.

Plain HTML and CSS. No build step, no dependencies, no JavaScript.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole page |
| `404.html` | Not-found page |
| `style.css` | Shared styles |
| `profile.jpg` | Profile photo |
| `favicon.ico` | Browser tab icon |
| `CNAME` | Custom domain for GitHub Pages |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is |

## Editing

Change the HTML, commit to `master`, push. GitHub Pages redeploys in about a minute.

To preview locally, open `index.html` in a browser, or run a server so that
root-relative paths resolve the same way they do in production:

```sh
python3 -m http.server 8000
```

## Images

Strip metadata before committing a photo. Phone cameras embed GPS coordinates
and a capture timestamp, and anything committed here is served publicly.
