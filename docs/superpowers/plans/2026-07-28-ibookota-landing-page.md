# iBooKota Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a fast, accessible, employee-focused static landing page that drives meeting-room bookings through WhatsApp.

**Architecture:** Use a dependency-free static page: one semantic HTML document, one stylesheet, and no custom JavaScript. Native `<details>` elements provide the FAQ interaction; every booking CTA points to one approved WhatsApp contact URL. No backend or build step belongs in this MVP.

**Tech Stack:** HTML5, CSS3, system font stack, native browser controls, Python standard-library HTTP server for local verification.

## Global Constraints

- Primary audience: employees booking meeting rooms during the workday.
- Primary CTA: `Book via WhatsApp`; it opens the organization’s WhatsApp booking contact.
- Required content: hero, three-step flow, example conversation, benefits, integration reassurance, FAQ, and final CTA.
- Product claims: AI checks availability, confirms bookings, records them in the database, and syncs Google Calendar.
- Tone: understated, clear, calm, useful; short sentences and plain workplace language.
- Exclude admin dashboard, utilization analytics, room recommendations, web booking, pricing, and public signup messaging.
- Accessibility basics are required: semantic landmarks, one page title, visible keyboard focus, sufficient color contrast, descriptive link text, and mobile readability.
- Keep the page dependency-free; do not add a package manager, framework, icon library, analytics, or backend for this MVP.
- The approved WhatsApp booking contact URL is a launch input. Use the exact same approved URL for every booking CTA; verify the destinations match before launch.

---

### Task 1: Create the semantic landing-page document

**Files:**
- Create: `index.html`

**Interfaces:**
- Produces: A browser-renderable page with `header`, `main`, and `footer` landmarks; all copy and CTA targets required by the PRD.
- Consumes: The approved WhatsApp booking contact URL supplied at implementation time.

- [ ] **Step 1: Create the document shell and metadata**

Add an HTML5 document with:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="Book an internal meeting room through one WhatsApp conversation with iBooKota.">
  <title>iBooKota — Book a meeting room in WhatsApp</title>
  <link rel="stylesheet" href="styles.css">
</head>
```

Use one visible `<h1>` only. Keep the page copy in the HTML so it remains searchable and usable without JavaScript.

- [ ] **Step 2: Add the header and hero**

Create a header with a text wordmark `iBooKota` and a main navigation link to the process section. Create the hero with:

```html
<section class="hero" aria-labelledby="hero-title">
  <p class="eyebrow">Internal meeting-room booking</p>
  <h1 id="hero-title">Book a meeting room in one WhatsApp conversation.</h1>
  <p>Ask iBooKota what’s available. It checks the schedule, confirms your booking, and syncs it automatically.</p>
  <a class="button button-primary" href="APPROVED_WHATSAPP_BOOKING_URL">Book via WhatsApp</a>
</section>
```

Replace `APPROVED_WHATSAPP_BOOKING_URL` with the organization’s real URL before launch. The CTA must be a normal link so it works without JavaScript.

- [ ] **Step 3: Add the three-step flow**

Add a section with `id="how-it-works"`, heading `How it works`, and three ordered steps titled exactly `Ask`, `Check`, and `Confirm`. Use an `<ol>` because the steps are sequential. Include the PRD copy: describe sending a WhatsApp message, AI finding available rooms and times, then recording and syncing the booking with Google Calendar.

- [ ] **Step 4: Add the example conversation**

Add a section titled `A booking conversation` containing four visually distinct but text-based messages:

```html
<ol class="conversation" aria-label="Example iBooKota booking conversation">
  <li><span>Employee</span><p>I need a room for six people tomorrow at 2 PM.</p></li>
  <li><span>iBooKota</span><p>Room Merapi is available from 2–3 PM. Should I book it?</p></li>
  <li><span>Employee</span><p>Yes.</p></li>
  <li><span>iBooKota</span><p>Booked. The event is on Google Calendar.</p></li>
