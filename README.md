# WFS Email Signature Generator

A self-service, static web page that lets U.S. Wildland Fire Service staff
generate their standard email signature block as a downloadable PNG. No
backend, no build step — just plain HTML/CSS/JS running entirely in the
browser.

## How it works

- The user picks their Geographic Area, then fills in Name, Job Title, and
  Agency Email (required), plus Office Phone, Mobile Phone, and FireNet
  Email (each individually optional) — **except at least one of Office or
  Mobile phone must be filled in**, since some staff only carry a cell
  phone.
- The signature is a stacked block (no divider bar): Name, Job Title,
  "U.S. Wildland Fire Service," the selected Geographic Area (e.g.
  "Northern Rockies Geographic Area" — regions whose name already ends in
  "Area," like "Southern Area," are left as-is rather than doubling up),
  "U.S. Department of the Interior," an Office/Mobile phone line, and one or
  two "Email:" lines — matching the department's standard shield-logo
  signature format.
- A live preview renders on an HTML `<canvas>` as they type.
- "Download PNG" saves the signature with a solid **white background**.
- "Copy Image" copies it straight to the clipboard (supported in most modern
  browsers) so it can be pasted directly into an email client's signature
  settings.
- The output is a flat image — the email addresses are styled to look like
  links (teal, underlined) but are not clickable, since a PNG has no way to
  carry a real hyperlink.

## Publishing to GitHub Pages

1. Create a new GitHub repository (public, or private with GitHub Pages
   enabled on your plan).
2. Add these two files/folders to the repo root, preserving the structure:
   ```
   index.html
   assets/logo.png
   ```
3. Commit and push to the `main` branch.
4. In the repo, go to **Settings → Pages**.
5. Under "Build and deployment," set **Source** to `Deploy from a branch`,
   branch `main`, folder `/ (root)`. Save.
6. GitHub will give you a URL like:
   `https://<your-org-or-username>.github.io/<repo-name>/`
   Share that link with staff.

No other configuration is needed — everything (fonts, layout, logo) is
self-contained in the repo.

## Customizing

- **Logo**: replace `assets/logo.png` with an updated shield graphic if it
  ever changes. Recommended: transparent PNG, similar aspect ratio (roughly
  170×200) for best fit.
- **Geographic Areas list**: edit the `<select id="region">` options in
  `index.html`.
- **Static lines** ("U.S. Wildland Fire Service," "U.S. Department of the
  Interior"): hardcoded as plain strings in the `lines` array built inside
  `render()` in `index.html`.
- **Colors/fonts/line spacing**: `NAME_SIZE`, `BODY_SIZE`, `LINE_PITCH`,
  `PADDING`, `GAP_LOGO_TEXT`, and `RIGHT_MARGIN`, defined just above
  `render()` in `index.html`. The canvas is sized dynamically every render —
  not a fixed frame — to hug the actual content: its height is driven by the
  number of lines × `LINE_PITCH`, and its width by the widest rendered line
  (measured with `ctx.measureText`) plus margins. The logo's displayed height
  always matches the full text block's height exactly, flush top-to-bottom
  with the text, and its width follows from its own aspect ratio — so it can
  never be clipped or look mismatched in scale. Very long names/titles/emails
  shrink (via `fitFontSize`, capped by `MAX_TEXT_WIDTH`) rather than growing
  the canvas without bound.

## Browser support

Works in all modern evergreen browsers (Chrome, Edge, Firefox, Safari).
"Copy Image" relies on the Clipboard API, which is well-supported but may be
blocked on very old browsers — "Download PNG" always works as a fallback.
