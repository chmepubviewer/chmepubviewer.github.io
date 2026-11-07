# CHM / EPUB Viewer

A browser-based reader for CHM help files and EPUB books, with full-text search, highlighted matches, back/forward navigation, and automatic reading-session restoration. Open a file directly in your browser—no installation or account required.

![License](https://img.shields.io/badge/license-MIT-green.svg)
![Dependencies](https://img.shields.io/badge/dependencies-JSZip%20%7C%20epub.js-blue.svg)
![Storage](https://img.shields.io/badge/storage-IndexedDB%20%2B%20localStorage-orange.svg)

## 📖 Live Demo

**[https://chmepubviewer.github.io](https://chmepubviewer.github.io)**

## ✨ Features

### Supported Formats

| Format | Support |
|--------|---------|
| CHM | ITSF container parsing, directory extraction, and LZX decompression |
| EPUB | Book rendering, navigation, and search using epub.js |

Open files using the **Open** button or drag and drop them onto the application.

### CHM Reading

- Built-in JavaScript LZX decoder
- Support for uncompressed and LZX-compressed content sections
- Preservation of unfinished LZX matches across output-frame boundaries
- Validation of compressed data, dictionary offsets, and block lengths
- Table of contents extracted from `.hhc` files
- HTML-page listing when table-of-contents entries are unavailable
- Default-topic detection from CHM metadata
- Internal article links and anchor navigation
- Embedded images, stylesheets, and supported font resources
- Character-encoding detection with language-based fallbacks
- Sandboxed article display with scripts removed

> CHM compatibility varies. Files with unsupported structures or damaged compressed data may not open.

### EPUB Reading

- Paginated, single-page reading layout
- Table-of-contents navigation
- Previous and next page controls
- Left and right arrow-key navigation
- Current section’s displayed page indicator
- Reading-position restoration using EPUB CFI locations
- Search highlights rendered through the epub.js annotation API without modifying source text

### Full-Text Search

Search across the opened book using the **Search** sidebar.

- Case-insensitive text search
- Result titles and text excerpts
- Highlighted matches in result excerpts and book content
- Click a result to open the matching article or EPUB location
- Search progress and skipped-section reporting
- Recent-query suggestions
- Separate controls to clear the active search or search history

**CHM search:**

- Searches HTML and XHTML article text
- Groups results by article
- Shows the number of matches in each matching article
- Opens a selected result and scrolls to its first highlighted match

**EPUB search:**

- Searches book sections using epub.js
- Returns individual match locations
- Opens selected matches using range CFIs
- Highlights matches using EPUB annotations

Search displays up to **500 results**. For CHM, each result represents an article; for EPUB, each result represents a match. Up to **3,000 matches** are highlighted in a single CHM article.

> Search operates on text content, not text embedded in images. EPUB matching behavior depends on epub.js.

### Back and Forward Navigation

- **Back** — Return to a previously visited reading location
- **Forward** — Revisit a location after going back
- Remembers CHM article paths, anchors, and scroll positions
- Remembers EPUB reading locations
- Restores the associated search-highlight query when revisiting a location
- Retains up to **200 navigation entries**
- Saves navigation history in **localStorage**, with a session snapshot in IndexedDB

Opening a new book starts a new navigation history. Navigating somewhere new after going back replaces the forward history.

> Back and Forward follow reading history. EPUB previous/next page controls move through the book’s pages.

### Search History

- Retains up to **30 recent queries**
- Saves queries in **localStorage**
- Keeps recent queries available when opening another book
- Offers clickable recent searches and input suggestions
- Remembers the active query for the saved reading session
- Reruns the saved query after restoring the book

Search results are regenerated from the book rather than stored as a permanent index.

### Automatic Local Saving

The application automatically saves:

- **Loaded file** — The original CHM or EPUB file is stored in IndexedDB.
- **Selected CHM article** — Article path and anchor are remembered.
- **CHM scroll position** — Resume at the saved vertical position.
- **EPUB reading position** — Resume from the saved CFI location.
- **Navigation history** — Back/forward entries and the current history position.
- **Search state** — Recent queries, active search, and highlight query.
- **Interface preferences** — Language, text size, sidebar visibility, and selected sidebar tab.

On page reload, the saved file is reopened and the reading session is restored.

Only the **most recently saved book** is retained. Opening and successfully saving another file replaces it.

> Files, histories, and preferences are stored in the current browser and site origin—not in the cloud. Clearing site data, private browsing, or browser storage eviction can remove them. Keep your original book files.

### Reading Controls

- Adjustable text size from **60% to 220%**
- Text-size changes in 10-percentage-point steps
- Persistent text-size setting across articles and reloads
- Collapsible sidebar with **Contents** and **Search** tabs
- Highlighted current article or chapter
- Automatic sidebar scrolling to the restored selection
- External CHM web links opened in a new tab

> **A− / A+** control the application’s text-size setting, not browser zoom. Fixed-size formatting inside some books may limit their effect.

### User Interface

- Ukrainian and English translations
- Dark application interface with a light reading area
- Responsive desktop and mobile layout
- Overlay sidebar on narrow screens
- Drag-and-drop file opening
- Loading and decompression progress indicators
- Search status announcements
- Toast notifications for loading, restoration, and storage errors
- Visible keyboard-focus indicators

## ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `←` | Previous EPUB page |
| `→` | Next EPUB page |
| `Enter` in the search field | Search the opened book |

EPUB arrow-key navigation is disabled during file loading or controlled navigation, while editing form fields or editable content, and when Ctrl, ⌘, or Alt is pressed.

CHM articles use normal browser scrolling and link navigation. Use the toolbar buttons for application Back and Forward navigation.

## 🚀 Getting Started

### Online Usage

Visit **[chmepubviewer.github.io](https://chmepubviewer.github.io)**.

1. Select **Open** or drag a `.chm` or `.epub` file into the window.
2. Choose an article or chapter from **Contents**.
3. Adjust text size using **A−** and **A+**.
4. Open **Search**, enter a query, and select a result.
5. Use **Back** and **Forward** to revisit reading locations.
6. Switch between **UK** and **EN** as needed.
7. Reload the page to restore the saved book, position, and histories.

Wait for the successful save notification before closing a newly opened book.

### Run Locally

Clone the repository:

```bash
git clone https://github.com/chmepubviewer/chmepubviewer.github.io.git
cd chmepubviewer.github.io
```

Alternatively, download and extract the repository ZIP from GitHub.

Open the application’s HTML file directly in a modern browser. For a root-page deployment, this is `index.html`; a standalone copy may be named `ChmEpubViewer.html`.

No build step or package installation is required.

The page loads **JSZip** and **epub.js** from jsDelivr. An Internet connection is needed to fetch these libraries unless they are already cached or you host local copies.

> Storage behavior for directly opened `file://` pages varies between browsers. Use the live site for a consistent site origin. Saved books do not automatically transfer between the live site, local files, browser profiles, or devices.

### Deploy to GitHub Pages

1. Fork or clone the [repository](https://github.com/chmepubviewer/chmepubviewer.github.io).
2. Ensure the application HTML is named `index.html` for root-page hosting.
3. Enable GitHub Pages for the appropriate branch and folder.
4. Open the published site.

## 🔎 Searching and Revisiting Results

### Search a Book

1. Open a CHM or EPUB file.
2. Select **Search** in the toolbar or sidebar.
3. Enter a word or phrase.
4. Press **Enter** or select the search button.
5. Select a result to open its location and view highlighted matches.

### Reuse a Previous Query

Recent queries appear when no search is active and are also available as input suggestions. Select a query to search the currently opened book.

### Clear Search Data

- **Clear search** removes the active query, results, and current highlights.
- **Clear history** removes the recent-query list without clearing the active search.

Clearing a search does not erase navigation history. Revisiting an older history entry can restore its associated highlight query.

## 💾 Saving and Restoring Reading Sessions

### What Is Stored?

| Storage | Contents |
|---------|----------|
| IndexedDB | Original book file, filename, session identifier, and a reading-session snapshot including navigation history and active search state |
| localStorage | Interface preferences, recent search queries, navigation history, and a recent reading-position snapshot |

The application uses session identifiers to prevent delayed position updates from being applied to a different book.

### Automatic Restoration

When the page opens:

1. The application reads the saved file from IndexedDB.
2. It selects the newer matching reading-session snapshot.
3. It reopens the EPUB or decompresses the CHM.
4. It restores the reading location and back/forward history.
5. It reruns the active search and restores search highlights where applicable.

For CHM files, restoration falls back to the default topic or an available HTML page if the saved article cannot be found.

For EPUB files, an unusable saved CFI falls back to the book’s initial location.

### If Saving Fails

The book can still be read in the current session if browser storage is unavailable or full. Notifications report file-saving or local-state-saving failures.

Check that:

- Site storage is enabled.
- The browser has available storage space.
- You are using the same browser profile and site address.
- Site data has not been cleared.

The application does not currently provide a multiple-book library, session export, or cloud synchronization.

## 🛠️ Technologies Used

- **HTML5** — Application structure, file selection, and search suggestions
- **CSS3** — Responsive layout, themes, transitions, and animations
- **Vanilla JavaScript** — Application logic, search, history, and CHM processing
- **Custom LZX decoder** — CHM content decompression
- **[epub.js 0.3.93](https://github.com/futurepress/epub.js)** — EPUB rendering, navigation, search, and highlights
- **[JSZip 3.10.1](https://github.com/Stuk/jszip)** — ZIP support for EPUB processing
- **IndexedDB** — Persistent file and reading-session storage
- **localStorage** — Preferences, navigation history, and search history
- **File and Blob APIs** — File access and embedded-resource URLs
- **DOMParser** — CHM HTML and table-of-contents parsing
- **TreeWalker and Range APIs** — CHM text extraction and match highlighting
- **TextDecoder** — Character-encoding support
- **Sandboxed iframe** — CHM article rendering

## 🔒 Privacy and Storage

Book parsing, CHM decompression, search, and reading-session storage happen locally in the browser. The application code does not upload loaded books or search queries to a server or require an account.

However:

- Third-party JavaScript libraries are downloaded from jsDelivr.
- Book content that references external resources may cause network requests.
- Opening an external link navigates to the linked website.
- Local storage is subject to browser quotas and eviction policies.
- Search queries and navigation history remain in the current browser profile until cleared or replaced.

CHM pages are rendered in a sandboxed iframe. Scripts, inline event handlers, and several embedded-content elements are removed before display. This is not a guarantee that arbitrary book content is completely safe; use trusted files.

## 📁 Project Structure

For GitHub Pages hosting:

```text
chmepubviewer.github.io/
├── index.html      # Complete application: HTML, CSS, and JavaScript
├── README.md       # Project documentation
└── LICENSE         # MIT License
```

The favicon is embedded in the HTML as an SVG data URL. No separate image asset is required.

## ⚠️ Current Limitations

- Only one saved book is retained at a time.
- Browser-local saves are not synchronized across devices or browsers.
- Navigation history is limited to **200 entries**, and recent search history to **30 queries**.
- Search displays up to **500 results**; matches beyond this limit may not be highlighted.
- Search does not provide OCR, regular-expression input, or advanced filtering.
- Searching large books can take time and increase memory usage. EPUB section documents remain cached during the session.
- Large CHM files are decompressed into memory and may affect responsiveness.
- CHM decompressed content is limited to **512 MiB**.
- The LZX implementation supports specific CHM control-data and reset-table layouts; not every CHM file is guaranteed to work.
- CHM scripts, forms requiring scripts, and interactive embedded content are not supported.
- Complex CHM resource references or legacy page layouts may not render exactly as in a native viewer.
- EPUB compatibility depends on epub.js; DRM-protected books are not supported.
- The EPUB page indicator describes the current section’s displayed pages, not a fixed whole-book page count.
- CHM scroll restoration uses a pixel offset, so a changed viewport or page layout may shift the visible text.
- Search highlights are not user-created annotations. There is no bookmark collection or note-taking system.
- External CDN dependencies mean fully offline startup is not guaranteed.
- Concurrent use in multiple tabs is not designed as a synchronized reading session.

## 🤝 Contributing

Contributions are welcome!

1. Fork the [repository](https://github.com/chmepubviewer/chmepubviewer.github.io).
2. Create a feature branch:

   ```bash
   git checkout -b feature/amazing-feature
   ```

3. Commit your changes:

   ```bash
   git commit -m "Add amazing feature"
   ```

4. Push your branch:

   ```bash
   git push origin feature/amazing-feature
   ```

5. Open a pull request.

Please test:

- CHM and EPUB file opening
- Table-of-contents and internal-link navigation
- Back/forward navigation and history restoration
- Full-text search, result navigation, and highlighted matches
- Search-history persistence and clearing controls
- CHM article and scroll restoration after reload
- EPUB reading-position restoration after reload
- Replacing the saved book with a different file
- Storage-unavailable and storage-full behavior
- Language switching and text-size controls
- Narrow mobile viewports

For LZX changes, include a reproducible test case or a redistributable sample file where possible.

### Ideas for Contributions

- [ ] Advanced search filters and next/previous match controls
- [ ] Faster search indexing for large books
- [ ] Multiple-book library
- [ ] Bookmarks and annotations
- [ ] Browsable navigation-history panel
- [ ] Additional CHM compatibility tests
- [ ] Background decompression using Web Workers
- [ ] Offline support with locally hosted dependencies
- [ ] Saved-session export and import
- [ ] Accessibility improvements
- [ ] Automated browser tests
- [ ] Additional interface languages

## 📝 License

MIT. See [LICENSE](LICENSE) for details.

Third-party dependencies retain their respective licenses.

---

<p align="center">Made with ❤️ for readers and explorers of digital libraries</p>
