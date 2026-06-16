---
name: superminds-ba-skill
description: Use when PM/PO/BA needs to draft, rewrite, or review product requirement docs, SRS/PRD/specs, or the next-step HTML flow artifact for a Superminds product feature. The skill enforces discovery across existing docs, conflict checks, source-code-as-reference-only, UI implementation links from design, mandatory event tracking, and a lightweight core-plus-conditional spec format. Trigger phrases include viết spec, viết PRD/SRS, rewrite doc, feature doc, BA agent, product agent, UI need to be implemented, compare docs, conflict check, event tracking.
---

# Superminds BA Skill

## Premises

- Work from the current repository root unless the user provides another path.
- Existing product docs can live in `specs/`, `docs/`, `confluence/pages/`, or any repo-specific docs folder. Discover the real folders with `rg --files`, `find`, and user-provided paths.
- Default output: follow the repo's existing convention. If no convention exists, use `specs/<feature-id>.md` and create a sibling HTML artifact only when requested by the workflow/user.
- Source code is implementation reference only. It helps the agent understand current behavior, naming, events, API contracts, and constraints; it is not a source for deciding product scope or recommending what the product should do.
- Product scope comes from PM/PO input, existing PRD/specs, design links/components, explicit decisions, and approved business rules.
- Design can be ahead of development. The spec must list UI that needs implementation through Figma/component links instead of assuming every screen exists in docs or code.

## Workflow

### 1. Discovery

Ask only for information that cannot be discovered locally:

1. Feature name / feature id.
2. Request type: new spec, rewrite, review, or HTML flow artifact.
3. Source links: Figma components/screens, PRD/spec links, Confluence/docs links, tickets, or existing files.
4. Optional code paths only when the user knows them.

Then discover context yourself:

1. Search current docs folders for related feature names, surfaces, shared screens, events, credits, refunds, paywalls, APIs, remote config keys, and navigation.
2. Search existing docs broadly enough to catch conflicts. Do not rely on one feature folder only.
3. Derive search terms from the requested feature and candidate contract names before finalizing analytics, refund, credit, paywall, API, remote config, or navigation contracts. Avoid hardcoded project-specific keyword lists.
4. Resolve source-code paths from repo structure, env vars, config files, package manifests, or user-provided paths. If code cannot be found, continue with docs/design and mark code reference as unavailable.
5. Print a short discovery summary: docs checked, design refs found, code refs found, and missing inputs.

### 2. Synthesize

Before drafting, write a compact internal synthesis:

- Product intent and user outcome.
- Related docs and whether they agree or conflict.
- Design/UI components that must be implemented.
- Current implementation behavior, only as reference.
- Contract candidates: events, params, credits, refunds, paywalls, APIs, remote config, navigation.
- Gaps that cannot be answered from available sources.

Conflict rule:

- Conflict detection is the agent's job, not a section to outsource to the doc.
- If another doc already defines a contract, preserve the existing contract unless user/product input explicitly changes it.
- If sources conflict, do not invent a quiet resolution. Put only truly unresolved items in Open Questions and state which sources disagree.

### 3. Debate

Run a short adversarial review before writing:

- A. Product value: what user/business outcome does this serve?
- B. Scope: what is in, out, and deferred?
- C. UI completeness: are all design components/screens listed with links?
- D. State coverage: empty, loading, success, failure, retry, permission, quota, purchase, refund, edge states.
- E. Contract consistency: existing events, params, APIs, remote config, credit/refund rules, naming.
- F. Cross-doc conflict: does this feature touch a screen or flow also modified by another doc?
- G. Source-code reference: what behavior exists today, and what should not be treated as product scope?
- H. Analytics: mandatory event tracking table with purpose, trigger, params, and consistency check.
- I. Acceptance: what can QA/dev verify without guessing?
- J. HTML flow: if an HTML artifact exists or is requested, does the markdown link to it relatively?

Only unresolved, decision-blocking items belong in Open Questions. Normal product choices should be decided in the flow/behavior section.

### 4. Draft Spec

Use [TEMPLATE.md](TEMPLATE.md). Keep the spec small by default:

Core sections:

1. Frontmatter
2. Goal
3. UI Need To Be Implemented
4. Scope & Entry Points
5. Inputs, Outputs & States
6. Flow / Behavior
7. Critical Invariants
8. Event Tracking
9. Open Questions
10. References

Conditional sections, include only when useful:

- User Stories
- Acceptance Criteria
- Failure Modes & Fallback UX
- Remote Config Keys
- Cost, Credit & Refund
- Out Of Scope

### 5. Review

Before delivering:

- Verify every Figma/design item has a link or an explicit missing-link note.
- Verify every mandatory event has purpose, trigger, params, and naming consistency with existing docs/code.
- Verify related existing docs were compared for shared screens/flows/contracts.
- Verify source-code references are phrased as implementation context, not product recommendations.
- Verify Open Questions contain only questions that cannot be answered from available sources.
- Verify all links intended for git are relative or normal web URLs. Do not write machine-local paths, `file://`, `localhost`, or repo-root absolute paths into the doc.

### 6. HTML Flow Artifact

When generating the next-step HTML artifact:

- Put the HTML file next to the markdown spec with the same basename, for example `specs/<feature-id>.md` and `specs/<feature-id>.html`.
- Link from markdown with a relative path such as `./<feature-id>.html`.
- The HTML is a visual review artifact, not the source of truth. The markdown owns requirements.
- Keep HTML single-file unless the repo has a clear artifact convention.
- Mermaid or CDN dependencies are acceptable for review artifacts, but avoid private/local absolute paths.
- The Flow / Behavior section should link to the HTML when it exists.

## Event Tracking Contract

Event tracking is mandatory for every feature spec. If no new events are needed, say so explicitly and still explain which existing events cover the flow.

Each event row must include:

- Event name
- Purpose
- Trigger
- Parameters
- Existing reference or consistency note

When a draft proposes a new event, compare against existing docs/code first. Prefer existing event names and params when they already cover the behavior.

## Output Style

- Write product docs in the user's preferred language.
- Be decisive when available sources are enough.
- Keep docs implementation-ready, not essay-like.
- Use relative file links for repo artifacts.
- Never hardcode one developer's machine path.
