# Zweli Group website (working draft)

A static, no-build website for three businesses under one shared name:

- **Zwilicious Food** — catering (`food.html`)
- **Zweli Travel Agency** — travel (`travel.html`)
- **Zweli Solar Power** — solar (`solar.html`)
- Shared **Home** (`index.html`) and **Contact** (`contact.html`) pages

It's plain HTML, CSS and JavaScript — no framework, no build step, no backend,
no database, and no paid services. It can be hosted for free on GitHub Pages
(or any static host).

> **This is a working draft.** Every business fact, menu item, package,
> price, specification and photo on the site is either marked "pending
> confirmation" or is placeholder artwork. Nothing is invented as if it
> were real. See **"What still needs the owner's input"** near the bottom
> of this file before treating the site as ready to launch.

---

## 1. Preview the site on your own computer

No installation is required — it's plain HTML/CSS/JS.

**Easiest option:** double-click `index.html` and it will open in your
browser. Every page and form works this way, since the site has no server
component. (One small limitation: browsers sometimes block loading Google
Fonts from a `file://` page depending on your settings — the site still
works perfectly, it just falls back to a system serif/sans-serif font.)

**Recommended option (matches how it will behave once hosted):** run a
tiny local web server from inside the project folder, for example:

```bash
# Python 3 (already installed on most Mac/Linux machines)
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

If you use VS Code, the "Live Server" extension also works well.

---

## 2. Project structure

```
zweli-group/
├── index.html              Home page
├── food.html                Zwilicious Food page + catering enquiry form
├── travel.html               Zweli Travel Agency page + travel enquiry form
├── solar.html                 Zweli Solar Power page + solar enquiry form
├── contact.html                Contact page (links to all 3 forms + WhatsApp)
├── css/
│   └── styles.css            All styling for every page (one shared file)
├── js/
│   ├── config.js              ← Edit this file to change the WhatsApp number
│   │                             or the working group name
│   └── main.js                Mobile menu, form validation, WhatsApp message
│                                building — shared by every page
├── assets/
│   └── images/
│       ├── brand/              Home/Contact/shared artwork + favicon
│       ├── food/                Food gallery + hero placeholder artwork
│       ├── travel/               Travel placeholder artwork
│       └── solar/                 Solar placeholder artwork
├── README.md                  This file
└── NEEDS-CONFIRMATION.md       Checklist of real business info still needed
```

---

## 3. Updating the WhatsApp number or the group name

Everything reads from **one file**: `js/config.js`.

```js
window.ZWELI_CONFIG = {
  groupName: "Zweli Group",           // shown in the header & footer
  whatsappNumber: "27614608400",      // digits only, no "+", no spaces
  whatsappDisplay: "+27 61 460 8400"  // how it's shown to visitors
};
```

To change the WhatsApp number, edit `whatsappNumber` (and `whatsappDisplay`
to match) in this one file — every page, every button, and every enquiry
form will use the new number automatically. You do not need to edit any
HTML page for this.

---

## 4. Replacing placeholder images with real photos

No real business photos were supplied with this project, so every image on
the site is **original placeholder artwork** (simple line-art icons on a
tinted background), not a real photo of any dish, event, property, or
person. Every placeholder is visibly labelled "Placeholder" on the page
itself and in its `alt` text, and the draft banner at the top of each page
links back to this file as a reminder.

To swap in a real, approved photo:

1. Add your image file into the matching folder in `assets/images/…`
   (e.g. a real buffet photo goes in `assets/images/food/`).
2. Open the page's HTML file and find the `<img>` tag that currently points
   at the placeholder `.svg` file for that spot (e.g.
   `assets/images/food/buffet-service.svg`).
3. Change the `src` to your new file, and update the `alt` text to describe
   the real photo. For example:

   ```html
   <!-- before -->
   <img src="assets/images/food/buffet-service.svg" alt="Placeholder image for buffet service at an event" loading="lazy">
   <span class="placeholder-tag">Placeholder</span>

   <!-- after -->
   <img src="assets/images/food/buffet-2026-oct-wedding.jpg" alt="Buffet table set up at a wedding reception in Johannesburg" loading="lazy">
   <!-- delete the <span class="placeholder-tag">…</span> line once the photo is approved -->
   ```

4. Delete the `<span class="placeholder-tag">Placeholder</span>` line right
   after the image once a real, approved photo is in place, so the
   "Placeholder" tag stops showing on the live site.
5. For best results and fast page loads, keep photos under ~400 KB each
   (export at roughly 1600px on the long edge, JPEG quality ~75–80%).

**Do not** publish AI-generated or stock food/event photos as if they were
confirmed Zwilicious Food menu items, Zweli Travel destinations, or Zweli
Solar installations — only real, approved photos and confirmed details
should replace the placeholders.

---

## 5. Editing menus, packages, prices and other business details

Every "service" or "category" list on the Food, Travel and Solar pages
(look for the amber **"Pending confirmation"** tags) is a placeholder
category, not a confirmed offering. To confirm one:

1. Open the relevant page's HTML file (`food.html`, `travel.html`, or
   `solar.html`) and find the `.service-row` block for that item.
2. Edit its heading and description text to match the real, confirmed
   service.
3. Change:
   ```html
   <span class="status-pill pending">Pending confirmation</span>
   ```
   to:
   ```html
   <span class="status-pill confirmed">Confirmed</span>
   ```

Do the same for any dish names, destinations, or specifications you want
to publish — only add specific menu items, packages, prices, travel
destinations, or technical specifications once the business owner has
confirmed them. The site intentionally avoids inventing any of these.

---

## 6. Testing the enquiry forms

Each of the three enquiry forms (Food, Travel, Solar) works the same way:

1. Fill in the form and press **"Review my enquiry."**
2. Required fields are checked; if something's missing, that field is
   outlined in red with a short error message, and the message won't send.
3. Once everything required is filled in, a summary window opens showing
   exactly what will be sent.
4. Pressing **"Continue to WhatsApp"** opens WhatsApp (in a new tab/app)
   with the message already typed in. Nothing is sent until the customer
   also presses **Send inside WhatsApp** — the website never sends
   messages on its own.
5. **"Go back & edit"** closes the summary without sending anything, so the
   customer can fix details first.

To manually test a flow: fill in a form with sample details (see the
suggested examples in `NEEDS-CONFIRMATION.md`), open the summary, and
confirm every field you typed appears correctly in it and in the WhatsApp
message text before pressing Send.

If you're comfortable with the command line and Node.js is installed, you
can also sanity-check that the WhatsApp text encodes and decodes back to
the exact same message with:

```js
const msg = "some multi-line test message";
console.log(decodeURIComponent(encodeURIComponent(msg)) === msg); // should print true
```

---

## 7. Publishing to GitHub Pages

This site has no build step, so publishing is simple:

1. Create a new GitHub repository (e.g. `zweli-group`) and push this whole
   folder's contents to its `main` branch (the `index.html` file should sit
   at the **root** of the repository, not inside a subfolder).
2. In the repository, go to **Settings → Pages**.
3. Under **"Build and deployment"**, set **Source** to **"Deploy from a
   branch"**, choose branch **`main`** and folder **`/ (root)`**, then save.
4. GitHub will publish the site at:
   - `https://<your-github-username>.github.io/<repository-name>/`
     if the repository is a normal project repository, or
   - `https://<your-github-username>.github.io/`
     if the repository is named exactly `<your-github-username>.github.io`.
