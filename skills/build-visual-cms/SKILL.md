---
name: build-visual-cms
description: Analyze an existing website and design or implement a project-specific, preview-first visual CMS with a compact structure-aware editing panel, full-height desktop live preview using the real public components, field-focus bidirectional selection, independently scrolling editor and preview panes, operator-managed image and always-available favicon editing, global style controls, sortable repeatable content, undo/redo, and clear draft/publish states. Use when asked to add a CMS, admin editor, visual content management, live website preview, editable site content, image management, visual selection, or non-developer publishing workflow to an existing web project. Do not assume any industry, page type, or content model.
---

# Build Visual CMS

Build a CMS that adapts to the website instead of imposing a generic demo schema. Preserve the public design and make routine content changes safe for non-developers.

## Core rules

- Inspect the project before designing the CMS. Infer its framework, routes, component boundaries, content sources, design tokens, authentication, storage, and deployment path.
- Treat existing domain concepts only as project-local data. Never introduce speakers, products, posts, events, teams, or other entities unless the target site contains them.
- Reuse the real public components in the preview. Do not maintain a second approximation of the website.
- Prioritize operator ease and preview visibility. Keep the editor compact and task-focused, let the preview use the remaining workspace, fill the available shell height, and keep all content reachable through pane-local scrolling without clipping.
- Keep editor and preview selection synchronized while keeping their scroll containers independent. A selection initiated in one pane may reveal, navigate, or scroll only the opposite pane; it must not move the initiating pane or bind both panes to one scroll position.
- Treat focus or click on every mapped editor control as selection of its preview target. Navigate, reveal, scroll, and highlight the preview once on target change; do not repeat scrolling on each keystroke or disrupt input focus.
- Map editable targets down to the smallest meaningful field-level element, including text, media, labels, buttons, links, and nested collection values. Prefer the deepest editable target under a pointer and fall back to its item, component, or section only when no finer mapping exists.
- Make the editor reflect the public layout's meaningful spatial structure and layout type at the desktop preview width. Preserve direction, grouping, hierarchy, ordering, grid areas, featured items, and row or column spans; never flatten an asymmetric composition into equal cards or turn a public list into an editor grid merely for convenience.
- Separate content data from presentation without rewriting unrelated architecture.
- Match controls to content semantics: short text, multiline text, rich text, image, link, number, boolean, select, list, object, and collection.
- Discover and classify all image assets that operators may reasonably need to change. Make them editable by default, including content, background, responsive, branding, app-icon, and social-sharing images; document every intentional exclusion and its reason. Always provide a favicon field in global settings even when the site has no current favicon.
- Give ordered collections deliberate drag-and-drop with a dedicated handle, clear drag preview and insertion target, pane-local auto-scroll, cancellation, completion feedback, undo, and accessible move controls. Keep selection and focus stable by item ID.
- Preserve unrelated user changes and existing visual conventions.
- Do not deploy or migrate production data unless the user authorizes it.

## Workflow

### 1. Discover the project

Read [references/content-discovery.md](references/content-discovery.md) completely. Inspect the repository and identify editable content candidates, repeated structures, shared settings, current persistence, and operational constraints.

Ask only about choices that cannot be inferred safely and materially change the implementation, such as draft-versus-immediate publishing, administrator access, or a new storage provider.

### 2. Propose the content model

Group discovered values into:

- global settings shared across the site;
- page or section content;
- repeatable collections;
- operational settings that should not be exposed as content.

Prefer explicit typed schemas. Preserve stable IDs for collection items. Distinguish empty values from missing values and plan backward-compatible defaults for existing saved data.

Read [references/cms-architecture.md](references/cms-architecture.md) completely before selecting state, persistence, authentication, media, or publishing architecture.

### 3. Design the editor experience

Read [references/editor-ux.md](references/editor-ux.md) completely. Default to a compact editing panel beside a dominant live preview, and adapt their proportions to the target desktop workspace without arbitrary fixed widths.

Make the common path obvious: choose a page, choose a section or item, edit, review, and save or publish. Avoid exposing raw implementation details or unconstrained CSS when safe presets are sufficient.

Treat the editor and preview as two coordinated views of the same content tree, not one coupled scrolling surface. Require stable addressable IDs for editable pages, sections, components, collection items, and fields where needed for reliable selection, source-aware scrolling, route changes, tab activation, and highlighting.

### 4. Implement incrementally

Use the project's existing framework, styling system, validation library, and data layer when suitable. Start with one representative page or content group to prove the architecture, then extend the established pattern.

Ensure editor state drives the same components used publicly. Add reusable field and collection primitives only when they reduce duplication in the actual project. Do not build a universal form engine speculatively. Implement bidirectional editor-preview selection for the representative page before extending the pattern.

For lists and grids, first match the public collection's layout type and order, then support add, edit, delete, polished pointer and keyboard reordering, empty state, and meaningful item summaries. Add media preview, replacement, removal, upload progress, and errors when media is editable.

Treat site-wide identity assets as explicit global settings. Always include favicon upload or replacement with a clear empty state; preserve an existing framework metadata pipeline or add the smallest framework-native favicon integration when none exists. Preserve icon manifests, responsive-image, optimization, and storage conventions rather than replacing them with generic URL fields.

### 5. Connect persistence and publishing

Keep content operations behind a small repository boundary when practical: load, save draft, publish, and restore or version. Use the existing backend when one exists. Do not add infrastructure merely for architectural purity.

Make unsaved, saving, saved, publishing, published, and failed states visible. Warn before losing dirty state. Treat destructive deletion and schema migration with appropriate recovery or confirmation.

### 6. Validate

Read [references/validation-checklist.md](references/validation-checklist.md) completely and perform checks proportional to the change. At minimum, run the project's lint/type/build checks and verify representative editing flows and desktop preview behavior.

Report what was implemented, persistence and publishing behavior, validation performed, and any remaining operational setup. Clearly distinguish local implementation from production deployment.

## Scope control

- If the user asks only for a proposal or audit, do not modify files.
- If the site has no authentication or persistence decision, implement only the safe local/editor architecture unless the user chooses the missing system.
- If adding CMS capability would require a major framework rewrite, present the smallest viable integration and its tradeoffs before expanding scope.
- Examples from prior projects are patterns, never default content models.
