# Editor UX

## Layout

Use a stable, preview-first desktop shell with two unmistakable regions: the left side is entirely the editor and the right side is entirely the live preview. Keep the left editor only as wide as its actual controls require and let the right preview consume the remaining space; avoid arbitrary fixed widths that make the editor dominate on large displays.

Fill the available application height below persistent chrome. Give the editor and preview separate bounded scroll containers with a valid shrinking height chain; ensure nested grid or flex children can shrink and scroll instead of being clipped. If preview uses an iframe, size it to its container and make the document itself reachable through preview scrolling.

Do not put both panes inside one shared vertical scrolling surface, mirror their scroll offsets, or let programmatic scrolling in one pane move the other pane. Never hide overflowing preview content without an accessible scroll path.

Organize navigation from broad to specific:

```text
global settings -> page -> section -> collection item -> field
```

Do not show the entire site schema as one long form. Preserve context by displaying the current page, section, and selected item.

Keep the interface visually clean, direct, and sparse. Do not place editing tools or persistence actions above the preview. Put one primary `Apply` button in a fixed or sticky footer at the bottom of the left editor pane. Do not show draft-save, publish, undo, or redo buttons.

## Structure-aware editor representation

Make the editing surface a simplified structural counterpart of the public component, not merely a stack of unrelated form controls. Preserve relationships that help an operator recognize where content appears:

- represent horizontal lists as horizontal flows or compact horizontal items when space allows;
- represent vertical lists as one-column vertical rows in their public order;
- represent grids with a corresponding grid and the same recognizable column, ordering, featured-item, grid-area, and span relationships;
- preserve grouping and nesting for split layouts, cards, tabs, accordions, carousels, and repeated sections;
- keep images, titles, labels, and summaries in positions that make items visually identifiable;
- base the editor representation on the public layout at the configured desktop preview width.

Match the collection primitive before optimizing editor density:

| Public preview structure | Editor representation |
| --- | --- |
| vertical list | one-column vertical rows |
| horizontal list | horizontal flow or horizontally oriented items |
| regular grid | corresponding columns and visual order when space permits |
| asymmetric or featured grid | matching featured item, row or column spans, and visual grouping |
| table | row-and-column representation |
| tabs | tab-aware grouped controls |
| accordion | collapsible grouped controls |
| carousel | ordered slide-aware items |

Do not convert a vertical public list into a multi-column editor card grid merely to save space. Likewise, do not flatten a public grid into an unrelated list or equal-card grid when position, size, or emphasis identifies items.

For example, when the public composition shows item 1 as a full-width or featured block and items 2 and 3 as a paired row, the editor must show the same `1–2` composition rather than three equal columns. Preserve this mapping while editing and reordering.

When the compact editor cannot show public components at full size, use compact item representations rather than changing the layout type. Preserve the same columns, spans, grouping, emphasis, and source order, and keep each item's controls clearly associated with its visual position.

Do not reproduce decorative styling at the expense of editing clarity, but make the editing arrangement feel nearly identical to the preview's meaningful structure. Preserve layout type, direction, order, grouping, hierarchy, relative position, and column behavior while keeping labels, controls, focus, and validation accessible.

## Field behavior

- Provide concise labels and help only where ambiguity exists.
- Size controls for expected content; multiline fields must be visibly multiline.
- Preserve intentional line breaks in preview and publication.
- Show existing media before upload controls.
- Validate near the field and retain the user's input after errors.
- Prefer safe presets over unrestricted visual controls.
- Keep keyboard focus visible and labels programmatically associated.
- On pointer click or keyboard focus, select the mapped preview field immediately. Change route or enclosing component state as needed, scroll only the preview to reveal it, and highlight it without waiting for content changes.
- Trigger preview reveal once when the focused target changes. Typing within the same field must update content without repeatedly scrolling the preview or stealing editor focus.

## Collections

Represent repeated content with meaningful cards or rows using an image, title, and short secondary value when available.

Support:

- selecting an item without losing its identity after reorder;
- adding and immediately focusing the new item;
- editing in a focused detail area;
- deletion with confirmation or recovery proportional to risk;
- drag-and-drop ordering plus keyboard-accessible move controls;
- empty states that explain how to add the first item;
- disabled or explanatory states when automatic sorting overrides manual order.

Avoid using array indices as persistent keys.

## Reordering interactions

