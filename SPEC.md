# Haven product and feature specification

This file contains cross-project product behavior and the current scope of each client. Stable architecture and constraints belong in `AGENTS.md`; setup belongs in `README.md`.

## Shared product behavior

Haven helps people discover homes for sale or rent in Yangon, Mandalay, and Bago. Visitors can browse without an account. Account holders can submit and manage listings, and administrators review submissions before they become public.

### Listings and discovery

- A listing has a title, description, purpose (`sale` or `rent`), property type, city, price, currency, and one or more photos before publication.
- Supported cities are Yangon, Mandalay, and Bago. Property types are House, Condo, Apartment, Land, and Commercial.
- Optional details include township, rental period, bedrooms, bathrooms, area, and area unit.
- Public discovery shows only approved, published listings. New submissions and edits requiring review remain private until approved.
- Public search supports location, purpose, property type, price range, bedroom count, pagination, and stable sorting.
- Demo content is fictional and intended for local development. Do not put real credentials or personal contact information in source control.

### Accounts and permissions

- Visitors can browse without signing in. Listing management requires an account.
- Owners can view and manage their own listings. Administrators can review and remove any listing.
- Passwords are never stored or returned in plaintext. Private account fields and credentials are not included in public listing responses.
- Ownership and administrator permissions are enforced by the API.

## Web client (`app/`)

- The home page introduces Haven and provides search by purpose, city, and budget.
- Visitors can filter listings by sale/rent and city, sort by newest or price, and open details without signing in.
- Cards show the primary photo, location, title, price, and available property facts. Detail pages show the gallery, description, facts, and owner-provided contact method.
- Show loading and empty states. Keep illustrative listings available when the API returns no results or is unavailable.
- Account holders can register, sign in, create/edit/remove listings, and see submission/review state. Administrators can approve, reject, and remove listings.
- The header light/dark toggle persists the user's choice and uses the operating system preference when no choice is saved.
- Support narrow mobile browsers and desktop layouts, with keyboard and touch access to navigation, search, cards, forms, and theme controls.

## API (`api/`)

- `GET /api/v1/listings` returns approved, published listings and supports city, purpose, property type, minimum/maximum price, minimum bedrooms, page, limit, and sort filters.
- `GET /api/v1/listings/:id` returns one published listing or not found.
- Register, sign in, sign out, and retrieve the current account through `/api/v1/auth/*`.
- Owners can list, create, edit, and delete their listings through `/api/v1/listings` and `/api/v1/listings/mine`.
- Administrators can list submissions, filter by review state, approve/reject a listing, and remove any listing under `/api/v1/admin/listings`.
- Validate supported cities, purposes, property types, positive prices and areas, and required listing content. A public listing requires at least one photo; drafts may omit publication requirements.
- Use consistent JSON errors and appropriate HTTP status codes. `GET /api/v1/health` reports service health.

## Mobile client (`mobile/`)

### Current scope

- Provide a lightweight home discovery screen for the follow-on mobile client.
- Load public listings from `/api/v1/listings` and show local illustrative listings when the API is unavailable or returns no usable results.
- Present listing photo, location, title, price, rental period label, bedroom count, and sale/rent status in a scrollable layout. Show loading feedback during fetches.

### Follow-on scope

Listing details, search filters, account flows, and owner listing management are future features. Define their behavior here before implementation and preserve API compatibility with the web client.
