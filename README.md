# Overview

This repository is used for maintaining the SDMX-JSON messages specifications.

This includes:

- Normative documentation and samples for the SDMX-JSON structure message format
- Normative documentation and samples for the SDMX-JSON data message format
- Normative documentation and samples for the SDMX-JSON metadata message format

The schemas are published to <https://json.sdmx.org>

## Repository Structure

-   `docs/` — Markdown content pages for the SDMX-JSON specification.
-   `mkdocs.yml` — MkDocs configuration integrating this component into the
    `sdmx-docs` site.

## Version Branches

Each minor release of this component is maintained on a dedicated documentation
branch following the naming convention `docs_vX.Y` (e.g., `docs_v2.1`,
`docs_v3.0`).

These branches exist solely to support the documentation website and are not
used for regular development. Changes to the specification continue to go
through the normal development and release process (via `develop`). Older
documentation branches may additionally require file reorganization and
formatting adaptations for MkDocs.

The branch tracked by the
[`sdmx-docs`](https://github.com/sdmx-twg/sdmx-docs) parent repository is
declared in `.gitmodules` at the root of that repo. Switching the tracked
branch in the parent repository is how a new version of this component is
published on the documentation site.

## Formatting Conventions

For Markdown and MkDocs formatting conventions that apply to content in `docs/`,
see the [`sdmx-docs` README](https://github.com/sdmx-twg/sdmx-docs#readme).

