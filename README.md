# marko-jovanovic-portfolio

Personal portfolio and CV website of Marko Jovanović, System Administrator.

Live: **https://marko.jovanovic.pro**

Plain HTML, CSS and JavaScript. No framework, no build step. The site is bilingual (Serbian / English) and the CV page is designed to print on a single A4 sheet.

## Structure

```
.
├── .github/workflows/main.yml   # deploy on push to main
└── web/                         # everything that gets published
    ├── index.html               # portfolio page
    ├── styles.css               # styles for index.html
    ├── script.js                # translations (sr/en) and certificate expiry
    ├── cv.html                  # CV page with its own sr/en dictionary
    ├── cv.css                   # screen and A4 print styles for the CV
    └── assets/
        ├── marko.jpg            # CV photo
        ├── favicon.png          # favicon and apple-touch-icon
        └── og-image.png         # 1200×630 image for link previews
```

## Run locally

```bash
python3 -m http.server 8000 --directory web
```

Then open http://localhost:8000.

## Deployment

Every push to `main` (or a manual run of the workflow) starts **Deploy website** in GitHub Actions. It connects to the server over SSH with host key verification, runs `git pull --ff-only` in the server's copy of the repository and syncs `web/` to the web root with `rsync`.

Required repository secrets:

| Secret | Purpose |
|---|---|
| `DEPLOY_SSH_KEY` | private key of the deploy user |
| `SSH_KNOWN_HOSTS` | server host key for host verification |

## Editing content

**Languages.** Texts on `index.html` are marked with `data-i18n="key"` and translated from the `translations` object in `script.js`. The CV uses `data-t="key"` and the dictionary inside `cv.html`. The chosen language is stored in `localStorage` under `portfolio-language`, so it carries over between the two pages.

**Certificates.** Each certificate has its expiry date in a `data-expires="YYYY-MM-DD"` attribute (on the card in `index.html` and on the list item in `cv.html`). When the date passes, the "expired" label appears automatically. After renewing a certificate, update the date and the years shown in the text.

**CV printing.** The CV must fit on one A4 page. After any content change, check the print preview (Ctrl+P) in both languages. Font sizes for screen and print are defined at the top of `cv.css`.

**Link previews.** `og-image.png` is shown when a link is shared (LinkedIn, Slack, etc.). Open Graph tags are in the `<head>` of `index.html` and `cv.html`.