</ol>
```

Do not claim features not in the PRD, such as equipment matching or room recommendations.

- [ ] **Step 5: Add benefits and integration reassurance**

Add a benefits section titled `Why iBooKota` with four concise items: no new booking system to learn, natural-language questions, fewer scheduling conflicts, and up-to-date booking records.

Add an integrations section titled `Everything stays in sync` with the four statements from the PRD: WhatsApp for conversation, AI for understanding, database for reliable records, and Google Calendar for shared visibility. Render the systems as plain text labels, not fake vendor badges or unverified technical detail.

- [ ] **Step 6: Add FAQ and final CTA**

Use native `<details>` / `<summary>` elements for the five FAQ questions from the PRD. Answers must stay within launch promises:

- iBooKota is for employees with access to the internal booking contact.
- Booking starts by sending a WhatsApp message.
- Users may ask which rooms are available.
- Confirmed bookings appear in Google Calendar and are recorded in the database.
- If no room is available, iBooKota should say so and the employee can ask for another time.

Add a final CTA section titled `Ready to book a room?` with another `Book via WhatsApp` link using the same approved URL. Add a short footer identifying iBooKota as an internal meeting-room booking assistant.

- [ ] **Step 7: Run a static-content check**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
from html.parser import HTMLParser

class Parser(HTMLParser):
    def __init__(self):
        super().__init__()
        self.ids = set()
        self.links = []
        self.h1_count = 0
    def handle_starttag(self, tag, attrs):
        attrs = dict(attrs)
        if "id" in attrs:
            self.ids.add(attrs["id"])
        if tag == "a":
            self.links.append(attrs.get("href", ""))
        if tag == "h1":
            self.h1_count += 1

p = Parser()
p.feed(Path("index.html").read_text())
assert p.h1_count == 1
assert "how-it-works" in p.ids
assert p.links.count("APPROVED_WHATSAPP_BOOKING_URL") == 0
assert sum("wa.me" in link or "whatsapp" in link for link in p.links) >= 2
assert "Book via WhatsApp" in Path("index.html").read_text()
print("static content checks passed")
PY
```

Expected: `static content checks passed`. If the approved URL is not yet available, stop before launch rather than shipping a dead CTA; the implementation may temporarily use a documented launch input, but the final check must see the real WhatsApp URL.

- [ ] **Step 8: Commit the semantic page**

This workspace currently has no Git repository. If Git is initialized before implementation, commit with:

```bash
git add index.html
git commit -m "feat: add iBooKota landing page structure"
```

---

### Task 2: Implement the responsive visual system

**Files:**
- Create: `styles.css`
- Modify: `index.html` only if a class or landmark needed by the stylesheet is missing

**Interfaces:**
- Consumes: The semantic class names from `index.html`.
- Produces: Responsive, readable presentation from 320px mobile widths through desktop widths without changing content or CTA behavior.

- [ ] **Step 1: Add tokens and base styles**

Use a small token set and system fonts; no external font request is needed:

```css
:root {
  --ink: #17211b;
  --muted: #5e6a62;
  --paper: #f7f8f5;
  --surface: #ffffff;
  --line: #dce3dc;
  --accent: #16794b;
  --accent-dark: #0f5c38;
  --max-width: 72rem;
  --radius: 1.25rem;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  color: var(--ink);
  background: var(--paper);
}

* { box-sizing: border-box; }
html { scroll-behavior: smooth; }
body { margin: 0; line-height: 1.6; }
a { color: inherit; }
```

Use restrained green only for action and product emphasis; keep body text dark on light surfaces. Do not use gradients, animation-heavy decoration, or stock-image dependencies.

- [ ] **Step 2: Style layout and typography**

Center content in a `.container` capped at `var(--max-width)`, use generous vertical section spacing, and establish a clear type scale: compact eyebrow, responsive h1 using `clamp()`, readable h2, and body text capped for line length. Use CSS Grid for the three steps and benefits; let them collapse to one column below the mobile breakpoint.

- [ ] **Step 3: Style CTA and content surfaces**

Make `.button` a high-contrast inline-flex control with a minimum 44px touch target, visible hover state, and a `:focus-visible` outline. Use plain cards or bordered surfaces for steps, conversation messages, and integration items. Keep borders and spacing as the visual hierarchy; avoid decorative icon packages.

- [ ] **Step 4: Add responsive and reduced-motion behavior**

