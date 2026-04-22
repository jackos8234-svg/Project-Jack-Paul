# PRD.md — FuelFinder Ireland

**Version:** 1.0  
**Date:** 2026-04-22  
**Status:** Draft  

---

## 1. Overview

### 1.1 Product Summary

FuelFinder Ireland is a mobile-first web application and native mobile app (iOS and Android) that aggregates petrol and diesel prices from stations across the Republic of Ireland. It allows users to find the cheapest fuel near them, along a planned route, or filtered by fuel type and station amenities — all without requiring a user account. Optional account creation unlocks fuel card integration.

### 1.2 Problem Statement

Fuel prices in Ireland vary significantly between stations and can change multiple times per day. Drivers have no reliable, real-time, centralised tool to compare prices, plan fuel stops on a route, or verify that a listed price is still accurate when they arrive. This leads to overspending, wasted journeys, and a lack of price transparency.

### 1.3 Target Users

- **Primary:** Everyday Irish drivers (cars, vans) seeking the cheapest nearby fuel.
- **Secondary:** Truck drivers and operators of heavy vehicles needing high-flow diesel and HGV-accessible stations.
- **Tertiary:** Commuters and long-distance drivers who want to plan fuel stops along a specific route.
- **Optional registered users:** Drivers who hold fuel cards and want to see applicable stations and discounts.

---

## 2. Goals & Success Criteria

### 2.1 Product Goals

- Provide accurate, near-real-time fuel prices for stations across the Republic of Ireland.
- Enable users to find the cheapest station by proximity or along a route.
- Build trust through crowd-sourced price validation and transparent data timestamps.
- Generate revenue through non-intrusive banner advertising in v1.
- Require zero friction to use — no login required for core functionality.

### 2.2 Success Criteria (v1 Launch)

- At least 500 petrol stations across the Republic of Ireland seeded in the database at launch.
- Price data freshness: all prices display the timestamp of last update.
- Crowd-sourced correction: 10 agreeing user submissions within a defined time window triggers an automatic price update.
- Map integration: tapping a station opens directions in Apple Maps or Google Maps.
- Core user flow (open app → find cheapest nearby station → get directions) completable in under 30 seconds.

---

## 3. Scope

### 3.1 In Scope (v1)

- Mobile-responsive web app and native iOS and Android apps.
- Fuel price display: petrol (unleaded), diesel, and premium grades where available.
- Map view and list view of nearby stations.
- GPS-based location detection (with user permission).
- Route-based station search (enter origin and destination, find cheapest station along or near route).
- Crowd-sourced price submission and correction (10-user threshold for auto-update).
- Price timestamp display ("Last updated: X hours ago") on each station detail.
- Station amenity data: toilets, food services, deli, truck/HGV accessibility, high-flow diesel.
- User reviews with text and photo attachments.
- Fuel type filter (petrol / diesel).
- HGV filters (truck-accessible, high-flow diesel).
- Map highlighting: cheapest stations in a dense area displayed in a distinct highlight colour (e.g. red).
- Optional user account: register, log in, add fuel card details (card type only — shows applicable stations in v1).
- Maps integration: deep link to Apple Maps or Google Maps for directions.
- Banner advertising (non-intrusive, placement defined — provider TBD).
- Admin dashboard: full station management, price management, flagged price report review, review moderation, amenity data management.
- English language only.
- Republic of Ireland only.

### 3.2 Out of Scope (v1)

- Northern Ireland coverage (GBP/pence per litre pricing adds complexity — post-launch).
- Irish (Gaeilge) language support.
- Intrusive or interstitial advertising (planned for post-launch once user base is established).
- Loyalty points or rewards tracking via fuel cards (v1 shows card compatibility only).
- In-app navigation (the app links out to Apple Maps / Google Maps rather than providing its own turn-by-turn).
- Push notifications.
- Automated price data ingestion via external API (data source is TBD — see dependencies).
- Price history charts or trends.
- Fuel cost calculators.

---

## 4. Features

### 4.1 Home / Map Screen

- On launch, the app requests GPS location permission.
- If granted: map centres on user's current location and loads nearby stations within a configurable radius.
- If denied: user is prompted to enter a location manually.
- Map pins represent stations. In areas with a high density of stations (e.g. Dublin city), the station(s) with the currently cheapest price for the user's selected fuel type are highlighted in red. All other pins use a standard colour.
- A toggle or tab allows switching between map view and list view.
- List view sorts stations by distance (nearest first) by default, with price sort as an option.
- Each station card/pin shows: station name, brand logo (if available), current price for selected fuel type, distance from user, and last updated timestamp.

