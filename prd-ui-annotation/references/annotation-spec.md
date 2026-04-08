# PRD UI Annotation Specification

## Core Requirement

Deeply parse the PRD and mount every requirement onto the UI as module-level annotations. The developer should be able to read the tooltip and avoid opening the original PRD for clarification.

## Modular Aggregation

- Normalize components: never add multiple badges to the same component or tightly related module.
- Before annotating, group PRD requirements by functional area and merge each group into one requirement number and one tooltip.
- Preserve every detail after grouping. Tooltips must include all source descriptions, business logic, preconditions, abnormal flows, and notes. Do not summarize away details.

Examples:

- Row operations: merge edit, delete, view, and permission-control requirements; annotate the table "操作" header or action area.
- Filter module: merge inputs, selects, query button, and reset button requirements; annotate the filter module's top-right corner.
- Tabs/container: merge tab-switching requirements; annotate the overall Tabs area.

## Tooltip Information Architecture

- Header: requirement number plus `需求描述：[模块名称]`.
- Add a light gray divider under the title bar.
- Requirement number style must exactly match the badge style.
- Preserve the original Markdown structure:
  - Line-height: `1.6`.
  - Paragraph margin-bottom: `12px`.
  - Preserve bold and italic emphasis one-to-one.
  - Preserve nested unordered and ordered list indentation.
  - Render blockquotes with a light gray left border.
- Use "小标题：内容" sections where useful, covering display style, interaction and sorting, business definition, and notes.
- When content mentions state colors such as green or red, add a matching colored dot before the text.

## Badge Positioning and Layout

- Use `position: absolute`.
- Do not affect the original page DOM layout, width, height, or spacing.
- Place badges at the module's top-right area: `top: -8px; right: -4px;`.
- Ensure badges are not blocked by page elements.
- If the target component has `overflow: hidden`, mount the badge to `body` and calculate global coordinates.
- Tooltip `z-index` must be `9999`.
- The tooltip defaults to the badge's lower-left side with `8px` spacing.
- If the tooltip would overflow the viewport, automatically flip or adjust its position.

## Visual and Interaction Rules

Badge style:

```css
display: inline-block;
vertical-align: top;
background: rgb(250, 173, 20);
color: #fff;
font-size: 10px;
font-weight: 700;
line-height: 14px;
padding: 0 4px;
border-radius: 2px;
border: 0;
cursor: pointer;
```

- Badge numbers use plain numeric values from `1` to `999`.

Tooltip style:

```css
background: #f0efef;
border-radius: 4px;
width: 450px;
z-index: 9999;
```

- Add a subtle diffuse shadow.
- Add an `X` close button in the upper-right corner.
- Hovering a badge opens the tooltip immediately.
- The same requirement number can have only one tooltip open at once.
- Different requirement numbers may have tooltips open at the same time.
- Clicking or dragging inside the tooltip must stop event bubbling and must not trigger page events below it.
- The whole tooltip must be freely draggable with the mouse.
- The tooltip may only be closed by clicking its upper-right `X` close button.

## Bidirectional Traceability

- Write generated requirement numbers back into the PRD at the corresponding requirement description start.
- Use `[1]`, `[2]`, etc. in the PRD.
- Ensure page annotation numbers and PRD numbers match exactly.

## Incremental Update Rules

- Identify added, modified, and deleted requirements by comparing current annotations against the updated PRD/page.
- Style lock: do not change any visual style parameters during updates. Keep badge color, size, tooltip background, offsets, and interaction behavior unchanged.
- For modified requirements, replace only the affected tooltip Markdown text.
- For deleted requirements, remove the corresponding badge and tooltip.
- For added requirements, generate a new number, badge, and tooltip using the existing style and continuous numbering.
- Do not change badge position unless the target component moved.

## Self-Check

After implementation, verify and fix:

- Did I run initialization or update according to the user's request?
- Does each component/module have at most one badge?
- Is the tooltip detailed enough to replace the PRD for the annotated module?
- Are any requirements missing?
- Does the tooltip support dragging?
- Can it only be closed through the `X` button?
- Does clicking or dragging the tooltip isolate events from the page below?
- Does the badge follow the `10px` bold style and all color/spacing rules?
- Are z-index and visibility correct?
- Does the tooltip preserve Markdown hierarchy and emphasis?
- During updates, did style parameters stay 100% unchanged?
- Were requirement numbers correctly written back to the PRD?
