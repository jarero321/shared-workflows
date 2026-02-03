# Shared Workflows

Reusable GitHub Actions workflows for CI/CD pipelines.

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

**Inputs:**
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

**Inputs:**
| Input | Default | Description |
|-------|---------|-------------|
| `node-version` | `'20'` | Node.js version |
| `package-manager` | `'npm'` | npm, pnpm, or bun |
| `build-command` | `'npm run build'` | Custom build command |

**Secrets:**
| Secret | Required | Description |
|--------|----------|-------------|
| `NPM_TOKEN` | Yes | npm authentication token |

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
