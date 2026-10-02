# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.1] - 2026-09-30

### Changed
- **IAM**: New sign in and sign up design: a glass card with the form over an animated night road scene (headlight and taillight trails toward the Lima skyline), with one short message for Renters or Owners. The content of the forms did not change.

## [0.2.0] - 2026-09-27

### Added
- **Communication**: Messages view (conversations and chat) for renters and owners; "Contact owner" in the vehicle detail.
- **Notification**: Notification bell in the top bar fed by domain events (booking requested, confirmed, rejected, cancelled; message sent; review published).
- **IAM**: My profile (personal data, photo, driver's license, preferences, password, account deletion) and a user profile dialog opened by clicking a user's name.
- **Fleet**: Availability view (calendar, block/unblock dates), owner vehicle detail, photo upload in the vehicle form, additional vehicle information, search bar with location, start date, end date and vehicle type.
- **Reputation**: Reviews of finished rentals, owner ratings view with reputation message.
- **Payment**: Transactions view with date range, summary, chart by vehicle, history and CSV report.
- **Dashboard**: Clickable owner metrics and new quick actions.

### Changed
- Navigation menus of both roles include Messages and My profile; the owner menu also includes Availability, Transactions and Ratings.

## [0.1.0] - 2026-09-27

### Added
- **IAM**: Sign in and sign up for Renters and Owners (`?role=renter|owner` from the Landing Page), session persistence and role-based route guards.
- **Fleet**: Vehicle search with filters (Specification pattern), map of nearby vehicles (OpenStreetMap / Leaflet), vehicle details, owner fleet management (create, edit, publish/unpublish, delete).
- **Booking**: Booking confirmation with availability check (no overlapping bookings), renter bookings (cancel) and owner bookings (accept / reject).
- **Engagement**: Favorite vehicles.
- **Dashboard**: Renter and Owner home views with metrics and upcoming bookings.
- **Shared**: Value Objects (`Url`, `DateTime`, `DateRange`, `Money`), `BaseApi` / `BaseEndpoint`, error interceptor, responsive layout, i18n (English by default, Spanish), terms and conditions.
- **Fake API**: json-server database and routes under `/api/v1`.
