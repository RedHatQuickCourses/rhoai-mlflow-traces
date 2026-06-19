# Project Context

This directory contains a quick course, which provides technical enablement about Red Hat products for for IT professionals (developers/sysadmins).

A quick course book is a static HTML web site rendered from asciidoc files using Antora.

## Critical Constraints

**MUST initialize course project before adding any content**
- If there is a `course-init.sh` file, offer to initialize the project using the initialize-course-project skill.

**NEVER modify**:
- `antora-playbook.yml`
- `*.sh` scripts, `devfile.yaml`, `package.json`
- `ui-assets/`, `ui-bundle/`, `supplemental-ui/`, `templates/`

## Commands

- Render or build course book: `npm run build`
- Preview web server: `npm run serve`
- Open course book preview: `firefox http://127.0.0.1:8080`
- Watch mode: run both `npm run watch:adoc` AND `npm run serve` as background tasks.

## Content Structure

This is a standard Antora documentation project:

- `antora.yml`: course book structure (chapters) and main navigation, chapter ordering
- `modules/`: course content, each subdirectory is a chapter
   - `nav.adoc`: chapter navigation, section and page ordering
   - `pages/`: sections or pages of a chapter, one asciidoc file (`*.adoc`) per section
   - `images/`: optional sudirectory for images of a chapter
   - `attachments/` optional subdirectory for downloadable content of a chapter
   - `partials/` shared snippets for reuse by multiple sections

## Hints

- Read the [AsciiDoc reference](USAGEGUIDE.adoc) when needed.
- Read files in the [templates directory](templates/) for examples of different kinds of section.
- Use Antora macros over plain asciidoc syntax.

