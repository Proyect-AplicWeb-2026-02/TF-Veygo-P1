# Veygo Web Application

Veygo is a peer-to-peer vehicle rental platform by the startup **Zyntra**. It connects vehicle **Owners**, who publish their cars and manage bookings, with **Renters**, who search nearby vehicles and book them in minutes. It is built with Domain-Driven Design and a layered Clean Architecture.

This repository contains the **Frontend Web Application** (first version, Sprint 2 / TB1). It consumes a **Fake API** (json-server). That API will be replaced by the ASP.NET Core RESTful Web Services in the next sprints; only the environment URL changes.

## Features

- **Sign in / Sign up** as Renter or Owner. The Landing Page call-to-actions open `/sign-up?role=renter` and `/sign-up?role=owner`.
- **Both roles**
  - Top bar with a notification bell (booking requests, confirmations, rejections, cancellations, new messages and reviews).
  - Messages: conversations between renters and owners.
  - My profile: personal data, photo, driver's license, preferences (language, dark mode), password change and account deletion.
  - Clicking a user's name opens their profile summary with a "Send message" option.
- **Renter**
  - Home with a search bar (location, start date, end date, vehicle type), categories, recommended vehicles, a map of nearby vehicles and upcoming bookings.
  - Vehicle search with filters (price, type, transmission, fuel, rating), availability for the selected dates and a map.
  - Vehicle details: specifications, equipment, pickup location, owner reputation and a price estimate.
  - Booking confirmation. Overlapping bookings are rejected.
  - My bookings: filter by status, cancel, and rate finished rentals.
  - Favorite vehicles.
- **Owner**
  - Home with clickable metrics (vehicles, upcoming bookings, monthly income, rating) and quick actions.
  - My vehicles: search, filter, sort, publish/unpublish, delete, and open the owner vehicle detail (summary, bookings, income, reviews, additional information).
  - Add or edit a vehicle, uploading photos from the device (main photo selection).
  - Availability: vehicle selector, calendar with booked and blocked days, block and unblock dates.
  - Bookings: accept or reject requests, filter by vehicle.
  - Transactions: editable date range, income summary, donut chart by vehicle, history and CSV report download.
  - Ratings: reviews per vehicle and a reputation message.
- **Inclusive design**
  - English (default) and Spanish (es-419).
  - ARIA attributes, visible keyboard focus and a skip link.
  - Responsive layout from 360 px to desktop.
- **External service**: OpenStreetMap tiles through Leaflet.

## Technology Stack

| Concern | Technology |
| --- | --- |
| Framework | Vue 3 (Composition API, `<script setup>`) |
| Build tool | Vite |
| UI components | PrimeVue (Material preset with Veygo blue) + PrimeIcons + PrimeFlex |
| Routing | Vue Router |
| HTTP client | Axios (shared `errorInterceptor`) |
| Internationalization | vue-i18n |
| Maps | Leaflet + OpenStreetMap |
| Fake API | json-server 0.17 |

## Project Structure

```
src/
├── shared/        Value Objects (Url, DateTime, DateRange, Money), BaseApi, BaseEndpoint, layout, i18n helpers
├── iam/           Identity and Access Management: User, sign in / sign up, session
├── fleet/         Vehicle Listings: Vehicle, VehicleLocation, VehicleSearchCriteria, search, detail, owner fleet
├── booking/       Booking lifecycle: Booking, BookingStatus, confirmation, renter and owner bookings
├── engagement/    Favorites
├── reputation/    Reviews, ReputationSummary, ratings view
├── payment/       Transactions (derived from bookings), IncomeSummary, CSV report
├── communication/ Conversations and messages
├── notification/  Notifications and domain event handlers
├── dashboard/     Renter and Owner home views (compose the other contexts)
├── router/        Routes and role guards
└── locales/       en.json, es.json
```

Every bounded context follows the same layers:

- `domain/model`: entities (`*.entity.js`), value objects and enumerations. It has no framework dependencies.
- `infrastructure`: the API client (`*-api.js`, extends `BaseApi`), resources (`*-resources.js`) and assemblers (`*.assembler.js`).
- `application`: the reactive store (`*.store.js`) that orchestrates the use cases.
- `presentation`: Vue components and views, named in kebab-case.

Design patterns used:

- Assembler / Data Mapper.
- Template Method (`BaseApi` / `BaseEndpoint`).
- Specification (`VehicleSearchCriteria`).
- Factory Method (`Booking.request`).
- Value Object.
- State transitions in the `Booking` aggregate (`confirm`, `reject`, `cancel`).
- Observer (domain event bus: `BookingRequested`, `BookingConfirmed`, `MessageSent`, `ReviewPublished`, ... → notifications).
- Read model (`AvailabilityCalendar`) and domain services (`IncomeSummary`, `ReputationSummary`).
- Anti-Corruption Layer (`TransactionAssembler` derives transactions from bookings).

## Quick Start

```bash
npm install
npm run fake-api   # terminal 1: Fake API on http://localhost:3000/api/v1
npm run dev        # terminal 2: web app on http://localhost:5173
```

Demo accounts (password `Veygo2026!`):

| Role | Email |
| --- | --- |
| Renter | leo.ramos@veygo.pe |
| Renter | juan.perez@veygo.pe |
| Owner | laura.torres@veygo.pe |
| Owner | carlos.mendoza@veygo.pe |

## Environment Variables

The app reads its settings from Vite environment files:

- `.env.development` is used by `npm run dev`.
- `.env.production` is used by `npm run build`.

| Variable | Description |
| --- | --- |
| `VITE_VEYGO_API_URL` | Base URL of the Veygo API (`http://localhost:3000/api/v1` locally) |
| `VITE_*_ENDPOINT_PATH` (users, vehicles, bookings, favorites, blocked dates, reviews, conversations, messages, notifications) | Resource paths |
| `VITE_MAP_TILES_URL` | Tile server of the external map service |
| `VITE_PRIME_UI_LICENSE_KEY` | PrimeUI license key. Define it **only** in `.env.local` (not committed). Get a free community key at https://primeui.dev. Without it, PrimeVue shows an "Invalid PrimeUI License" badge. |

## Deployment

1. **Fake API**
   - Publish the `server/` folder as its own repository.
   - Deploy it as a Node web service, for example on Render, with start command `npm start`.
   - Put its URL plus `/api/v1` in `.env.production`.
2. **Web App**
   - Run `npm run build`.
   - Deploy `dist/`, for example with `firebase deploy` (`firebase.json` is included and rewrites every route to `index.html`).

## Author

Zyntra team, UPC, 1ASI0730 Aplicaciones Web, 2026-20.
