<!--
Spec template for `superminds-ba-skill`.
Keep core sections small. Add conditional sections only when the feature needs them.
-->

---
feature: <feature-id>
version: 1.0.0
status: draft
last_updated: <YYYY-MM-DD>
owner: <PM/PO/BA>
platforms: [<platform>]
supersedes: []
design_refs: []
implementation_refs:
  - path: <relative/path>
    purpose: implementation reference only
remote_config: []
verified:
  date: <YYYY-MM-DD>
  sources_checked: []
---

# <Feature Name>

## 1. Goal

<Chốt mục tiêu user/business và kết quả mong muốn.>

## 2. UI Need To Be Implemented

Liệt kê các UI cần dev implement bằng link design/component cụ thể.

| UI / component | Design link | Notes |
| --- | --- | --- |
| <screen/component> | <figma/component link> | <what must be implemented> |

## 3. Scope & Entry Points

### In Scope

- <scope item>

### Entry Points

- <entry point>

## 4. Inputs, Outputs & States

### Inputs

- <input>

### Outputs

- <output>

### States

- Loading:
- Empty:
- Success:
- Failure:
- Retry:
- Permission / quota / purchase states, if applicable:

## 5. Flow / Behavior

HTML flow artifact, if available: `./<feature-id>.html`

1. <Step 1>
2. <Step 2>
3. <Step 3>

## 6. Critical Invariants

- <Rule that must never be violated>

## 7. Event Tracking

Event tracking is mandatory. If no new events are required, state which existing events cover this flow.

| Event | Purpose | Trigger | Params | Existing reference / consistency note |
| --- | --- | --- | --- | --- |
| <event_name> | <why this event exists> | <when fired> | <param list> | <existing doc/code ref or new-event rationale> |

## 8. Open Questions

Only include questions that cannot be answered from current docs, design, code reference, or product decisions.

- <question>

## 9. References

- Design:
- Existing docs:
- Source-code reference:

## Conditional: User Stories

- As a <user>, I want <goal>, so that <benefit>.

## Conditional: Acceptance Criteria

- Given <context>, when <action>, then <expected result>.

## Conditional: Failure Modes & Fallback UX

| Case | User impact | Expected behavior |
| --- | --- | --- |
| <failure> | <impact> | <fallback> |

## Conditional: Remote Config Keys

| Key | Purpose | Default | Notes |
| --- | --- | --- | --- |
| <key> | <purpose> | <default> | <notes> |

## Conditional: Cost, Credit & Refund

| Scenario | Credit/cost behavior | Event/ledger behavior | Notes |
| --- | --- | --- | --- |
| <scenario> | <behavior> | <event/ledger> | <notes> |

## Conditional: Out Of Scope

- <explicitly excluded item>
