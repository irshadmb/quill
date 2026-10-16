Title: fix(editor): sanitize HTML output from getHTML/convertHTML to prevent script injection (Snyk report)

Summary:
Sanitize HTML produced by Editor.getHTML/convertHTML to remove scriptable elements and unsafe attributes/URLs. This addresses a Snyk-reported injection risk by:

- Removing forbidden elements (script, iframe, object, embed, style) returned by blots.
- Stripping event handlers and style attributes.
- Whitelisting attributes (href, src, alt, title, width, height, colspan, rowspan, data-*).
- Validating href/src values to disallow javascript: and unsafe data: URIs.
- Reconstructing DOM elements safely rather than returning outerHTML/innerHTML verbatim.

Tests:
- Build & unit tests: yarn build && yarn test
- Manual checks: insert malicious markup (script, iframe, onclick handlers, style URLs, javascript: links) and verify editor.getHTML() strips or neutralizes them. Verify normal links/images/lists/tables still serialize correctly.

Security rationale:
- Removes executable elements and event handlers.
- Validates URL schemes and disallows javascript/data URIs.
- Escapes attribute values to avoid injection via attributes.

Patch location: packages/quill/src/core/editor.ts
