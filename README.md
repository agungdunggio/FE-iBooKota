# iBooKota

Internal meeting-room booking landing page.

Employees book rooms through WhatsApp. iBooKota checks availability, confirms the booking, and adds it to Google Calendar.

## For users

1. Open the landing page.
2. Select **Book via WhatsApp**.
3. Send the meeting time and requirements.
4. Confirm an available room with iBooKota.
5. Check the confirmed booking in Google Calendar.

## Project contents

| File | Purpose |
| --- | --- |
| `index.html` | Landing-page content and semantic markup |
| `styles.css` | Responsive visual design and accessibility states |
| `PRD.md` | Product requirements and launch scope |
| `DESIGN.md` | Visual design rules and tokens |
| `docs/` | Product design and implementation plans |

## Run locally

No dependencies or build step required.

```bash
python3 -m http.server 8000
```

Open <http://localhost:8000>.

## Change the WhatsApp number

Update every booking CTA in `index.html`:

```html
<a href="https://wa.me/6281234567890">Book via WhatsApp</a>
```

Keep all CTA URLs identical. Use an approved organization WhatsApp number before deployment.

## Deploy

This is a static site. Deploy the repository with any static hosting provider, such as:

- GitHub Pages
- Netlify
- Vercel
- Cloudflare Pages

Publish `index.html` and `styles.css` from the repository root. No server-side runtime is required for the landing page.

## Scope

Included:

- Employee-focused landing page
- WhatsApp-first booking CTA
- AI availability and booking explanation
- Database and Google Calendar sync explanation
- Example booking conversation
- FAQ

Not included in this repository:

- Booking backend
- WhatsApp bot or AI service
- Database
- Google Calendar integration
- Admin dashboard
- Usage analytics
- Web booking form

The landing page explains the booking flow. WhatsApp, AI, database, and Google Calendar services must be configured separately.

## Before launch

- Replace the WhatsApp number if the organization uses a different booking contact.
- Test both page CTAs on mobile and desktop.
- Verify the WhatsApp flow, room availability, database record, and Google Calendar event.
- Check keyboard navigation and FAQ interaction.
- Confirm deployed files load from the site root.
