# Lumen Archive · 金光藏經閣

A personal PDF + EPUB reader and library that lives entirely in GitHub. Your books, highlights, notes, tags, bookmarks and reading progress are all stored as plain files in a repository you own.

## What it does

- **Library**: lists every `.pdf` and `.epub` in your `books/` folder. Subfolders become shelves. You can search, filter by type, and see recent books first.
- **Reader**: PDFs (via pdf.js) with selectable text and zoom. EPUBs (via epub.js) in page or scroll layout with adjustable text size. Ivory, Paper and Gold Night page themes.
- **Annotations**: select text to highlight or underline it in 5 colours. Each highlight can have a long-form note and tags. You can also add page notes (no selection needed) and bookmarks.
- **Notes panel**: filter by colour or text, tap any note to jump to it, and export a book's notes to Markdown. The Contents tab shows the EPUB table of contents or PDF outline.
- **Notebook**: search every note across all books, filter by #tag, and export everything to one Markdown file.
- **Sync**: notes are saved to `annotations/<book>.json` in your repo a few seconds after each change. They are saved on the device first, so nothing is lost offline. Opened books are cached for offline reading.

## Setup (about 5 minutes)

### 1. Host the app (public repo, free GitHub Pages)
1. Create a new **public** repo, e.g. `lumen-archive`.
2. Upload everything in this folder: `index.html`, `manifest.webmanifest`, `sw.js`, `.nojekyll` and the `icons/` folder.
3. Go to **Settings → Pages**, set Source to *Deploy from a branch*, choose `main` and `/ (root)`, then save.
4. After a minute your reader is at `https://<username>.github.io/lumen-archive/`.

### 2. Make a library repo (private recommended)
1. Create a **private** repo, e.g. `my-library`.
2. Add a `books/` folder. You can upload through the app later, or drag files in on github.com.
   Use subfolders for shelves: `books/Philosophy/Meditations.epub`.

Books you bought are usually copyrighted, so keep them in a **private** repo. Free GitHub Pages only works for public repos, which is why the app and the books are in separate repos.

### 3. Create a token
1. Go to GitHub → **Settings → Developer settings → Fine-grained personal access tokens → Generate new token**.
2. Under **Repository access**, choose *Only select repositories* and pick `my-library`.
3. Under **Permissions → Repository permissions**, set **Contents** to *Read and write*.
4. Generate the token and copy it.

### 4. Connect
Open your reader, tap **Connect GitHub**, and enter:
- Repository: `username/my-library`
- Branch: `main`
- Token: the one you just made

Tap **Test** to check the connection, then **Save**.

## Install it as an app
Lumen Archive is a Progressive Web App (PWA). Once it's installed, it opens full screen with its own icon and works offline for any book you've opened before.
- **Android / Huawei / Chrome / Edge**: tap the phone icon in the top bar (or **Settings → Install app**). If you don't see it, use your browser menu → **Add to home screen** / **Install app**.
- **iPhone / iPad (Safari)**: tap Share → **Add to Home Screen**.
- **Desktop Chrome / Edge**: use the install icon in the address bar.

Want a real Android APK (for example, to sideload on a Huawei device)? Paste your Pages URL into [pwabuilder.com](https://www.pwabuilder.com), choose **Android**, and download the package it builds.

When you update `index.html` on GitHub, the installed app picks up the new version the next time it opens while online.

## How notes are stored

`annotations/Philosophy__Meditations.epub.json`
```json
{
  "book": "books/Philosophy/Meditations.epub",
  "progress": { "cfi": "epubcfi(...)", "pct": 0.42, "t": 1764000000000 },
  "annotations": [
    {
      "id": "m1x2y3", "type": "highlight", "kind": "epub", "style": "highlight",
      "color": "#f6c855", "text": "the quoted passage", "note": "my thoughts",
      "tags": ["stoicism"], "label": "Book IV", "loc": { "cfi": "epubcfi(...)" },
      "created": 1764000000000, "updated": 1764000000000
    }
  ]
}
```
PDF highlights store `loc.page` and `loc.rects` as fractions of the page size, so they stay aligned at any zoom level. Deleted notes are kept as small `"deleted": true` stubs so they don't come back when syncing from another device.

## Limits and notes
- GitHub blocks files over **100 MB** and warns over 50 MB. Repos work best under a few GB in total.
- Each note save is one commit (they're batched a few seconds after you stop editing).
- The token is stored only in your browser's local storage. Use a fine-grained token scoped to the one library repo.
- You can also open a file with **Just read here** without uploading it. Notes for those files stay on that device only.
- You can change the look in the `<style>` block at the top of `index.html`. The colours are CSS variables in `:root`.
