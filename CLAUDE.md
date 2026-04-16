# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

`pkl-jar` is a collection of [PKL](https://pkl-lang.org/) packages published to `pkg.pkl-lang.org`. Each top-level directory is a self-contained PKL package.

## Common Commands

```sh
# Evaluate a PKL module
pkl eval <module>.pkl

# Resolve project dependencies (run inside a package directory)
pkl project resolve

# Package a project for distribution (run inside a package directory)
pkl project package

# Run tests
pkl test <test-module>.pkl
```

## Architecture

### `basePklProject.pkl`

All package `PklProject` files amend `basePklProject.pkl`. It automatically derives the package name from the directory name (the last path segment relative to the root module), and fills in standard metadata fields:

- `baseUri` → `package://pkg.pkl-lang.org/github.com/tkoyasak/pkl-jar/<name>`
- `packageZipUrl` → points to the GitHub release ZIP
- `sourceCode` / `sourceCodeUrlScheme` → GitHub source links
- `licenseText` → reads `LICENSE` from the repo root

Each package's `PklProject` only needs to set `version` and any `dependencies`.

### Adding a New Package

1. Create a directory (e.g., `myPackage/`).
2. Add `myPackage/PklProject` that amends `"../basePklProject.pkl"` and sets `version`.
3. Add `myPackage/PklProject.deps.json` (run `pkl project resolve` to generate it).
4. Add the PKL module files.

### Package: `obsidian.webClipper`

Provides a type-safe PKL schema for [Obsidian Web Clipper](https://obsidian.md/clipper) templates. The main module is `Template.pkl`, which defines the `Template` and `Property` types and outputs JSON via `JsonRenderer`.
