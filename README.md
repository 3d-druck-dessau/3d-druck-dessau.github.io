# 3d-druck-dessau.de

Website for 3D-Druck Dessau, 3D printing for businesses in Dessau-Roßlau.
Built with Jekyll.

## Running it

```sh
bundle install
bundle exec jekyll serve --livereload
```

Then open <http://localhost:4000>.

## Layout

```
index.html                the landing page
imprint.html              Impressum
datenschutz.html          Datenschutzerklärung (Händlerbund, plus site-specific notes)
agb.html                  AGB und Kundeninformationen (Händlerbund)
zahlung-und-versand.html  Zahlung und Versand (Händlerbund)
_layouts/druck.html       the one layout: head, header, footer, JSON-LD
assets/druck/             stylesheet, self-hosted fonts, form.js
```

## Email address and WhatsApp

Neither appears in the built HTML. `assets/druck/form.js` joins them from split
strings at runtime and fills `[data-email]`, `[data-phone]` and `[data-whatsapp]`
elements. The inquiry form opens the visitor's mail app with a pre-filled
message; there is no form service.

## Deploying

Pushing to `main` runs `.github/workflows/deploy.yml`, which builds the site
and publishes it to GitHub Pages. Pull requests build and run the link check
without deploying. There are no secrets and no credentials to rotate.

The custom domain comes from the committed `CNAME` file. DNS for the apex must
point at GitHub Pages:

```
A     3d-druck-dessau.de  185.199.108.153
A     3d-druck-dessau.de  185.199.109.153
A     3d-druck-dessau.de  185.199.110.153
A     3d-druck-dessau.de  185.199.111.153
CNAME www                 3d-druck-dessau.github.io.
```

HTTPS is automatic once *Enforce HTTPS* is enabled under Settings > Pages.

## Link checking

`script/check_links.rb` runs against `_site/` in CI and fails the build on
internal links that resolve to nothing, or on protocol-relative subresources.
Run it locally with:

```sh
bundle exec jekyll build && ruby script/check_links.rb
```