- Start pointer dragging from a dedicated, clearly labeled handle rather than the whole editable card. Preserve text selection, field input, links, and buttons inside the item.
- Change the handle cursor from `grab` to `grabbing` and give it a clear pointer target.
- Keep a size-preserving placeholder at the source and show a compact, identifiable drag overlay containing useful item context such as its image, title, or number.
- Show the exact destination before drop: an insertion line for lists, a target cell for grids, and an explicit empty drop zone for empty collections. Mark invalid destinations clearly.
- Follow the public layout's movement model: vertical lists move vertically, horizontal lists horizontally, and grids by visual cell order. Do not allow cross-group moves unless the content model explicitly supports them.
- Auto-scroll only the editor's bounded scroll container when the pointer approaches its edge. Increase speed gradually, cap it, and never move the outer page or preview pane as a side effect of dragging.
- Reflect the provisional order in the preview without persistently saving every pointer movement. Keep a visible preview association for the dragged item without forcing preview scroll when the affected region is already visible.
- Restore the original order on `Escape`, invalid drop, or cancellation; retain the selected item by stable ID.
- After drop, move focus back to the item's handle, announce its new position, and animate items briefly without excessive motion.
- Provide keyboard-accessible pick up, move, drop, and cancel behavior or explicit move controls with equivalent outcomes. Announce position and valid destinations to assistive technology.

## Image and identity assets

- Show the current asset, usage context, recommended dimensions or aspect ratio, and supported formats before replacement controls.
- Provide upload and replacement for operator-managed images; provide removal or restore-default only when the consuming component has a safe empty or fallback state.
- Expose alternative text for meaningful content images. Mark decorative images explicitly and avoid misleading alt-text controls for CSS decoration when the public implementation cannot use them.
- Group responsive sources, crops, light/dark variants, or icon sizes as one understandable asset set instead of unrelated file inputs.
- Always place a favicon field under recognizable global site or brand settings. Show upload or add controls when none exists and replacement or restoration controls when one does.
- Place logo, touch icon, manifest icons, and default social image under the same global settings when applicable.
- Preview content and background image changes in their real public components. For browser-level assets such as favicons, provide a local preview and clearly indicate when a reload or metadata refresh is required after applying.
- Explain why a discovered asset is read-only or excluded when an operator could reasonably expect to edit it.

## Preview

- Render real public components and styles.
- Update promptly as working state changes.
- Render a stable desktop preview at the configured representative width.
- Use all available shell height and keep the full preview document reachable through its own scroll container. Empty space is acceptable; inaccessible clipped content is not.
- Give each editable preview target a stable mapping to its editor target; do not depend on array indexes or visible text for identity. Support mappings at page, section, component, collection-item, and field level.
- Map individual editable text, rich text, media, labels, buttons, links, and nested values whenever they have distinct editor controls. Prefer the deepest mapped target under the pointer; fall back through item, component, and section ancestors only when no finer target exists.
- Let clicking an editable preview target keep the preview's current scroll position while selecting, expanding, and—only when needed—scrolling the corresponding editor control into view.
- When editor selection changes, keep the editor's current scroll position while navigating the preview to the required route, activating enclosing tabs, accordions, or carousel state as needed, scrolling the preview target into view, and applying a visible selection outline or overlay.
- Store the initiating pane or event origin with selection commands. Apply reveal and scroll effects only to the opposite pane, and consume reflected selection updates without triggering navigation or scrolling again.
- Distinguish preview hover from persistent selection. Keep highlights aligned after preview-container and content resizing.
- Use scroll margins or equivalent positioning so sticky headers and editor chrome do not obscure the selected target.
- Preserve selection across content edits and collection reorder by stable ID. Clear or redirect selection predictably when the selected target is deleted.
- During reorder, render provisional order changes promptly while keeping preview scrolling independent. On cancel, restore the prior preview order; on drop, retain and highlight the moved item.
- Avoid stealing text-entry focus, moving the initiating pane, coupling scroll positions, or creating event and scroll loops when editor and preview notify each other of the same selection.
- Provide a non-pointer path from the editor to every target; preview click-to-edit must complement rather than replace accessible editor navigation.
- Contain preview links and destructive interactions so operators do not accidentally leave the editor or trigger real actions.

## Apply and feedback

Preview changes immediately, but keep them in working state until the operator selects `Apply`. Make unchanged, changed, applying, applied, and failed states distinct through the single bottom action and concise adjacent feedback.

Disable `Apply` when nothing changed, prevent duplicate submission while applying, and retain failed changes for retry. Warn before closing or navigating away with unapplied changes. Do not add separate draft-save, publish, undo, or redo buttons.

## Global styles

Always show a recognizable global settings group with primary color, typography or font-family, and favicon controls. Additional safe options may include logo, default social image, density, button style, or card radius when the site supports them.

Bind primary color and typography controls to the site's existing shared tokens so every intended color and text consumer changes consistently across the entire site. Offer the site's loaded font families or a small validated preset list; do not accept arbitrary font URLs, arbitrary CSS, or choices the public rendering path cannot load. Show every global effect immediately throughout the preview when the browser surface permits it.

## Destructive and high-impact actions

Confirm permanent deletion and broad style resets when their impact warrants it. Keep production writes behind the single explicit `Apply` action and never disguise them as preview controls.
