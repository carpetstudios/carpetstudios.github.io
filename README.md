# Carpet Studios website

The static site for Carpet Studios and its first game, Junk Flight. It is plain HTML and one stylesheet, with no framework, no build step, and no third-party requests.

```
index.html            Home: Junk Flight hero, screenshots
privacy/index.html    Privacy policy (Junk Flight + this site)
support/index.html    Contact and FAQ
404.html              Not-found page (GitHub Pages serves it automatically)
assets/css/site.css   The only stylesheet
assets/brand/         Studio logo (stacked) and header lockup (SVG)
assets/icons/         Favicons (SVG + PNG) and Apple touch icon
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

## Contact and beta links

The contact email (`carpetstudioshq@gmail.com`) and the TestFlight link (`https://testflight.apple.com/join/bAjfNBZ2`) appear on several pages. To change one, replace it everywhere:

```sh
grep -rln 'carpetstudioshq@gmail.com' --include='*.html' .
```

## Share image

The `og:url`, `og:image`, and `twitter:image` tags use full `https://carpetstudios.github.io/...` URLs, because some services, Facebook in particular, ignore relative ones. If you move to a custom domain, update them:

```sh
grep -rn 'carpetstudios.github.io' --include='*.html' .
```

## Deploy on GitHub Pages

The site is published from the `carpetstudios` GitHub organization, so it lives at the root of `https://carpetstudios.github.io/`.

1. Create the organization at <https://github.com/account/organizations/new> (the Free plan is enough) and name it `carpetstudios`.
2. In that organization, create an empty public repo named exactly `carpetstudios.github.io`. That name is what makes GitHub serve it at the organization's root.
3. Push this repo to it:

   ```sh
   git remote add origin https://github.com/carpetstudios/carpetstudios.github.io.git
   git push -u origin main
   ```

4. In the repo, open **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, and save.
5. The site appears at `https://carpetstudios.github.io/` within a minute or two.

The main pages use relative paths and work at any URL. `404.html` uses root paths (`/assets/...`) because GitHub serves it at any missing URL, at any depth, so it needs the site at a domain root: the organization site or a custom domain, not a `user.github.io/repo/` project path. To add a custom domain later, enter it under **Settings → Pages → Custom domain**, set the DNS records GitHub shows, and turn on **Enforce HTTPS**.

## Notes

- **No third-party requests.** Every page sets a Content Security Policy that only allows its own images and stylesheet, so an accidental external font, script, or embed will be blocked. That matches the privacy policy.
- **No JavaScript.** No page runs any script, and the CSP forbids it. If you ever add one, allow it in that page's CSP.
- **Brand colors.** The studio shell uses the logo colors: slate `#0F172A` background, slate `#E2E8F0` and `#94A3B8` text, and blue `#2563EB` for buttons and accents. The logos are drawn for that dark background, so don't put them on light colors. Small text links use a lighter blue, `#60A5FA`, because the brand blue is below WCAG AA contrast on slate. Junk Flight keeps its own look: ocean blue `#2F86F6`, navy `#16356A`, and cream.
- **No secrets.** The site needs no keys or credentials. Keep it that way.
