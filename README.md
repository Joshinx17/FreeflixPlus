# FreeFlix+

FreeFlix+ is a lightweight, static entertainment directory for discovering movie, series, anime, cartoon, and drama websites from one responsive interface.

The project uses a noir-inspired visual style with a curated card layout, client-side discovery tools, an accessible information drawer, and browser-only favorites. It has no backend, account system, database, build step, or framework dependency.

> **Important:** FreeFlix+ is a directory, not a content-hosting service. External websites may be unofficial, ad-supported, unavailable, or unlicensed in your region. Check each destination before opening it, follow applicable laws, and use normal browser security and privacy protections.

## Features

### Discovery

- Search by site name, category, type, or description.
- Filter by category, including cinematic, animated, and drama collections.
- Filter by `Streaming` or `Downloading`.
- Sort by the original curated order or alphabetically.
- View a live result count.
- See a clear empty state when no entries match.

### Personalization

- Mark sites as favorites with the star button on each card.
- Toggle a favorites-only view from the discovery toolbar.
- Favorites persist in the current browser using `localStorage`.
- Favorites are keyed by the site URL, so they do not change the editable content data.

### Accessibility and usability

- Skip-to-content link for keyboard users.
- Semantic headings, labels, buttons, and dialog roles.
- Visible keyboard focus indicators.
- Drawer focus return and focus trapping.
- `Escape` closes the information drawer.
- Screen-reader announcements for result-count changes.
- Reduced-motion support through `prefers-reduced-motion`.
- Mobile-friendly touch targets and stacked controls on narrow screens.
- Sticky collection headings and a back-to-top button for long pages.

### Performance and resilience

- Images use lazy loading and asynchronous decoding.
- Fixed image containers reduce layout shifting while images load.
- Broken images receive an inline fallback instead of leaving a blank card.
- Filtering hides and reorders existing card elements rather than rebuilding the entire catalog on every search keystroke.
- External links open in a new tab with `noopener noreferrer`.

## Quick start

The project is plain HTML, CSS, and JavaScript. No package installation is required.

### Open directly

Open `index.html` in a modern browser. Most features work when opened directly from the filesystem.

### Run a local server

Using Python:

```powershell
python -m http.server 4173
```

Then open [http://localhost:4173](http://localhost:4173).

Using Node.js, any static server can be used. For example, if you already have `serve` available:

```powershell
serve .
```

## Project structure

```text
FreeflixPlus/
├── index.html          # Page shell, discovery controls, drawer, and footer
├── style.css           # Noir theme, responsive layout, states, and accessibility styles
├── script.js           # Editable directory data and client-side interactions
├── Assets/             # Local image assets used by selected entries
└── README.md           # Project documentation
```

## Adding or editing websites

All website details are intentionally kept in the `library` array near the top of `script.js`. Add or edit entries there; do not change the rendering logic for normal content updates.

Each category has this shape:

```js
{
    category: "Category name",
    items: [
        {
            name: "Example site",
            type: "Streaming",
            url: "https://example.com/",
            img: "./Assets/example.png",
            desc: "A short description shown in the information drawer."
        }
    ]
}
```

### Field guidance

| Field | Required | Purpose |
| --- | --- | --- |
| `category` | Yes | Section used by the category filter and section heading. |
| `name` | Yes | Card title, search text, and accessible labels. |
| `type` | Yes | Usually `Streaming` or `Downloading`; used by the type filter. |
| `url` | Yes | Destination opened by the Visit button and drawer. |
| `img` | Yes | Remote image URL or a local path such as `./Assets/example.png`. |
| `desc` | Yes | Longer explanation displayed in the Info drawer and included in search. |

Use HTTPS URLs where possible. Keep descriptions factual and concise, and verify links before publishing them. The application escapes text before inserting it into the card markup, but trusted, well-formed values are still recommended.

## How the JavaScript is organized

`script.js` has two intentionally separate responsibilities:

1. **Content data:** the `library` array is the easy-to-edit source of categories and websites.
2. **Interface behavior:** the code below the data handles rendering, search, filtering, sorting, favorites, image fallbacks, the drawer, and scrolling.

The renderer creates a record for each item with its category and original order. Search and filters update card visibility in place. Sorting changes the order of existing card nodes, which keeps interaction state and page performance predictable.

Favorites use the storage key `freeflixplus-favorites`. Clearing browser site data or using private browsing can remove them; this does not affect `library`.

## Styling and responsive behavior

The visual system is defined in `style.css` with a small set of CSS variables:

- `--cinematic-black`: page background.
- `--cinematic-gold`: accent, focus, and interactive highlight color.
- `--deep-gray`: card surface.
- `--text-main` and `--muted`: readable foreground colors.
- `--line`: subtle borders.

Responsive behavior includes:

- Desktop multi-column card grids.
- Tablet toolbar wrapping before it can overflow.
- Two-column card grids on smaller screens.
- Single-column controls and stacked card actions on narrow phones.
- A full-width drawer on mobile.
- A final single-column card layout around 390px and below.

When changing spacing or colors, preserve visible focus outlines and minimum touch-target sizes.

## Accessibility notes

The site should be usable with keyboard navigation alone:

- Use `Tab` to reach search, filters, favorites, cards, and drawer controls.
- Use `Enter` or `Space` to activate buttons.
- Use `Escape` to close the Info drawer.
- Use the skip link to jump directly to the catalog.

The drawer is marked as a modal dialog and returns focus to the control that opened it. Result counts use `aria-live`, and decorative icons are hidden from assistive technology.

## Validation

Run these checks after editing:

```powershell
node --check script.js
git -c core.whitespace=cr-at-eol diff --check
```

Also manually verify:

- A search term matches names, categories, types, and descriptions.
- Category and type filters work together.
- A favorite remains after reloading the page.
- Favorites-only mode shows the correct empty state when nothing is saved.
- Broken image URLs show the fallback artwork.
- The drawer opens, traps focus, closes with `Escape`, and restores focus.
- The toolbar and card actions remain usable at approximately 320px wide.
- Reduced-motion preferences remove large transitions and animation movement.

## Deployment

Because this is a static site, deploy the project directory to any static hosting provider that serves HTML, CSS, JavaScript, and the `Assets` folder. Ensure the deployed paths preserve the relative structure used by `index.html`, `style.css`, `script.js`, and local images.

Before publishing, check that:

- Every `url` in `library` points to the intended destination.
- Local image paths use the correct letter casing.
- External image hosts allow browser embedding.
- The site’s safety notice and external-link behavior remain visible.

## License and content responsibility

This repository contains the FreeFlix+ interface and directory data. Images, trademarks, and linked websites may belong to their respective owners. Add only links and assets you are permitted to publish, and follow the laws and terms applicable to your location.
