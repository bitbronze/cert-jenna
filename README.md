# Cert Jenna

Self-signed certificates, generated in the browser. One file, no build step, no server.

## Deploying to GitHub Pages

This site is a single static `index.html` at the repository root, so it deploys with the
branch-based flow and needs no workflow file:

1. Push this repository to GitHub with the default branch set to `main`.
2. Go to **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`.
4. Save. The site is published at `https://bitbronze.github.io/cert-jenna/` within a minute or two.

Notes:

- `.nojekyll` is included so Pages serves the files as-is instead of running them through
  Jekyll. Keep it even though nothing here uses Jekyll — it prevents future surprises with
  files or folders whose names begin with an underscore.
- HTTPS is required for the generator to work: `crypto.subtle` is only exposed in a secure
  context. GitHub Pages serves HTTPS by default, and `localhost` counts as secure too.
- Opening `index.html` straight from disk (`file://`) will not generate anything, because
  browsers withhold `crypto.subtle` from that origin. Serve it over HTTP from `localhost`
  to test locally.

### If you rename the repository or use a custom domain

The published URL appears in `index.html` (canonical link, Open Graph and JSON-LD blocks),
`llms.txt`, `robots.txt`, `sitemap.xml` and `404.html`. Update all six if the address
changes. For a custom domain, also add a `CNAME` file containing the bare domain.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: markup, styles, and certificate generation logic. |
| `llms.txt` | Plain-text summary and technical reference for language models. |
| `robots.txt` | Crawler policy, points at the sitemap. |
| `sitemap.xml` | Single-page sitemap. |
| `404.html` | Fallback page for unknown paths, styled to match. |
| `.nojekyll` | Tells Pages to skip Jekyll processing. |

## Naming

The app is **Cert Jenna**. The hero wordmark styles it as Cert(ificate) Jenna(rator) as a
visual gag; metadata, this README and `llms.txt` use the real name, with the wordmark
recorded as an alternate name in the JSON-LD block.

## What it generates

- **Gen Simple** — `server.crt`, `server.key`. Signed by a throwaway root that is not
  included, so nothing is left behind that could be trusted as a CA.
- **Gen Complete** — `root.crt`, `root.key`, `server.crt`, `server.key`, `client.crt`,
  `client.key`. A real CA you can import into a trust store, plus server and client leaves.

RSA-2048 keys, SHA-256 signatures, version 3 certificates, a single `CN` in the subject,
and a `dNSName` SAN only when the app name is a valid DNS name. See `llms.txt` for the
full extension list and known limitations.

## Caveats

These certificates are for development. Don't ship them, don't load the private keys into
anything long-lived, and remember that a root CA you generate and import is trusted for
every site it signs.
