# Cert Jenna

Self-signed certificates, generated in the browser. One file, no build step, no server.

If Cert Jenna saved you some time, [star the repository](https://github.com/bitbronze/cert-jenna).

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: markup, styles, and certificate generation logic. |
| `llms.txt` | Plain-text summary and technical reference for language models. |
| `robots.txt` | Crawler policy, points at the sitemap. |
| `sitemap.xml` | Single-page sitemap. |
| `404.html` | Fallback page for unknown paths, styled to match. |
| `.nojekyll` | Tells Pages to skip Jekyll processing. |
| `og-image.png` | Social preview image referenced by Open Graph and Twitter Card tags. |

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
