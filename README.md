# Carpet Studios website

The static site for Carpet Studios and its first game, Junk Flight. It is plain HTML and one stylesheet, with no framework, no build step, and no third-party requests.

```
index.html            Home: Junk Flight hero, screenshots
privacy/index.html    Privacy policy (Junk Flight + this site)
support/index.html    Contact and FAQ
404.html              Not-found page (GitHub Pages serves it automatically)
assets/css/site.css   The only stylesheet
assets/brand/         Studio wordmark (transparent PNG)
assets/icons/         Favicons and Apple touch icon
assets/img/           Key art, screenshots, app icon (WebP), share image (JPEG)
favicon.ico           Fallback favicon for custom-domain root
.nojekyll             Tells GitHub Pages to serve files as-is
```

## Preview locally

From the repo root:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000/>. Use a local server rather than opening the files directly: the folder links (`privacy/`, `support/`) and the Content Security Policy expect to run over HTTP.

## Before launch: replace the placeholders

| Placeholder | Where | Replace with |
| --- | --- | --- |
| `EMAIL-PLACEHOLDER@example.com` | Footers, privacy, support | The contact email |
| `https://testflight.apple.com/join/TESTFLIGHT-PLACEHOLDER` | Home, support | The public TestFlight link |

Find them all with:

```sh
grep -rn PLACEHOLDER --include='*.html' .
```

The share-image tags (`og:image`, `twitter:image`) use relative paths so the site works on any URL. Some services, Facebook in particular, only accept absolute URLs. Once the final address is known, you can make those tags absolute, for example `https://carpetstudios.example/assets/img/og-image.jpg`.

## Deploy on GitHub Pages

1. Push this repo to GitHub.
2. In the repo, open **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, and save.
4. The site appears at `https://<user>.github.io/carpet-studios/` within a minute or two.

All paths are relative, so the same files work at that project URL and later on a custom domain. To add a custom domain, enter it under **Settings → Pages → Custom domain**, set the DNS records GitHub shows, and turn on **Enforce HTTPS**.

## Notes

- **No third-party requests.** Every page sets a Content Security Policy that only allows its own images and stylesheet, so an accidental external font, script, or embed will be blocked. That matches the privacy policy.
- **The one script** lives in `404.html`. GitHub Pages serves that page at any missing URL, at any depth, so the script sets a `<base>` that points relative links at the site root. If you edit the script, update the `sha256-…` hash in that page's CSP, or the browser will block it.
- **Brand colors.** The studio red `#EE3224` is sampled from the logos. Small text links use a darker shade, `#C42414`, so they meet WCAG AA contrast on the cream background. Junk Flight uses ocean blue `#2F86F6` with navy `#16356A`.
- **No secrets.** The site needs no keys or credentials. Keep it that way.
