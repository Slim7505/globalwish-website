# globalwish.net — Global Wish Co., Ltd.

The corporate website for グローバルウィッシュ株式会社 (Global Wish Co., Ltd.) in Japanese, English and Russian.

## How the site is built

The site is a static build. There is no server, database or tracking.

| Path | What it holds |
|---|---|
| `content/site.json` | Facts shared by every language: domain, email, phone, the page list and image sizes |
| `content/ja.json`, `content/en.json`, `content/ru.json` | Every visible word, one file per language. All three files must have the same keys, and the build stops if one is missing |
| `templates/` | Jinja2 templates: `base.html` plus one template per page |
| `assets/` | CSS, JavaScript and optimised WebP images |
| `build.py` | Writes the finished site to `dist/`: pages in `/ja/`, `/en/` and `/ru/`, a language chooser at `/`, `sitemap.xml` with hreflang links, `robots.txt`, `404.html` and `CNAME` |
| `check.py` | Fails the build on a broken internal link, a missing title, description or canonical tag, missing hreflang, more or fewer than one `<h1>`, a missing `alt`, or a wrong `lang` attribute |

To work on the site locally:

```bash
pip install jinja2==3.1.6
python build.py && python check.py
python -m http.server -d dist 8000   # then open http://localhost:8000/ja/
```

**Editing text:** change the same key in all three language files, then push to `main`. GitHub Actions (`.github/workflows/pages.yml`) builds the site, checks it and deploys it to GitHub Pages.

## Contact form

The site has no backend. The form checks the required fields, then opens the visitor's own email app with a pre-filled message to `sales@globalwish.net`. Nothing is stored on the site.

## Where the content came from

Everything was restored from the Wayback Machine captures of globalwish.net, taken between 2022 and January 2025.

- **Address:** the January 2025 captures show the Kawasaki address with TEL 044-201-4250 and FAX 044-589-4041, so the site uses those. Earlier captures show older addresses: Tsunashima, Yokohama (2022) and Nihonbashi, Tokyo (2023–24).
- **Prices:** they are not published, because the archived prices are from 2022–2023. Visitors are asked to send an inquiry or go to the Amazon store.
- **Health claims:** the archive's medical-sounding claims (for example blood pressure, immunity or weight loss) were left out to stay within Japan's 薬機法 and 景品表示法. Origin, taste and tradition were kept.

## Going live (one-time setup)

1. In GitHub, go to **Settings → Pages → Build and deployment → Source** and choose **GitHub Actions**.
2. At the domain's DNS provider, add these records:
   - `A` records on `@` pointing to `185.199.108.153`, `185.199.109.153`, `185.199.110.153` and `185.199.111.153`
   - `AAAA` records on `@` pointing to `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153` and `2606:50c0:8003::153`
   - a `CNAME` record on `www` pointing to `slim7505.github.io`
3. Once the DNS check passes, go to **Settings → Pages**, set the custom domain to `globalwish.net` and tick **Enforce HTTPS**.

> Changing the DNS records does not touch email. The `MX` records for `sales@globalwish.net` stay as they are.
