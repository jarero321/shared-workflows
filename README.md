<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=0,2,5,30&height=180&section=header&text=shared-workflows&fontSize=36&fontColor=fff&animation=fadeIn&fontAlignY=32" />

<div align="center">

![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![Workflows](https://img.shields.io/badge/workflows-3-7c3aed?style=for-the-badge)
![License](https://img.shields.io/github/license/jarero321/shared-workflows?style=for-the-badge)

**Reusable GitHub Actions workflows for CI/CD pipelines. One config, every repo.**

[Node CI](#node-ciyml) •
[Go CI](#go-ciyml) •
[Release & Publish](#release-and-publishyml) •
[Usage](#usage-example)

</div>

---

## Features

| Feature | Description |
|:--------|:------------|
| **Node.js CI** | Lint, typecheck, test, and build with npm, pnpm, or bun |
| **Go CI** | Lint, test with race detection, and build Go projects |
| **Release & Publish** | Auto-create GitHub releases and publish to npm |
| **Multi Package Manager** | First-class support for npm, pnpm, and bun |
| **Fully Configurable** | Every step is optional and every command is customizable |
| **Composite Action** | Reusable `node-setup` action for consistent Node.js environments |

## Tech Stack

<div align="center">

**Powered By**

<img src="https://skillicons.dev/icons?i=githubactions,nodejs,go,npm&perline=8" alt="tech" />

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
|:------|:--------|:------------|
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
|:------|:--------|:------------|
| `go-version` | `'1.23'` | Go version |
| `run-lint` | `true` | Run golangci-lint |
| `run-tests` | `true` | Run tests with race detection |
| `run-build` | `true` | Run build |
| `main-path` | `'./cmd/server'` | Path to main.go |
| `build-output` | `'./bin/app'` | Build output path |

---

### `release-and-publish.yml`

Create GitHub releases and publish packages to npm registry.

```yaml
jobs:
  publish:
    uses: jarero321/shared-workflows/.github/workflows/release-and-publish.yml@main
    with:
      package-manager: 'bun'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

| Input | Default | Description |
|:------|:--------|:------------|
| `package-path` | `'package.json'` | Path to package.json |
| `node-version` | `'20'` | Node.js version |
| `package-manager` | `'npm'` | npm, pnpm, or bun |
| `build-command` | `'npm run build'` | Custom build command |

| Secret | Required | Description |
|:-------|:---------|:------------|
| `NPM_TOKEN` | Yes | npm authentication token |

**How it works:**
1. Reads version from `package.json`
2. Checks if git tag already exists
3. Creates GitHub Release with auto-generated notes (if new version)
4. Builds and publishes to npm with public access

---

## Node Setup Action

Composite action used internally by the workflows. Available for direct use:

```yaml
steps:
  - uses: jarero321/shared-workflows/.github/actions/node-setup@main
    with:
      node-version: '20'
      package-manager: 'bun'
      install: 'true'
      build: 'false'
```

| Input | Default | Description |
|:------|:--------|:------------|
| `node-version` | `'20'` | Node.js version |
| `package-manager` | `'npm'` | npm, pnpm, or bun |
| `install` | `'true'` | Run dependency install |
| `build` | `'false'` | Run build step |
| `build-command` | `'npm run build'` | Custom build command |
| `registry-url` | `''` | NPM registry URL (for publishing) |

---

## Usage Example

### Full CI + Release on Push

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
      test-command: 'bun run test:run'
      build-command: 'bun run build'
```

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    branches: [main]

jobs:
  publish:
    uses: jarero321/shared-workflows/.github/workflows/release-and-publish.yml@main
    with:
      package-manager: 'bun'
      build-command: 'bun run build'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

## Structure

```
shared-workflows/
├── .github/
│   ├── actions/
│   │   └── node-setup/
│   │       └── action.yml       # Composite Node.js setup action
│   └── workflows/
│       ├── node-ci.yml          # Node.js CI workflow
│       ├── go-ci.yml            # Go CI workflow
│       └── release-and-publish.yml  # Release + npm publish
└── README.md
```

---

## Used By

<table>
<tr>
  <td width="50%">
    <h3 align="center">cli-builder</h3>
    <div align="center">
      <p>Shared CLI components and utilities</p>
      <a href="https://github.com/jarero321/cli-builder">
        <img src="https://img.shields.io/badge/CODE-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="code" />
      </a>
      <a href="https://www.npmjs.com/package/@cjarero183006/cli-builder">
        <img src="https://img.shields.io/npm/v/@cjarero183006/cli-builder?style=for-the-badge&color=CB3837&logo=npm&logoColor=white" alt="npm" />
      </a>
    </div>
  </td>
  <td width="50%">
    <h3 align="center">claude-skills</h3>
    <div align="center">
      <p>Claude Code skills manager CLI</p>
      <a href="https://github.com/jarero321/claude-skills">
        <img src="https://img.shields.io/badge/CODE-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="code" />
      </a>
      <a href="https://www.npmjs.com/package/@cjarero183006/claude-skills">
        <img src="https://img.shields.io/npm/v/@cjarero183006/claude-skills?style=for-the-badge&color=CB3837&logo=npm&logoColor=white" alt="npm" />
      </a>
    </div>
  </td>
</tr>
<tr>
  <td width="50%">
    <h3 align="center">create-clean-app</h3>
    <div align="center">
      <p>Clean Architecture scaffolder</p>
      <a href="https://github.com/jarero321/create-clean-app">
        <img src="https://img.shields.io/badge/CODE-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="code" />
      </a>
      <a href="https://www.npmjs.com/package/@cjarero183006/create-clean-app">
        <img src="https://img.shields.io/npm/v/@cjarero183006/create-clean-app?style=for-the-badge&color=CB3837&logo=npm&logoColor=white" alt="npm" />
      </a>
    </div>
  </td>
  <td width="50%">
    <h3 align="center">mcp-repo-monitor</h3>
    <div align="center">
      <p>GitHub monitoring MCP server</p>
      <a href="https://github.com/jarero321/mcp-repo-monitor">
        <img src="https://img.shields.io/badge/CODE-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="code" />
      </a>
    </div>
  </td>
</tr>
</table>

---

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**[Report Bug](https://github.com/jarero321/shared-workflows/issues)** · **[Request Feature](https://github.com/jarero321/shared-workflows/issues)**

</div>

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=0,2,5,30&height=120&section=footer" />
