# CMS architecture

## Single rendering source

The editor preview and public site must render the same public components with different content state. Avoid duplicated preview-only markup and CSS.

Preferred data flow:

```text
editor controls -> working content state -> public components in preview
                                |-> Apply -> persistence adapter -> public pages
```

## Content model

- Define explicit types or schemas close to the content boundary.
- Give repeatable objects stable IDs independent of array position.
- Provide backward-compatible defaults when stored content may predate new fields.
- Preserve meaningful line breaks and distinguish arrays from multiline strings based on semantics.
- Keep secrets and operational integration settings outside editable content.

## Working state

- Keep one canonical working state in the editor.
- Derive changed state by comparing it against the last applied snapshot.
- Preview working changes immediately without persisting each keystroke.
- Do not add draft saving, version history, or undo/redo controls.
- Keep item selection by ID so reordering does not edit the wrong item.

## Persistence

Prefer a narrow boundary such as:

```ts
interface ContentRepository<T> {
  load(): Promise<T>;
  apply(content: T): Promise<void>;
}
```

Adapt this shape to the existing application rather than requiring the exact interface. Possible backends include files, an existing API, a database, or a hosted content service.

Never silently replace production persistence with browser-only storage. Local storage is acceptable for prototypes only when clearly described.

## Apply behavior

Expose one operator action labeled `Apply` at the bottom of the left editor pane. Adapt that action to the existing durable write or publication mechanism without exposing separate save-draft and publish controls. Disable it when nothing changed, show applying and failure feedback in place, and keep failed working changes available for retry.

## Media

Reuse existing asset storage and image processing where possible. Validate file type, size, dimensions, aspect ratio, animation, and transparency only as required by the consuming component. Show upload progress and actionable errors, and store durable references rather than transient local object URLs. Do not make uploaded private documents public without explicit authorization.

Model media fields with the metadata the project actually consumes, such as alternative text, intrinsic dimensions, crop or focal point, caption, responsive variants, and light/dark variants. Do not invent transformation controls that the rendering path cannot honor.

Always model favicon as a global setting, including a missing-value state that can create the site's first favicon. Update the framework's existing metadata pipeline or add its smallest native favicon mechanism, generate or request required size variants deliberately, and preserve cache-busting behavior. Group other application identity assets when applicable. Treat default Open Graph or social-sharing images as global metadata while allowing page-specific overrides only when the site already supports or needs them.

Provide replacement, removal, and restoration semantics appropriate to each asset. Prevent removal when a required identity or layout asset has no safe fallback.

## Authentication and authorization

Reuse established authentication. Protect editor routes and every write operation server-side; hiding UI is not authorization. If permissions vary, separate viewing, applying, and administrative capabilities.

## Preview isolation

Prevent preview navigation from replacing the editor unexpectedly. Render the configured desktop preview at a stable representative width and avoid scaling techniques that make text or controls misleading.
