---
name: build-visual-cms
description: Analyze an existing website and design or implement a scope-bounded, preview-first visual CMS with a compact left editing pane, dominant full-height desktop live preview using the real public components, field-focus bidirectional selection, independently scrolling panes, structure-faithful editing, operator-managed images, always-available favicon editing, site-wide style tokens, sortable content, and one bottom Apply action. Use when asked to add a CMS, admin editor, visual content management, live website preview, editable site content, image management, visual selection, or a non-developer editing workflow to an existing web project. Do not assume any industry, page type, or content model.
---

# Build Visual CMS

Build the smallest CMS that satisfies the requested scope while preserving the public design and making routine changes safe for non-developers.

## Choose the execution scope

Choose once before inspecting broadly. Do not invent an effort tier or silently expand scope.

- **Scoped (default):** When the user names a page, section, feature, or change, inspect, implement, and validate only that scope and its direct shared dependencies.
- **Representative:** When the user broadly asks for a CMS and the architecture is unproven, implement one representative page or content group plus required global settings first.
- **Full-site:** Use only when the user explicitly requests the entire site or all pages. Prove the pattern on one representative surface before extending it.

Do not promote scoped or representative work to full-site on your own. If no material choice requires approval, continue through the chosen scope without pausing merely to report an intermediate milestone.

## Quality invariants

Apply these to every implemented scope:

- Inspect the relevant project code before designing the CMS. Reuse its framework, routes, components, content sources, design tokens, authentication, storage, and visual conventions.
- Never invent domain entities that the target site does not contain.
- Render the real public components in preview; do not maintain preview-only approximations.
- Make the left pane entirely the compact editor and the right pane entirely the dominant live preview. Use a light neutral or clearly contrasting accessible editor surface.
- Give editor and preview separate bounded scroll roots without exception. Synchronize selection, but scroll only the opposite pane; never move the initiating pane or outer page.
- Preserve the meaningful public structure for every edited surface: layout type, direction, order, grouping, hierarchy, columns, featured items, grid areas, and spans.
- Match controls to content semantics and preserve existing data compatibility.
- Always provide global settings for primary color, typography or font family, and favicon. Bind style controls to safe shared tokens and always allow adding the first favicon.
- Keep top-level editor navigation to `Global settings` and `Page`; show the page selector only in page mode. Group applicable global controls under Brand, Style, Header, and Footer.
- Use one primary `Apply` action fixed or sticky at the bottom of the editor. Do not add top save/publish, draft-save, undo, or redo actions.
- Preserve unrelated user changes. Do not deploy or migrate production data without authorization.

## Conditional capabilities

Apply a capability only when it exists in the chosen scope; once included, follow its full quality rule rather than treating it as optional polish.

- **Editable fields:** Map each included control to the smallest meaningful preview target. Focus or click reveals and highlights the opposite target once without keystroke scroll loops.
- **Lists and grids:** Match the public collection primitive and spatial relationships. Never turn a public vertical list into an editor grid or flatten asymmetric layouts.
- **Ordered collections:** Provide deliberate drag-and-drop with a handle, destination feedback, editor-local auto-scroll, cancellation, stable IDs, and keyboard operation through the handle. Do not add standalone up/down buttons.
- **Images:** Inventory operator-relevant images inside the chosen scope and make safe assets editable. For full-site work, inventory all operator-relevant content, background, responsive, branding, app-icon, and social-sharing assets and report exclusions.
- **Shared content:** Treat shared-layout content or content reused across two or more pages as global. Keep page-only tabs, navigation, and calls to action inside their page.
- **Persistence or authentication:** Reuse the existing backend and authorization. Protect every write server-side and keep content operations behind the smallest practical load/apply boundary.

## Load references conditionally

Do not read every reference by default. Read only the files and sections needed for the chosen scope:

- Read [references/content-discovery.md](references/content-discovery.md) for representative or full-site discovery, a new content model, or asset inventory. Skip it for a narrow change whose sources are already clear.
- Read [references/cms-architecture.md](references/cms-architecture.md) when persistence, authentication, media storage, working state, or Apply behavior is in scope.
- Read [references/editor-ux.md](references/editor-ux.md) when building or changing editor layout, selection, scrolling, structure mapping, media controls, global controls, or reordering.
- Read only the applicable sections of [references/validation-checklist.md](references/validation-checklist.md) after implementation. Do not execute unrelated checklist items.

## Workflow

### 1. Inspect the bounded surface

Trace the requested surface and its direct consumers end to end. For representative or full-site work, identify shared layouts, global settings, content sources, repeated structures, and operational constraints. Ask only about choices that cannot be inferred safely and materially change the implementation.

### 2. Model only discovered content

Group values into global settings, page or section content, repeatable collections, and non-editable operational settings. Prefer explicit types, stable item IDs, backward-compatible defaults, and a clear distinction between missing and intentionally empty values.

### 3. Design the editor contract

Keep the left editor compact, clean, and structure-aware beside the dominant preview. Make the common path obvious: choose global or page scope, select a section or item, edit while reviewing preview, then select `Apply`.

Treat editor and preview as coordinated views of one content tree. Use stable target IDs and source-aware selection so route changes, tabs, highlights, and opposite-pane scrolling cannot loop or steal focus.

### 4. Prove before expanding

For representative or full-site work, implement one representative page or content group first, including real preview rendering, field selection, independent scrolling, global tokens, and Apply behavior. Pass the bounded validation budget before copying the pattern to more pages. Do not build every page first and validate afterward.

Extend only the proven pattern and only through the chosen scope. Prefer the minimum code that satisfies the invariants; do not build a universal form engine or create auxiliary implementation documents.

### 5. Apply safely

Preview working changes immediately but persist only through the single `Apply` action. Distinguish unchanged, changed, applying, applied, and failed states without adding more actions. Warn before losing unapplied work and retain failed changes for retry.

### 6. Validate within budget

By default:

1. Run the smallest repository-provided format, lint, type, test, or build checks that cover the changed files; run the full production build only when required by the project or no narrower check provides confidence.
2. Verify one representative edit-to-preview selection flow, independent scrolling in both directions, one relevant global token change, and one Apply-and-reload round trip.
3. Run additional checklist items only for capabilities changed in this task.

Do full-page matrices, every-page screenshots, exhaustive asset checks, maximum-data cases, or all-view console sweeps only when the user requests full verification, the chosen scope is full-site and the risk warrants it, or a failure suggests broader impact. Never escalate to exhaustive verification merely because time is available.

Report the implemented scope, Apply behavior, checks performed, intentionally unimplemented areas, and whether work is local, staged, or deployed.

## Scope control

- If the user asks only for a proposal or audit, do not modify files.
- If authentication or persistence is undecided, implement only the safe local/editor boundary unless the user chooses the missing system.
- If CMS capability requires a major framework rewrite, present the smallest viable integration and its tradeoffs before expanding.
- Prior-project examples are patterns, never default content models.
