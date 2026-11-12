# Contributing

This repository contains the ISA-ARC Workspace, ISA-XLSX and WR-XLSX specifications. Contributions target `dev`; `release` remains the publication branch.

## Local preview

The Jekyll setup matches the [ARC RO-Crate profile repository](https://github.com/nfdi4plants/arc-ro-crate-profile/tree/1e5ffde5136ffdc62ca7aa86b58171062ff522d0): the Slate theme, GitHub Pages dependencies, Mermaid support and a Jekyll devcontainer.

Open this repository in VS Code and select **Dev Containers: Reopen in Container**. The container installs the gems in `docs/Gemfile` and starts the site with live reload. Open <http://localhost:4000/>.

When symlinks are created inside the container on a Windows bind mount, use the container for editing and Git operations as well. Native Windows tools may not resolve those Linux-created links. The ordinary source files remain accessible under the versioned directory.

With Ruby and Bundler installed locally, the equivalent commands are:

```sh
cd docs
bundle install
bundle exec jekyll serve --baseurl '' --livereload
```

The local preview overrides `baseurl` to serve at `/`; published builds keep `/ISA-ARC-Workspace/` from `docs/_config.yml`. If an already-running preview returns `Not Found` at `/`, stop it with Ctrl+C and restart it with the command above.

For a build without the preview server:

```sh
cd docs
bundle exec jekyll build
```

Generated files in `docs/_site/`, Bundler caches and `docs/Gemfile.lock` are ignored, as in the reference setup. The `github-pages` gem determines the compatible dependency versions.

## Specification sources and versions

The source documents live in a versioned collection under `docs/specifications/`:

```text
docs/specifications/
  3.0.0-draft.2/
    index.md
    ISA-ARC-Workspace.md
    ISA-XLSX.md
    WR-XLSX.md
  index.md                 -> 3.0.0-draft.2/index.md
  ISA-ARC-Workspace.md      -> 3.0.0-draft.2/ISA-ARC-Workspace.md
  ISA-XLSX.md               -> 3.0.0-draft.2/ISA-XLSX.md
  WR-XLSX.md                -> 3.0.0-draft.2/WR-XLSX.md
```

The three specification files at the repository root are also symlinks, pointing to their stable paths in `docs/specifications/`. Existing root paths remain usable for editing. Edit the versioned sources or follow these links; do not replace a symlink with a separate copy.

The directory version identifies the workspace collection. The documents retain their own editions: Workspace `3.0.0-draft.2`, ISA-XLSX `3.0.0-draft.1`, and WR-XLSX `1.0.0-draft.1` in the current collection. Relative links keep the three documents together within either the selected version or the latest collection.

To prepare another collection:

1. Copy the complete current version directory to a new version directory.
2. Update document versions and dependency references where appropriate, and update that collection's `index.md`.
3. Add the collection to `docs/index.md`.
4. Update all four stable links directly under `docs/specifications/` together. Their targets must refer to the same collection directory. The root aliases do not need to change.
5. Build the site and check the versioned and stable pages before committing. Preserve earlier published version directories.

This is the same checked-in version-folder and latest-symlink model as the reference repository. Versions and aliases are updated explicitly; there is no automated version bump or tag-generated documentation.

## Updating aliases in the devcontainer

After preparing the complete new collection, run this from the repository root in the container. Replace the example version with the actual collection version:

```sh
(
  set -eu
  collection_version='3.0.0-draft.3'
  cd docs/specifications

  # Check every destination and existing alias before updating any links.
  for document in index.md ISA-ARC-Workspace.md ISA-XLSX.md WR-XLSX.md; do
    test -f "$collection_version/$document" || {
      printf 'Missing versioned document: %s\n' "$collection_version/$document"
      exit 1
    }
    test -L "$document" || {
      printf 'Expected a symbolic link: %s\n' "$document"
      exit 1
    }
  done

  for document in index.md ISA-ARC-Workspace.md ISA-XLSX.md WR-XLSX.md; do
    ln -sfn -- "$collection_version/$document" "$document"
  done

  git add -- "$collection_version" index.md ISA-ARC-Workspace.md ISA-XLSX.md WR-XLSX.md
  git ls-files -s -- index.md ISA-ARC-Workspace.md ISA-XLSX.md WR-XLSX.md
)
```

All four stable links must have Git mode `120000`.

## Symlinks on Windows

Enable Windows **Developer Mode** before cloning, then enable Git's symlink support from the first checkout:

```powershell
git clone -c core.symlinks=true https://github.com/nfdi4plants/ISA-ARC-Workspace.git
cd ISA-ARC-Workspace
Get-Item ISA-ARC-Workspace.md, docs\specifications\ISA-ARC-Workspace.md |
    Select-Object Name, LinkType, Target
```

Both entries should show `SymbolicLink`. In an existing checkout, `git config core.symlinks true` enables future symlink handling but does not repair links already checked out as plain text files. Preserve local work before repairing or replacing such a checkout.

To advance the stable links, run this from the repository root after creating the complete new version directory. Replace the example version with the actual collection version:

```powershell
$collectionVersion = '3.0.0-draft.3'
$specificationDirectory = (Resolve-Path -LiteralPath 'docs/specifications').Path
$documentNames = @('index.md', 'ISA-ARC-Workspace.md', 'ISA-XLSX.md', 'WR-XLSX.md')

# Verify every destination and existing alias before changing any links.
foreach ($documentName in $documentNames) {
    $destination = Join-Path $specificationDirectory "$collectionVersion/$documentName"
    if (-not (Test-Path -LiteralPath $destination -PathType Leaf)) {
        throw "Missing versioned document: $destination"
    }
    $alias = Get-Item -LiteralPath (Join-Path $specificationDirectory $documentName)
    if ($alias.LinkType -ne 'SymbolicLink') {
        throw "Expected a symbolic link: $($alias.FullName)"
    }
}

Push-Location $specificationDirectory
try {
    foreach ($documentName in $documentNames) {
        Remove-Item -LiteralPath $documentName
        New-Item -ItemType SymbolicLink -Path $documentName -Target "$collectionVersion/$documentName"
    }
} finally {
    Pop-Location
}

git add docs/specifications
git ls-files -s docs/specifications
```

The stable links must have Git mode `120000`; the actual versioned Markdown documents must remain ordinary files.

## GitHub Pages workflow

`.github/workflows/jekyll-gh-pages.yml` builds `docs/` using the same GitHub Pages actions as the reference repository. It runs on pushes to `release` or a manual workflow dispatch. Deployment requires the repository's Pages source to be **GitHub Actions**.

The configured site path is `/ISA-ARC-Workspace/`. The workflow publishes the site; it does not change w3id or arc-rdm.org redirects. Those destinations are managed separately.