### 4.2 Fuel Type Filter

- Persistent filter at the top of the screen: Petrol / Diesel (default: Petrol).
- Sub-filter for fuel grade where applicable (e.g. standard unleaded, premium unleaded, standard diesel).
- Filter state persists for the session.

### 4.3 Station Detail Screen

- Accessed by tapping a map pin or list item.
- Displays:
  - Station name and brand.
  - Address.
  - All available fuel types and their current prices.
  - Last updated timestamp per fuel type.
  - Amenities (toilets, food, deli, car wash, ATM — as populated).
  - HGV flags: truck-accessible forecourt, high-flow diesel availability.
  - "Get Directions" button — deep links to Apple Maps or Google Maps with the station as destination.
  - User reviews section (text + photos, most recent first).
  - "Report incorrect price" button.
  - "Submit updated price" button.

### 4.4 Price Submission & Correction

- Any user (no account required) can submit a price update for any fuel type at a station.
- Submission form: fuel type selector, price input (€ per litre, to 3 decimal places), optional note.
- Submissions are logged with timestamp and (if logged in) user ID, or anonymously.
- If 10 or more users submit the same price (within a configurable time window, default 24 hours) for the same fuel type at the same station, the price is automatically updated in the database.
- Submissions that do not reach the threshold within the time window are discarded and do not affect the displayed price.
- Admin can override or manually update any price at any time via the admin dashboard.
- When a price is updated via crowd-source, the timestamp updates to reflect the time of the 10th confirming submission.

### 4.5 Route-Based Search

- User inputs an origin and a destination (text search or use current location for origin).
- App calculates the route and identifies petrol stations within a configurable corridor distance of that route (default: 5km either side).
- Results are displayed on a map overlay of the route and in a list, sorted by price (cheapest first) for the selected fuel type.
- User can adjust the corridor width with a slider (e.g. 1km–20km).
- Tapping a result opens the Station Detail Screen.

### 4.6 Station Amenities

- Each station can have the following amenity flags (boolean): toilets, food service, deli/hot food, car wash, ATM, HGV accessible forecourt, high-flow diesel.
- Amenity data can be submitted by any user (no account required) via the station detail screen.
- Amenity submissions go to a pending queue in the admin dashboard for approval before going live.
- Admin can also add or edit amenity data directly.

### 4.7 User Reviews

- Any user (no account required) can submit a text review and up to 5 photos per review.
- Reviews display: text, photos (thumbnail grid), date submitted, and an anonymous identifier or username if logged in.
- Users can flag a review as inappropriate. Flagged reviews enter a moderation queue in the admin dashboard.
- Admin can remove reviews.
- No star rating system in v1 — text and photo reviews only.

### 4.8 Optional User Accounts

- Users can optionally create an account (email + password, or social login TBD).
- Account creation is never required to use any core feature.
- Registered users can:
  - Save a fuel card type to their profile (e.g. Circle K Miles, Applegreen Rewards, Texaco Star — list to be defined).
  - When a fuel card is saved, stations that accept that card are visually flagged on the map and in list view.
  - v1 shows card compatibility only — no discount amount data.
- Account data: email, hashed password, saved fuel card type. No payment data is stored.

### 4.9 Advertising

- v1 includes non-intrusive banner advertisements only.
- Placement: a single banner ad at the bottom of the map/list screen and on the station detail screen.
- No interstitial ads, no pop-ups, no video ads in v1.
- Ad provider: TBD (Google AdSense or equivalent — to be confirmed before build).
- Ads must not obscure any price data, map controls, or navigation elements.
- Post-launch: more prominent ad formats may be introduced once the user base is established. This is explicitly a post-v1 decision.

### 4.10 Admin Dashboard

- Accessible via a separate web URL (not visible in the public app).
- Authentication: admin login required (email + password, MFA recommended).
- Features:
  - **Station management:** add, edit, deactivate stations. Fields: name, brand, address, coordinates, amenities, fuel types offered.
  - **Price management:** view and manually update any price at any station. Override crowd-sourced updates.
  - **Price report queue:** review pending crowd-sourced price submissions. See submission count vs threshold, approve early or reject.
  - **Amenity submission queue:** approve or reject user-submitted amenity updates.
  - **Review moderation:** view flagged reviews, remove inappropriate content.
  - **User management:** view registered accounts, disable accounts if necessary.
  - **Data source management:** (placeholder for future external data ingestion configuration — TBD).