At narrow widths, stack header content, hero content, steps, and benefit items. Ensure the CTA stays full-width or comfortably tappable without horizontal overflow. Add:

```css
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
  *, *::before, *::after {
    animation-duration: 0.01ms;
    animation-iteration-count: 1;
    transition-duration: 0.01ms;
  }
}
```

- [ ] **Step 5: Verify manually in a local browser**

Run:

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000/`. Check 320px and desktop widths. Confirm no horizontal scroll, all sections are visible, FAQ opens natively, and both CTA locations point to the same approved WhatsApp URL.

- [ ] **Step 6: Run the static checks again**

Run the Task 1 parser check and add this CSS sanity check:

```bash
python3 - <<'PY'
from pathlib import Path
css = Path("styles.css").read_text()
assert "prefers-reduced-motion" in css
assert ":focus-visible" in css
assert "@media" in css
print("responsive accessibility checks passed")
PY
```

Expected: `responsive accessibility checks passed`.

- [ ] **Step 7: Commit the visual system**

If Git is initialized before implementation, commit with:

```bash
git add index.html styles.css
git commit -m "style: design responsive iBooKota landing page"
```

---

### Task 3: Perform launch-readiness verification

**Files:**
- Modify: `index.html` if verification finds copy, URL, or accessibility defects
- Modify: `styles.css` if verification finds responsive or focus defects

**Interfaces:**
- Consumes: Completed static page and stylesheet.
- Produces: A launch-ready static page satisfying the approved PRD and its success criteria.

- [ ] **Step 1: Verify required content mechanically**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
html = Path("index.html").read_text()
required = [
    "Book a meeting room in one WhatsApp conversation.",
    "How it works", "Ask", "Check", "Confirm",
    "Why iBooKota", "Google Calendar", "database",
    "Who can use iBooKota?", "What happens if no room is available?",
    "Book via WhatsApp",
]
for text in required:
    assert text in html, text
assert html.count("Book via WhatsApp") >= 2
print("PRD content checks passed")
PY
```

Expected: `PRD content checks passed`.

- [ ] **Step 2: Verify links and headings**

Run:

```bash
python3 - <<'PY'
from html.parser import HTMLParser
from pathlib import Path

class Check(HTMLParser):
    def __init__(self):
        super().__init__(); self.headings = []; self.links = []
    def handle_starttag(self, tag, attrs):
        attrs = dict(attrs)
        if tag in {"h1", "h2"}: self.headings.append((tag, attrs.get("id")))
        if tag == "a": self.links.append(attrs.get("href", ""))

c = Check(); c.feed(Path("index.html").read_text())
assert sum(tag == "h1" for tag, _ in c.headings) == 1
assert len(c.links) >= 2
assert all(link.startswith("https://wa.me/") or link.startswith("https://api.whatsapp.com/") or link.startswith("#") for link in c.links)
print("heading and link checks passed")
PY
```

Expected: `heading and link checks passed`.

- [ ] **Step 3: Exercise the page through the local server**

Run `python3 -m http.server 8000`, load the page, and exercise:

1. Activate the hero CTA with keyboard; it must expose the WhatsApp destination.
2. Tab through the page; focus must remain visible.
3. Open every FAQ item; each summary must reveal an answer.
4. Resize from 320px to desktop; content must remain readable with no horizontal overflow.
5. Activate the final CTA; it must use the same WhatsApp destination as the hero CTA.

- [ ] **Step 4: Review scope and copy**

Search the final page for excluded or unsupported promises:

```bash
grep -Ein "analytics|dashboard|pricing|signup|equipment|location recommendation|revolutionary|game-changing" index.html && exit 1 || true
```

Expected: no unsupported product claims or excluded MVP positioning.

- [ ] **Step 5: Fix defects, rerun all checks, and record completion**

Any failed check is a blocker. Correct the smallest responsible file, rerun Tasks 1–3 checks, then report the exact commands and results. Do not add placeholder CTAs, skipped tests, or unimplemented interactive behavior.

- [ ] **Step 6: Commit the verified page**

If Git is initialized before implementation, commit with:

```bash
git add index.html styles.css
git commit -m "chore: verify iBooKota landing page"
```
