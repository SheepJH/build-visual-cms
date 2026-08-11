# Content discovery

## Goal

Derive the CMS model from the target website. Do not begin with assumed entities.

## Repository inspection

Identify:

- framework, routing model, rendering mode, package scripts, and deployment configuration;
- public pages, shared layouts, navigation, headers, footers, and reusable sections;
- hard-coded copy, imported content objects, JSON, Markdown, database reads, and API calls;
- repeated rendering through arrays or mapped data;
- content images, CSS and inline background images, responsive sources, videos, documents, and external-link fields;
- branding and metadata assets such as logos, favicons, Apple touch icons, web-app manifest icons, mask icons, and default social-sharing images;
- theme variables, fonts, colors, spacing, radii, and reusable component variants;
- existing authentication, roles, storage, database, server actions, and API routes;
- existing admin surfaces and editing conventions.

Respect repository instructions and dirty worktrees. Prefer fast file search and inspect representative components before broad refactoring.

## Classify candidates

Classify each candidate as one of:

1. Global content: branding, navigation labels, contact details, social links, footer.
2. Global style: controlled tokens such as brand color, typography preset, spacing density, or card style.
3. Page content: headings, descriptions, calls to action, metadata.
4. Section content: locally grouped fields.
5. Collection: ordered repeatable objects displayed as a list, grid, carousel, timeline, table, or tabs.
6. Operational configuration: secrets, permissions, integration IDs, database configuration. Do not expose these as ordinary CMS fields.

## Image asset inventory

Inventory operator-relevant images across components, content files, stylesheets, public/static directories, framework metadata, manifests, and storage-backed records. Classify each asset as:

- site-wide identity: logo variants, favicon, touch icon, manifest icons, mask icon, and default social image;
- page or section content: hero, editorial, gallery, card, testimonial, team, product, or other project-specific imagery;
- presentation: background, texture, decorative illustration, or responsive art direction;
- system asset: library icons, authentication/security imagery, generated build output, or implementation-critical graphics.

Make operator-managed assets editable by default. Do not expose system assets or assets whose replacement would break behavior, licensing, security, or layout assumptions. Record excluded assets and a concise reason instead of silently omitting them.

Create a global favicon candidate even when discovery finds no favicon file, metadata entry, or manifest reference. Absence is an editable empty state, not a reason to omit the field.

Trace variants that represent one logical image, including desktop/mobile crops, `srcset` sources, light/dark logos, favicon sizes, and manifest icon sets. Model them together when operators must understand or replace them as a group.

## Infer field semantics

- Use a short text field for labels and concise titles.
- Use multiline input when intentional line breaks matter.
- Use rich text only when authors genuinely need formatting; avoid it for structured content.
- Use a link field with validation for URLs and internal routes.
- Use an image or media field when the value represents an asset, not a raw URL editing task.
- Use a list for repeated primitive values.
- Use a collection for repeated objects with stable IDs.
- Use a select, radio group, or preset when the allowed choices are finite.
- Use a boolean switch only for clear on/off behavior.

## Discovery output

Before implementation, maintain a compact internal map containing:

- content group and source location;
- proposed field type;
- public component consuming it;
- whether it is global, page-local, or repeated;
- persistence and migration implications;
- image usage locations, required dimensions or aspect ratios, variants, alternative text behavior, and whether replacement is safe;
- intentionally excluded image assets and the reason for exclusion;
- uncertainty requiring user input.

Do not force every visible string into the CMS. Prioritize content operators reasonably need to change and keep system copy or implementation details in code when appropriate.