5. Because every link in this site is a **relative link** (e.g.
   `href="food.html"`, `src="assets/images/…"`, `href="css/styles.css"`)
   rather than an absolute path starting with `/`, it works correctly at
   either of the URLs above with **no extra "base path" configuration
   needed**. If you ever rename the repository, the site will keep working
   without any code changes.
6. It can take a minute or two for GitHub Pages to publish after your first
   push. Refresh the Pages settings screen to see the live link once it's
   ready.

To update the live site later, just push new commits to `main` — GitHub
Pages will redeploy automatically.

---

## 8. Accessibility & performance notes

- All interactive controls (menu, forms, summary window) work with a
  keyboard: <kbd>Tab</kbd> to move between fields, <kbd>Enter</kbd>/<kbd>Space</kbd>
  to activate buttons, and <kbd>Esc</kbd> to close the mobile menu or the
  summary window.
- Every form field has a visible, linked `<label>`; every image has
  descriptive `alt` text; every page has a unique `<title>` and a short
  meta description for search engines.
- Text/background colour combinations across the site were checked against
  the WCAG AA contrast guideline (4.5:1 for normal text).
- Placeholder images are lightweight SVG files (a few KB each), so the
  site loads quickly. Once you replace them with real photos, keep photos
  reasonably compressed (see section 4) so pages keep loading fast on
  mobile data.
- The layout was tested at mobile (390px), tablet (1024px) and desktop
  (1440px) widths with no horizontal scrolling or overlapping content.

---

## 9. What still needs the owner's input

See **`NEEDS-CONFIRMATION.md`** for the full checklist of real business
details, menu information and photography this site needs before it can
be called finished and launched.
