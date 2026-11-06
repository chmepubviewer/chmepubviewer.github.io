# CHM / EPUB Viewer

A browser-based reader for CHM help files and EPUB books, with built-in LZX decompression, table-of-contents navigation, adjustable text size, and Ukrainian/English interface options. Open a file directly in your browser and resume reading after a page reload—no installation or account required.

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
| EPUB | Book rendering and navigation using epub.js |

Open files using the **Open** button or drag and drop them onto the application.

### CHM Reading

- Built-in JavaScript LZX decoder
- Support for uncompressed and LZX-compressed content sections
- Preservation of unfinished LZX matches across output-frame boundaries
- Validation of compressed data, dictionary offsets, and block lengths
- Table of contents extracted from `.hhc` files
- HTML-page listing when a usable table of contents is unavailable
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

### Automatic Local Saving

The application automatically saves:

- **Loaded file** — The original CHM or EPUB file is stored in IndexedDB.
- **Selected CHM article** — Article path and anchor are remembered.
- **CHM scroll position** — Resume at the saved vertical position.
- **EPUB reading position** — Resume from the saved CFI location.
- **Interface preferences** — Language, text size, and sidebar visibility.

On page reload, the saved file is reopened automatically and the reading position is restored.

Only the **most recently saved book** is retained. Opening and successfully saving another file replaces it.

> Files and reading positions are stored in the current browser and site origin—not in the cloud. Clearing site data, private browsing, or browser storage eviction can remove them. Keep your original book files.

### Reading Controls

- Adjustable text size from **60% to 220%**
- Text-size changes in 10-percentage-point steps
- Collapsible table-of-contents sidebar
- Highlighted current article or chapter
- Automatic sidebar scrolling to the restored selection
- External CHM web links opened in a new tab

### User Interface

- Ukrainian and English translations
- Dark application interface with a light reading area
- Responsive desktop and mobile layout
- Overlay sidebar on narrow screens
- Drag-and-drop file opening
- Loading and decompression progress indicators
- Toast notifications for loading, restoration, and storage errors

## ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `←` | Previous EPUB page |
| `→` | Next EPUB page |

EPUB keyboard navigation is disabled while a file is loading and when focus is in an input, textarea, or select field.

CHM articles use normal browser scrolling and link navigation.

## 🚀 Getting Started

### Online Usage

Visit **[chmepubviewer.github.io](https://chmepubviewer.github.io)**.

1. Select **Open** or drag a `.chm` or `.epub` file into the window.
2. Choose an article or chapter from the contents sidebar.
3. Adjust text size using **A−** and **A+**.
4. Switch between **UK** and **EN** as needed.
5. Reload the page to restore the saved file and reading position.

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

## 💾 Saving and Restoring Reading Sessions

### What Is Stored?

| Storage | Contents |
|---------|----------|
| IndexedDB | Original book file, filename, session identifier, and reading position |
| localStorage | Interface preferences and a small reading-position snapshot |

The application uses session identifiers to prevent delayed position updates from being applied to a different book.

### Automatic Restoration

When the page opens:

1. The application reads the saved file from IndexedDB.
2. It selects the newer matching reading-position snapshot.
3. It reopens the EPUB or decompresses the CHM.
4. It restores the selected article or chapter and reading position.

For CHM files, restoration falls back to the default topic or an available HTML page if the saved article cannot be found.

For EPUB files, an unusable saved CFI falls back to the book’s initial location.

### If Saving Fails

The book can still be read in the current session if browser storage is unavailable or full. A notification explains that automatic restoration is unavailable.

Check that:

- Site storage is enabled.
- The browser has available storage space.
- You are using the same browser profile and site address.
- Site data has not been cleared.

The application does not currently provide a saved-book library, session export, or cloud synchronization.

## 🛠️ Technologies Used

- **HTML5** — Application structure and file selection
- **CSS3** — Responsive layout, themes, transitions, and animations
- **Vanilla JavaScript** — Application logic and CHM processing
- **Custom LZX decoder** — CHM content decompression
- **[epub.js 0.3.93](https://github.com/futurepress/epub.js)** — EPUB rendering and navigation
- **[JSZip 3.10.1](https://github.com/Stuk/jszip)** — ZIP support for EPUB processing
- **IndexedDB** — Persistent storage of the original loaded file
- **localStorage** — Preferences and reading-position snapshots
- **File and Blob APIs** — File access and embedded-resource URLs
- **DOMParser** — CHM HTML and table-of-contents parsing
- **TextDecoder** — Character-encoding support
- **Sandboxed iframe** — CHM article rendering

## 🔒 Privacy and Storage

Book parsing, CHM decompression, and reading-session storage happen locally in the browser. The application code does not upload loaded books to a server or require an account.

However:

- Third-party JavaScript libraries are downloaded from jsDelivr.
- Book content that references external resources may cause network requests.
- Opening an external link navigates to the linked website.
- Local storage is subject to browser quotas and eviction policies.

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
- Large CHM files are decompressed into memory and may affect responsiveness.
- CHM decompressed content is limited to **512 MiB**.
- The LZX implementation supports specific CHM control-data and reset-table layouts; not every CHM file is guaranteed to work.
- CHM scripts, forms requiring scripts, and interactive embedded content are not supported.
- Complex CHM resource references or legacy page layouts may not render exactly as in a native viewer.
- EPUB compatibility depends on epub.js; DRM-protected books are not supported.
- The EPUB page indicator describes the current section’s displayed pages, not a fixed whole-book page count.
- CHM scroll restoration uses a pixel offset, so a changed viewport or page layout may shift the visible text.
- There is no full-text search, annotation system, bookmark collection, or reading-history library.
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
- CHM article and scroll restoration after reload
- EPUB reading-position restoration after reload
- Replacing the saved book with a different file
- Storage-unavailable and storage-full behavior
- Language switching and text-size controls
- Narrow mobile viewports

For LZX changes, include a reproducible test case or a redistributable sample file where possible.

### Ideas for Contributions

- [ ] Full-text search
- [ ] Multiple-book library
- [ ] Bookmarks and annotations
- [ ] Reading history
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
