# Rana Travels Baijnath — Implementation Plan

## Product scope
Create an English-language public website for Rana Travels Baijnath using only agency facts verifiable on the named public Instagram profile (`@ranatravels_`) and the Rana Travels Baijnath Facebook page. Public agency facts include its Baijnath, Himachal Pradesh identity, bus service, profile links, and contact details explicitly listed by those sources. Keep the public routes directory empty until an administrator creates and publishes entries: do not seed, infer, or advertise any route, timetable, or fare. Provide a separate, protected admin area to log in and add, edit, publish/unpublish, and delete locally supplied route entries. Persist routes in the project's managed database. Keep administrator credentials out of source, Git, the UI, and logs; load them from protected runtime secrets. Use only verified contact channels for enquiry actions. This task prepares a preview; it does not authorize broad public publication.

## Implementation architecture
- Retain the initialized React, TypeScript, Tailwind, Express, tRPC, Drizzle, MySQL, and Vite project.
- Public React view: editorial home page, agency details/contact block, verified social links, and an explicitly empty-until-populated route directory.
- Hero image: use the attached Rana-branded bus photo at `/manus-storage/images(3)_899ce5b4.jpg` as the full-bleed responsive background, with a dark text-contrast overlay; retain the source image unmodified in project storage.
- Admin React view: `/admin` renders a login form while signed out and route manager after authentication. Route form uses administrator-supplied origin, destination, optional timing, fare, notes, and availability; all fields except the route name/places remain optional, and public cards only show supplied information.
- Server: dedicated Express login/logout/session and route endpoints; compare login values only against protected environment secrets. Use an opaque random session token with only its hash stored, an HttpOnly session cookie, secure cross-site Preview attributes (`SameSite=None; Secure`), expiry, and a stable double-submit CSRF token for both login and route mutations. Public reads return only published routes; admin endpoints manage all route records.
- Data: additive Drizzle migration and schema for routes and hashed, expiring admin sessions. Do not add seeded route or timetable rows.
- Source facts: Instagram `https://www.instagram.com/ranatravels_/`; Facebook public page `https://www.facebook.com/p/RAnA-BuS-Transport-BaijNath-100082062084095/`. The public profiles identify the bus service with Baijnath, Himachal Pradesh. Instagram lists +91 91293 00044 and +91 91293 00022; Facebook lists +91 91293 00022 and +91 70189 99097, plus `info@ranatransport.com`. Use click-to-call and email actions; do not imply WhatsApp availability. Include only these source-listed contact items and link to those social profiles.

## Design direction
- **Design movement:** Himalayan road-journal editorial, pairing the tactile clarity of a local transit notice with the atmosphere of a mountain travel feature.
- **Core principles:** grounded in the region; warm and legible; routes are never assumed; useful actions remain obvious.
- **Color philosophy:** deep pine and ink signal reliability; warm ivory feels like paper; a vivid marigold-orange nods to sunlit journeys and acts as an ownable accent.
- **Layout paradigm:** a left-aligned editorial masthead and full-bleed photo hero, then a horizontal journey-line divider and generously spaced, staggered content bands rather than a generic centered tile grid.
- **Signature elements:** the user's circular Rana Tour & Travels badge used as the official mark; fine route-line strokes used as dividers only (never as actual route maps); compact status labels for route publication.
- **Interaction philosophy:** direct, plain-language actions; no fake booking flow or claims; route cards appear only after the admin publishes verified details.
- **Animation:** restrained fade/slide entrance for the hero and section reveals, small accent-line transitions on links, reduced-motion support; no looping decorative motion.
- **Typography:** editorial display face (Fraunces) for major headings paired with Manrope for navigation, forms, labels, and body copy; responsive type scale and high contrast.
- **Brand essence:** a Baijnath-based bus-service presence where local route information is added directly by its administrator; dependable, local, straightforward.
- **Brand voice:** specific and neighborly; explain what's known without overselling. Examples: “A local bus service from Baijnath.” “Route details appear here when Rana Travels posts them.”
- **Wordmark & logo:** use the supplied Rana Tour & Travels logo in the public header/footer, admin area, favicon, and project metadata, with readable site-name text beside the small header mark.
- **Signature brand color:** marigold amber `#E1A233`.

## Project structure
- `client/src/App.tsx`: public pages, route display, and `/admin` login/manager views.
- `client/src/index.css`: responsive editorial design system and reduced-motion rules.
- `client/public/`: page-route manifest; favicon and the supplied logo resolve from durable project storage.
- `server/admin.ts`: dedicated login, CSRF, route CRUD, publication, and logout endpoints.
- `server/_core/index.ts`: register the admin endpoints alongside the existing application middleware and tRPC service.
- `server/db.ts`: Drizzle queries for public route reads, admin route mutations, and session lifecycle.
- `drizzle/schema.ts` plus `drizzle/0001_*.sql`: route and session tables; no route seed data.
- `TODO.md`: remaining product outcomes and their original request clauses.

## Explicit limits
No inferred route listings, schedules, fares, WhatsApp, fleet specifications, or promises of service. No public deployment without a distinct request. The admin login values must be configured using protected secrets input, not copied into the repository.
