<div align="center">

```
                    _     __ _
__      _____  _ __| | __/ _| | _____      _____
\ \ /\ / / _ \| '__| |/ / |_| |/ _ \ \ /\ / / __|
 \ V  V / (_) | |  |   <|  _| | (_) \ V  V /\__ \
  \_/\_/ \___/|_|  |_|\_\_| |_|\___/ \_/\_/ |___/
```

### I kept copying CI configs between repos. So I centralized them.

![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?logo=github-actions&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?logo=node.js&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?logo=go&logoColor=white)

[![License](https://img.shields.io/badge/license-MIT-brightgreen?style=flat-square)](LICENSE)

**Reusable GitHub Actions workflows for CI/CD pipelines**

[Node CI](#node-ciyml) · [Go CI](#go-ciyml) · [npm Publish](#npm-publishyml) · [Usage](#usage-example)

</div>

---

## Available Workflows

### `node-ci.yml`

General CI workflow for Node.js projects.

```yaml
jobs:
  ci:
    uses: jarero321/shared-workflows/.github/workflows/node-ci.yml@main
    with:
      package-manager: 'bun'  # npm, pnpm, bun
      run-lint: true
      run-typecheck: true
      run-tests: true
      run-build: true
```

| Input | Default | Description |
|-------|---------|-------------|
| `node-version` | `'20'` | Node.js version |
| `package-manager` | `'npm'` | npm, pnpm, or bun |
| `run-lint` | `true` | Run lint step |
| `run-typecheck` | `true` | Run type check step |
| `run-tests` | `true` | Run tests step |
| `run-build` | `true` | Run build step |
| `lint-command` | `'npm run lint'` | Custom lint command |
| `typecheck-command` | `'npm run typecheck'` | Custom typecheck command |
| `test-command` | `'npm run test:run'` | Custom test command |
| `build-command` | `'npm run build'` | Custom build command |

---

### `go-ci.yml`

CI workflow for Go projects.

```yaml
jobs:
  ci:
    uses: jarero321/shared-workflows/.github/workflows/go-ci.yml@main
    with:
      go-version: '1.23'
      main-path: './cmd/server'
      build-output: './bin/app'
```

| Input | Default | Description |
|-------|---------|-------------|
| `go-version` | `'1.23'` | Go version |
| `run-lint` | `true` | Run golangci-lint |
| `run-tests` | `true` | Run tests |
| `run-build` | `true` | Run build |
| `main-path` | `'./cmd/server'` | Path to main.go |
| `build-output` | `'./bin/app'` | Build output path |

---

### `npm-publish.yml`

Publish package to npm registry.

```yaml
jobs:
  publish:
    uses: jarero321/shared-workflows/.github/workflows/npm-publish.yml@main
    with:
      package-manager: 'bun'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

| Input | Default | Description |
|-------|---------|-------------|
| `node-version` | `'20'` | Node.js version |
| `package-manager` | `'npm'` | npm, pnpm, or bun |
| `build-command` | `'npm run build'` | Custom build command |

| Secret | Required | Description |
|--------|----------|-------------|
| `NPM_TOKEN` | Yes | npm authentication token |

---

## Usage Example

### Full CI + Publish on Release

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: jarero321/shared-workflows/.github/workflows/node-ci.yml@main
    with:
      package-manager: 'bun'
```

```yaml
# .github/workflows/publish.yml
name: Publish

on:
  release:
    types: [published]

jobs:
  publish:
    uses: jarero321/shared-workflows/.github/workflows/npm-publish.yml@main
    with:
      package-manager: 'bun'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

## License

MIT

---

<div align="center">

**[Report Bug](https://github.com/jarero321/shared-workflows/issues)** · **[Request Feature](https://github.com/jarero321/shared-workflows/issues)**

</div>
