# Muawé Healing Arts website

A one-page website for Muawé Healing Arts, an art psychotherapy practice in Manchester City Centre. About, services with prices, art for sale, reviews, journal, location and a booking form.

The whole site is one file of plain HTML, CSS and JavaScript. There is nothing to build.

## Run it

You need Node 18 or newer.

```
node serve.mjs
```

Then open http://localhost:3000.

## What is in here

| Path | What it is |
| --- | --- |
| `index.html` | The whole site: markup, styles and scripts |
| `images/` | Paintings, logo, journal and service images |
| `emails/reservation-confirmation.html` | The booking confirmation email, with `{{placeholders}}` for Zapier |
| `brand_assets/` | Logo, brand guidelines and screenshots of the previous site |
| `serve.mjs` | A small local web server |

## How booking works

The booking form sends each request to a Zapier webhook. The address is the `WEBHOOK` constant in `index.html`. Zapier runs the reservation flow and sends the confirmation email.

Fields sent: `name`, `email`, `booking_type`, `service`, `preferred_date` (YYYY-MM-DD), `preferred_time` (HH:MM, 24-hour), `message`, `source`, `page`, `submitted_at`.

The form shows "Thank you. Your booking is confirmed." only after Zapier replies with success. If the request fails it shows an error and the practice's email address instead. A hidden field and a short delay after page load keep simple bots from sending bookings.

Because this repository is public, the webhook address is visible to anyone. Add a filter step at the start of the Zap (for example, only continue if the email looks real) so junk requests do not start the flow.

The email template needs a Formatter step in the Zap to turn the date and time into friendly text; see the comments in the template.

## Notes

- Fonts load from Google Fonts: Playfair Display, Montserrat and Antonio.
- The honey-coloured hero background, the About section's moving light (WebGL) and the floating paintings are drawn in the browser. They switch off for visitors who prefer reduced motion.
- Opening hours used by the booking form are 9am to 8pm, in 15-minute slots.

## Licence

All rights reserved. The text, images and brand assets belong to Muawé Healing Arts and the artists.
