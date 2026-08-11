# Validation checklist

## Architecture

- Public pages and editor preview use the same rendering components.
- Content types and stored data remain compatible.
- Collection items have stable unique IDs.
- Authentication protects routes and write operations.
- Secrets and operational settings are not exposed as content.

## Editing flows

- Load existing content.
- Edit each representative field type.
- Preserve multiline content and intentional empty values.
- Add, select, edit, reorder, and delete collection items.
- Confirm selection remains on the correct item after reorder.
- Reorder representative lists and grids with pointer and keyboard or equivalent move controls.
- Verify dragging starts from its handle without blocking text selection, input editing, links, or buttons.
- Verify the source placeholder, drag overlay, insertion line or target cell, invalid destination, and empty drop zone communicate the result before drop.
- Drag through a long collection and confirm only the editor pane auto-scrolls with controlled speed; the outer page and preview pane must not follow.
- Cancel with `Escape` and an invalid drop, then confirm the original order returns. Complete a drop and confirm focus, selection, position announcement, and applied order remain correct.
- Confirm provisional preview order appears without saving every pointer movement or forcing unnecessary preview scroll.
- Confirm editor collections preserve recognizable public direction, grouping, order, and grid relationships at representative widths.
- Confirm asymmetric layouts preserve featured items, grid areas, and row or column spans. Test a `1–2` composition and ensure the editor does not flatten it into three equal cards.
- Confirm public vertical lists remain one-column editor lists and are not converted into multi-column card grids for compactness.
- Confirm public horizontal lists, grids, tables, tabs, accordions, and carousels use the corresponding editor representation when present.
- In a compact editor pane, confirm the structural miniature retains the public columns, spans, grouping, emphasis, and source order instead of changing layout type.
- Verify complex structures such as split layouts, tabs, accordions, carousels, and nested groups remain understandable without flattening everything into a single form.
- Replace and remove media; test upload error behavior when applicable.
- Verify every operator-managed image in the discovery inventory is editable, or appears in the exclusion report with a valid reason.
- Test content, background, responsive, logo, favicon or app-icon, and social-sharing image flows when those asset classes exist.
- Confirm global settings always show favicon controls. Test both replacing an existing favicon and adding the first favicon when no asset or metadata entry exists.
- Confirm global settings always show primary-color and typography or font-family controls, even when discovery must introduce the smallest shared token.
- Confirm the left editor exposes only `Global settings` and `Page` as top-level choices, with the page selector visible in page mode.
- Confirm shared logos, header navigation labels, links, order, shared calls to action, and footer content appear once under the appropriate global group instead of being duplicated per page.
- Confirm page-only tabs and subnavigation remain in their page editor rather than being incorrectly promoted to global settings.
- Verify required dimensions, aspect ratios, variants, alternative text, safe removal, and restore-default behavior as applicable.
- Confirm there are no draft-save, publish, undo, or redo buttons.
- Verify the left editor has one bottom `Apply` action with unchanged, changed, applying, applied, and failed feedback.
- Verify navigation protection with unapplied changes.

## Preview and public output

- Compare the same working content in editor preview and public rendering after applying.
- Check the configured desktop preview width.
- Select representative pages, sections, fields, and collection items in the editor; confirm the preview changes route or component state as needed, scrolls the target into view, and highlights the correct element.
- Focus every representative text, multiline, link, media, and nested field without editing; confirm focus alone reveals and highlights its exact preview target. Continue typing and confirm the preview does not repeatedly scroll or steal focus.
- Click representative editable targets in the preview; confirm the editor reveals and selects the exact corresponding target.
- Click text, media, button, link, and nested collection fields; confirm the deepest editable target wins instead of selecting only its enclosing section.
- Select an editor target and confirm only the preview moves; click a preview target and confirm only the editor moves. Verify the initiating pane retains its scroll position in both directions.
- Confirm editor and preview use independent bounded scroll containers in the desktop editor shell.
- Confirm the left region is entirely the editor, the right region is entirely the preview, and editing or persistence controls do not intrude above or inside the preview.
- Confirm the shell fills the available height, the preview uses the remaining width and height, and long preview content remains reachable without outer-page scrolling or clipping.
- Verify hover and selected highlights remain aligned after preview-container changes, content resizing, and sticky-header scrolling.
- Confirm source-aware synchronization does not create reflected selection events, scroll loops, repeated navigation, coupled scroll offsets, or lost text-input focus.
- Reorder and delete selected collection items; confirm stable-ID selection behavior remains predictable.
- Verify long titles, long words, missing images, empty collections, and maximum realistic item counts.
- Ensure preview links and forms cannot accidentally trigger unintended production behavior.
- Change the primary color and confirm every intended site-wide consumer updates consistently in preview while unrelated colors remain unchanged.
- Change the typography or font-family preset and confirm every intended site-wide text consumer updates consistently, the chosen font actually loads, and unrelated icon or code fonts remain unchanged.
- Confirm the whole editor remains visually clean, direct, and concise at the representative desktop size.

## Engineering checks

- Run repository-provided formatting, lint, type, test, and production build commands as relevant.
- Inspect browser console and server logs for new errors.
- Test keyboard navigation and visible focus for editor controls.
- Respect reduced-motion preferences for reorder transitions and avoid disorienting layout animation.
- Confirm every preview-selectable target can also be reached without relying on pointer interaction.
- Verify labels, buttons, dialogs, and drag alternatives are accessible.
- Check that unrelated user changes remain intact.

## Persistence and operations

- Select `Apply`, reload, and confirm durable content.
- Test failure and retry without losing edits.
- Confirm media URLs remain durable and have intended access controls.
- Confirm responsive variants, metadata references, manifests, favicons, and social-sharing consumers resolve the applied assets after reload when applicable.
- Treat migrations, deployments, and production writes as separate authorized actions.

## Handoff

Report:

- editable areas and intentionally non-editable areas;
- where content and media are stored;
- editable image classes, grouped variants, and intentionally excluded assets with reasons;
- apply behavior;
- authentication and permissions;
- checks completed and known limitations;
- whether the work is local, staged, or deployed.
