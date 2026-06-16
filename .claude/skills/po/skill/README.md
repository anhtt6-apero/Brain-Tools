# Superminds BA Skill

Standalone BA/product-agent skill for drafting and reviewing feature specs, PRD/SRS documents, and HTML flow artifacts.

## Structure

- `SKILL.md`: skill instructions and workflow.
- `TEMPLATE.md`: default feature spec template.
- `specs/`: generated or test feature specs.
- `specs/rewrite/`: rewritten drafts used to test or compare skill behavior.

## Usage

Use this skill when a PM/PO/BA request needs:

- Product requirement docs or feature specs.
- UI implementation lists linked to design components.
- Cross-doc conflict checks.
- Source-code-as-reference-only analysis.
- Mandatory event tracking review.
- Optional HTML flow artifacts linked with relative paths.

Generated docs should use repo-relative links and avoid machine-local paths.
