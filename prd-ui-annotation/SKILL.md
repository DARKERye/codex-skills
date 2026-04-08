---
name: prd-ui-annotation
description: Use this skill when the user asks to annotate UI pages from a PRD, initialize requirement badges/tooltips, update existing annotations after PRD or UI changes, map PRD requirements to UI modules, or write requirement IDs back into PRD documents.
---

# PRD UI Annotation

## Purpose

Use this skill to turn PRD requirements into non-invasive UI annotations: numbered badges on page modules plus detailed draggable tooltips that can replace reading the original PRD for development handoff.

You act as a rigorous product manager and frontend engineering expert. Preserve requirement detail, map related requirements to the right UI module, and keep annotation behavior stable across initialization and incremental updates.

## Required Reference

Before modifying code or PRD files, read `references/annotation-spec.md`. It contains the exact badge styling, tooltip layout, positioning rules, update rules, and self-check list.

## Workflow Decision

First determine the user's intent.

- If the request is ambiguous, do not annotate yet. Ask: "请问您是需要执行【Workflow A: 初始化标注】（针对新页面/新文档），还是执行【Workflow B: 标注内容更新】（针对已有标注的增量修改）？"
- If the user provides a new PRD and page implementation or asks to start annotation, run Workflow A.
- If the user provides an adjusted PRD/page or asks to update existing labels, run Workflow B.

## Workflow A: Initialization

Use this for a new page or a PRD that has not yet been annotated.

1. Read the PRD and page implementation.
2. Group requirements by functional UI module before adding any badge. A closely related component/module must receive only one badge.
3. Preserve all original requirement detail inside the tooltip for that module: original descriptions, business logic, preconditions, edge cases, exceptions, and notes.
4. Add numbered badges using the exact styles from `references/annotation-spec.md`.
5. Add hover-triggered, draggable tooltips using the exact interaction rules from the reference.
6. Write the generated numeric requirement IDs back into the PRD at the corresponding requirement description start, using `[1]`, `[2]`, etc.
7. Verify badge numbers and PRD numbers match one-to-one.

## Workflow B: Incremental Update

Use this for an already annotated page/PRD.

1. Compare the current annotations with the updated PRD/page.
2. Classify changes as added, modified, or deleted requirements.
3. Keep all visual style parameters locked unless the target component itself moved.
4. For modified items, update only the affected tooltip Markdown content.
5. For deleted items, remove the corresponding badge and tooltip.
6. For added items, create new badges/tooltips using the established style and continuous numbering.
7. Re-run the self-check list from `references/annotation-spec.md`.

## Grouping Heuristics

- Row actions such as edit, delete, view, and permission controls belong under one annotation on the table action area.
- Filters such as inputs, selects, query, and reset controls belong under one annotation on the filter module.
- Tabs and container switching logic belong under one annotation on the overall Tabs/container region.
- Do not split a single functional module into multiple badges unless the UI regions are genuinely independent.
