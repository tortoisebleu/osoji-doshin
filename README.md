# Osoji Doshin (お掃除同心)

A small, single-file tool that cleans up the HTML of a [Blogger](https://www.blogger.com/) post.

Paste the post's HTML, click one button, and get tidier HTML back, together with a report that checks that no image or link was changed by accident.

- Runs **entirely in your browser**. Nothing is uploaded, and there is no server, no tracking and no API key.
- **One HTML file.** No installation, no dependencies, no network access.
- The screen is in **Japanese**. A key to the buttons is below.

> *Osoji* (お掃除) means "cleaning", and *doshin* (同心) was a patrol officer in Edo-period Japan. The tool cleans your HTML, then patrols it for problems.

## Quick start

1. Download `osoji-doshin_v4.html` and open it in a modern desktop browser.
2. In the Blogger post editor, switch to **HTML view** and copy everything (`Ctrl + C`).
3. Paste it into the left box ("元のHTML") with `Ctrl + V`, then click **掃除する** (Clean up).
4. Review the report, then click **コピー** (Copy) and paste the result back into Blogger's HTML view.

## What it does

### Cleanup (automatic)

- Removes Blogger-specific attributes from images and links (`imageanchor`, `border="0"`, `data-original-height`, `data-original-width`).
- Removes inline `style` attributes from image wrappers and image links, and `dir="ltr"` from paragraphs and headings.
- Removes leftover utility classes pasted from AI chat tools (`break-words`, `whitespace-normal`, and similar).
- Wraps every image in `<div class="separator">`.
- Adds `width` and `height` to every image, read from the size in the image URL (for example `w640-h480`).
- Puts `loading="eager"` on the first image only, and removes `loading` from the others.
- Orders image attributes consistently: `src`, `width`, `height`, `alt`, `loading`.
- Removes stray `<br>` tags between blocks, and empty `<p>` and `<div>` elements.
- Replaces `hyphenhyphen` in body text with `--`.

### Report (nothing is changed automatically)

- **Verification:** compares image `src`, link `href` and image `alt` before and after the cleanup, and tells you if anything differs.
- **Changes:** how many of each fix were applied.
- **Warnings** for things you may want to fix by hand:
  - opening and closing tag counts that do not match in the original HTML (browsers silently repair this, so it is invisible after cleanup)
  - empty, file-name-like or `hyphenhyphen` `alt` text
  - paragraphs that contain only bold text (heading candidates), and headings containing long dashes
  - `<blockquote>` elements and leading spaces
  - a half-width `,` or `.` between Japanese characters
  - duplicate `id` values, and `#anchor` links with no matching `id`

### Working with an AI assistant

Blogger image URLs are very long and appear twice per image, so they take up most of the text of a post. The **Claude用にコピー** (Copy for Claude) button shortens the shared part of each image URL to a token such as `{{U1}}` and puts a short report in front of the HTML. This makes the text much smaller to paste into an AI chat. After the assistant sends HTML back, paste it into the restore box and click **URLを復元** (Restore URLs) to put the original URLs back.

### Other conveniences

- Optional automatic cleanup when you paste. A paste always replaces the previous contents, so articles never get mixed.
- **次の記事へ（リセット）** (Next article / Reset) clears everything for the next post.
- **ファイルに保存** (Save as file) saves `<article-name>_v1.html`, `_v2.html` and so on. The counter is kept in the browser, so earlier files are never overwritten.

## Key to the Japanese interface

| Japanese | Meaning |
|---|---|
| 記事名 | Article name (used for the saved file name) |
| 元のHTML | Original HTML (input) |
| 掃除する | Clean up |
| 貼り付けて掃除 | Paste from the clipboard and clean up in one click |
| 掃除後のHTML | Cleaned HTML (output) |
| コピー | Copy |
| Claude用にコピー | Copy with shortened image URLs and a report |
| ファイルに保存 | Save as file |
| 検証 / 掃除した内容 / 警告 | Verification / Changes made / Warnings |
| URLを復元 | Restore the shortened image URLs |
| 次の記事へ（リセット） | Next article (reset everything) |

## Notes and limitations

- **Always check the result before publishing, and keep a copy of the original post.** The tool is careful, but it does not understand the content of your post.
- It is built for **Blogger** HTML. Some warnings assume **Japanese** text (the dash and half-width punctuation checks) and reflect the author's own style.
- URL shortening depends on the current format of Blogger image URLs (`blogger.googleusercontent.com/img/...`). If Blogger changes the format, images are left as they are.
- The list of shortened URLs for the restore step is kept in your browser's local storage, for the most recent article only. Cleaning another article, or clearing the browser's site data, makes the previous restore impossible. Cleaning the original HTML again recreates the list.
- The "Paste and clean up" button reads your clipboard, so your browser may ask for permission. If it does not work, paste with `Ctrl + V` instead.
- Not affiliated with Google. Blogger is a trademark of Google LLC.

## License

[MIT](LICENSE)
