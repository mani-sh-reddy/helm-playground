# Helm Playground

This repository contains the source code for helm playground. Forked from shipmight/helm-playground

[![Screenshot of Helm Playground – Click to open](screenshot.png)](https://helm-playground.com)

## How it works

A piece of Go code is compiled to a [Wasm](https://en.wikipedia.org/wiki/WebAssembly) module which can be ran in the browser. This code implements a simple function which takes two inputs:

- YAML template
- YAML values

Then it simply renders the given template with the given values using [Sprig](https://github.com/Masterminds/sprig), which is also what Helm uses.

The Wasm module is compiled in a GitHub action. You can find the workflow in [`.github/workflows/compile.yaml`](.github/workflows/compile.yaml). When a commit is pushed to `master`, the workflow is triggered, the code is compiled and committed back to `master` with the commit message `[GitHub action] Wasm module`. The `master` branch is hosted live via GitHub Pages at https://helm-playground.com.

## Development

### Pull the repository

```bash
git clone git@github.com:shipmight/helm-playground.git
```

### Run tests for the golang code

```bash
make test
```

### Build Wasm from golang

```bash
make build
```

Note: do not commit changes in `dist/`. GitHub Actions builds and commits after your commit is merged.

### Test the built Wasm code in browser

```bash
yarn --cwd ./browser-test # Install puppeteer in the subfolder
make browser-test
```

### Locally develop in browser

You need a HTTP server to run the site locally, because fetch doesn't work under `file://` protocol.

```bash
npx http-server -c-1
```

### Updating dependencies

Dependency versions in `go.mod` should mirror a recent Helm release.

See the top comment in [`go.mod`](go.mod) for the current mirrored Helm version and upgrade instructions.

## License

Some files in this repository contain embedded license notes.

Other files in this repository are licensed under GNU AGPLv3 (see [LICENSE](./LICENSE)).
