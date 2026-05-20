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

Each release of this component is maintained on a dedicated branch following the
pattern `X.Y.x` (e.g., `2.1.x`). The branch tracked by the
[`sdmx-docs`](https://github.com/sdmx-twg/sdmx-docs) parent repository is
declared in `.gitmodules` at the root of that repo. Switching the tracked branch
in the parent repository is how a new version of this component is published on
the documentation site.

## Formatting Conventions

For Markdown and MkDocs formatting conventions that apply to content in `docs/`,
see the [`sdmx-docs` README](https://github.com/sdmx-twg/sdmx-docs#readme).

