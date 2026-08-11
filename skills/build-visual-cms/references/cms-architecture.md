# CMS architecture

## Single rendering source

The editor preview and public site must render the same public components with different content state. Avoid duplicated preview-only markup and CSS.

Preferred data flow:

```text
editor controls -> draft content state -> public components in preview
                              |-> persistence adapter -> published content -> public pages
```

## Content model

- Define explicit types or schemas close to the content boundary.
- Give repeatable objects stable IDs independent of array position.
- Provide backward-compatible defaults when stored content may predate new fields.
- Preserve meaningful line breaks and distinguish arrays from multiline strings based on semantics.
- Keep secrets and operational integration settings outside editable content.

## State and history

- Keep a canonical draft state in the editor.
- Derive dirty state by comparing against the last saved or published snapshot.
- Record undo history at meaningful edits and cap its size.
- Clear redo history after a new edit.
- Avoid recording initialization, remote hydration, or successful publication as user edits.
- Keep item selection by ID so reordering does not edit the wrong item.

## Persistence

Prefer a narrow boundary such as:

```ts
interface ContentRepository<T> {
  load(): Promise<T>;
  saveDraft?(content: T): Promise<void>;
  publish(content: T): Promise<void>;
}
```

Adapt this shape to the existing application rather than requiring the exact interface. Possible backends include files, an existing API, a database, or a hosted content service.

Never silently replace production persistence with browser-only storage. Local storage is acceptable for prototypes only when clearly described.

## Publishing modes

Choose deliberately:

- Immediate save: appropriate for low-risk internal sites.
- Draft then publish: appropriate when review or safe preview matters.
- Versioned publish: appropriate when rollback, audit, or multiple editors matter.

Do not implement the most complex mode by default. Match the operational need.

## Media

Reuse existing asset storage and image processing where possible. Validate file type, size, dimensions, aspect ratio, animation, and transparency only as required by the consuming component. Show upload progress and actionable errors, and store durable references rather than transient local object URLs. Do not make uploaded private documents public without explicit authorization.

Model media fields with the metadata the project actually consumes, such as alternative text, intrinsic dimensions, crop or focal point, caption, responsive variants, and light/dark variants. Do not invent transformation controls that the rendering path cannot honor.

Always model favicon as a global setting, including a missing-value state that can create the site's first favicon. Update the framework's existing metadata pipeline or add its smallest native favicon mechanism, generate or request required size variants deliberately, and preserve cache-busting behavior. Group other application identity assets when applicable. Treat default Open Graph or social-sharing images as global metadata while allowing page-specific overrides only when the site already supports or needs them.

Provide replacement, removal, and restoration semantics appropriate to each asset. Prevent removal when a required identity or layout asset has no safe fallback. Keep old durable assets recoverable when draft, rollback, or versioning promises require it.

## Authentication and authorization

Reuse established authentication. Protect editor routes and every write operation server-side; hiding UI is not authorization. If permissions vary, separate viewing, editing, publishing, and administrative capabilities.

## Preview isolation

Prevent preview navigation from replacing the editor unexpectedly. Render the configured desktop preview at a stable representative width and avoid scaling techniques that make text or controls misleading.
