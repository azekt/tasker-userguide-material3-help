# Tasker Documentation – Material 3 Dynamic Theming

Material 3 (Material You) dynamic styling for Tasker help pages in WebView using a single CSS stylesheet, with zero external dependencies and automatic light/dark fallback for standard web browsers.

---

## Repository Structure

```text
tasker-userguide-material3-help/
├── README.md
├── compare_content.html        # Offline browser tool to compare text content between help folders
└── help/                       # Styled help pages + stylesheet + assets
    ├── styles.css              # Single stylesheet (Material 3 + fallbacks)
    ├── images/                 # Lightweight vector icons (SVG)
    ├── ah_set_variable.html    # Example action help page
    ├── ah_show_scene_v2.html
    └── ...
```

---

## How It Works

### 1. Inside Tasker (Scenes V2 WebView)
Tasker exposes system theme roles in the WebView as CSS variables (`--tasker-<role>`, e.g., `--tasker-primary`, `--tasker-surfaceDim`, `--tasker-primaryContainer`, `--tasker-onSurface`).

`styles.css` maps UI elements directly to these roles:
```css
--bg-color: var(--tasker-surfaceDim, var(--fb-background));
--text-color: var(--tasker-onSurface, var(--fb-on-surface));
--link-color: var(--tasker-primary, var(--fb-primary));
--param-bg-color: var(--tasker-primaryContainer, var(--fb-container-high));
```
Colors automatically follow the device's dynamic theme palette and system light/dark mode.

### 2. Outside Tasker (Standard Browsers)
When viewed outside Tasker, `--tasker-*` variables are undefined, falling back to `--fb-*` colors. The stylesheet respects system theme preferences via `@media (prefers-color-scheme: dark)` as well as manual theme toggles (`[data-theme='dark']`).

### 3. Lightweight & Self-contained
- **No external frameworks:** Pure CSS; no Bootstrap, CDN scripts, or external font downloads.
- **Fast loading:** Minimal overhead for instant rendering in WebView.
- **Vector icons:** Crisp, small SVGs (100–500 bytes) matching Tasker's iconography.

---

## Testing

- **In Tasker:** Load any page (e.g. `help/ah_set_variable.html`) into a Scene V2 WebView element. Changing the system color palette or dark mode in Android Settings updates the page colors in real time.
- **In a desktop browser:** Open any `.html` file in `help/` directly to inspect the light/dark fallback layout.

---

## Content Comparison Tool (`compare_content.html`)

A standalone, client-side utility to compare the textual content of HTML documentation files across two folders. It strips HTML tags and styles to isolate and diff only the rendered body text, verifying whether documentation content has changed.

### Usage:
1. Open `compare_content.html` in any modern web browser.
2. Select or drag-and-drop:
   - **Old Folder:** original `help` directory
   - **New Folder:** this repository's `help` directory
3. Browse diffs:
   - **Single File:** Side-by-side view with word-level diffing (green: added, red: deleted, orange: modified) and keyboard navigation.
   - **All Files on One Page:** Full summary report listing changed, identical, and missing files, with an option to download a static HTML report.

### Technical Details:
- **100% Client-side:** Runs entirely in the browser using the Web File System and DOMParser APIs.
- **No dependencies:** Does not require PHP, Node.js, local servers, or network access. Files are processed strictly in local memory.