---

## 5. User Stories

| ID | As a... | I want to... | So that... |
|----|---------|-------------|------------|
| US-01 | Guest user | See the cheapest petrol station near my current location | I can save money on fuel |
| US-02 | Guest user | Filter results by diesel instead of petrol | I only see relevant stations for my vehicle |
| US-03 | Guest user | See when a price was last updated | I know how reliable the listed price is |
| US-04 | Guest user | Report an incorrect price at a station | Other users get accurate information |
| US-05 | Guest user | Get directions to a station in Apple Maps or Google Maps | I can navigate there easily |
| US-06 | Guest user | Search for cheap fuel along my planned route | I can plan a fuel stop on a long journey |
| US-07 | Guest user | See what amenities a station has | I can choose a station that also has food or toilets |
| US-08 | Guest user | Write a review and attach photos for a station | I can share my experience with other drivers |
| US-09 | Guest user | Filter for HGV-accessible stations with high-flow diesel | I can find suitable stations for my truck |
| US-10 | Registered user | Save my fuel card type to my account | I can quickly see which stations accept my card |
| US-11 | Admin | Manually update a fuel price | I can correct bad data without waiting for crowd-source threshold |
| US-12 | Admin | Review and approve crowd-sourced price submissions | I can maintain data quality |
| US-13 | Admin | Moderate flagged user reviews | I can keep content appropriate |
| US-14 | Admin | Add and edit station details and amenities | I can keep the station database accurate |

---

## 6. Non-Goals

The following are explicitly not being built in v1 and should not be architected for unless noted:

- Northern Ireland coverage.
- Irish language (Gaeilge) support.
- In-app turn-by-turn navigation.
- Fuel price history or trend charts.
- Fuel cost calculator.
- Push notifications or price alerts.
- Loyalty points tracking.
- Intrusive or interstitial advertising.
- Automated external data ingestion (data source TBD — infrastructure should be designed to accommodate this in future).

---

## 7. Dependencies & Open Questions

| # | Dependency / Question | Owner | Status |
|---|----------------------|-------|--------|
| D-01 | External fuel price data source (API or scrape target for automated price ingestion) | Product | **TBD — must be resolved before v1 launch** |
| D-02 | Ad network provider (Google AdSense or alternative) | Product | TBD |
| D-03 | Fuel card list (which cards to support in v1) | Product | TBD |
| D-04 | Social login provider for accounts (or email/password only?) | Engineering | TBD |
| D-05 | Crowd-source time window (default 24hrs — confirm or adjust) | Product | Draft: 24 hours |
| D-06 | Route corridor default distance (draft: 5km) | Product | Draft: 5km |
| D-07 | Maps integration: Apple Maps vs Google Maps vs both (deep link to both, user's device default?) | Engineering | TBD |

---

## 8. Platform & Technical Notes

- **Platforms:** iOS (native), Android (native), mobile-responsive web app.
- **Location:** GPS permission requested on first launch. Graceful fallback to manual location entry if denied.
- **Maps:** Deep-link integration to Apple Maps and/or Google Maps for directions. No in-app map navigation.
- **Authentication:** Optional. Core features are fully accessible without an account.
- **Data privacy:** No personally identifiable data collected from guest users. Anonymous price submissions. Registered user data: email and fuel card type only. GDPR compliance required (Republic of Ireland / EU).
- **Language:** English only (v1).
- **Currency:** Euro (€), price per litre displayed to 3 decimal places (standard Irish pump format, e.g. €1.879).

---

## 9. Out of Scope Revisited (Explicit Exclusions)

To be clear with any AI agent reading this document: the following must not be built, scaffolded, or architected unless this PRD is explicitly updated:

- No Northern Ireland data, GBP pricing, or pence-per-litre formatting.
- No Gaeilge strings or i18n framework (English only).
- No interstitial, pop-up, or video ads.
- No in-app navigation engine.
- No price history database tables or chart components.
- No push notification infrastructure.
- No loyalty/rewards points logic for fuel cards.

---

*End of PRD.md v1.0*
