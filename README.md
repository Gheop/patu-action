# 🕷️ Patu Optimize (GitHub Action)

Optimize your build's images, SVG and fonts through [Patu](https://patu.dev) as a
single CI step. It runs [`@patu.dev/cli`](https://www.npmjs.com/package/@patu.dev/cli)
on the directory you point it at. Never bigger, never breaks your build.

Part of [**Patu for JavaScript**](https://github.com/Gheop/patu-js).

## Quick start

Grab a free key at [patu.dev](https://patu.dev), store it as a repository secret
named `PATU_KEY`, then add a step after your build:

```yaml
- run: npm run build            # produces ./dist

- uses: Gheop/patu-action@v1
  with:
    directory: dist
    api-key: ${{ secrets.PATU_KEY }}
```

Want the assets served from the edge instead? Switch to `cdn` mode:

```yaml
- uses: Gheop/patu-action@v1
  with:
    directory: dist
    api-key: ${{ secrets.PATU_KEY }}
    mode: cdn
```

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `directory` | yes | | Build output to optimize (`dist`, `build`, `out`, …). |
| `api-key` | yes | | Your Patu API key. Pass it from a secret, never inline. |
| `mode` | no | `optimize` | `optimize` (smaller bytes to disk) or `cdn` (serve from `cdn.patu.dev`). |
| `strict` | no | `false` | `true` fails the build if any asset fails to optimize. |
| `endpoint` | no | | Override the Patu API endpoint (self-hosted or staging). |
| `version` | no | `latest` | Which `@patu.dev/cli` version to run (pin it for reproducible CI). |
| `node-version` | no | `20` | Node.js version the action sets up. |

## What it does

In `optimize` mode (the default) it rewrites your images to AVIF/WebP inside a
`<picture>`, minifies your SVG and trims your fonts, writing the smaller bytes
back into the directory. In `cdn` mode it stores the assets on `cdn.patu.dev`,
rewrites the references (with Subresource Integrity), and keeps the local
originals as a fallback. Either way, a file that can't be beaten is left exactly
as it was, and a single failed asset never sinks the run (unless you set
`strict`).

See the [Patu for JavaScript README](https://github.com/Gheop/patu-js) for the
full story on the two modes and the guarantees.

## License

[MIT](./LICENSE).
