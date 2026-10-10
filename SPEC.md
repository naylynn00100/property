# Property Portal Feature Specification

## 1. Overview

Build a property marketplace for Myanmar focused on **Yangon** and **Mandalay**. The first release lets people discover, publish, and manage property listings for **sale** and **rent**.

The product is English-only in version 1 and all monetary values are displayed and stored in **Myanmar kyat (MMK)**. No other currencies are supported.

## 2. Goals

- Give buyers and renters a fast, trustworthy way to find properties in Yangon and Mandalay.
- Let owners and agents create listings to sell or rent property.
- Provide responsive web and mobile experiences.

## 3. Version 1 Scope

### User roles

| Role | Capabilities |
| --- | --- |
| Visitor | Browse, search, filter, view listing details, and contact a listing owner/agent. |
| Registered user | All visitor capabilities plus create, edit, publish, pause, and delete their own listings. |
| Admin | Review and manage users and listings; hide or remove inappropriate listings. |

### Listings

Each listing must include:

- Title and English description
- Transaction type: `sale` or `rent`
- Property type: apartment/condo, house, land, commercial, or room
- City: Yangon or Mandalay
- Township/neighborhood and address text
- Price in whole MMK
- Bedrooms, bathrooms, floor area, and land area where applicable
- Furnishing status and key amenities
- At least one image before publication
- Contact name and phone number
- Listing status: draft, pending review, published, paused, sold/rented, or rejected

For rental listings, price is monthly MMK unless the listing states another billing period. The rental billing period must be explicit.

### Discovery

Visitors can:

- Search by title, township/neighborhood, and property type.
- Filter by city, transaction type, property type, price range, bedroom count, bathroom count, and listing status where authorized.
- Sort published results by newest, price ascending, or price descending.
- View listing photos, description, attributes, location text, price, and owner/agent contact details.

### Accounts and contacts

- Email and password registration and sign-in.
- Secure authenticated sessions.
- A contact action that reveals or initiates contact using the listing phone number. Version 1 does not require in-app chat or payments.
- Users can update their own profile and manage only their own listings.

### Administration

- Admins can list users and listings.
- Admins can approve, reject, pause, hide, or remove listings.
- Admin actions must be recorded with actor, action, target, and timestamp.

## 4. Out of Scope for Version 1

- Languages other than English
- Currencies other than MMK or currency conversion
- Online payments, deposits, or contracts
- In-app messaging, saved searches, push notifications, and favorites
- Map-based search and exact public map coordinates
- Cities outside Yangon and Mandalay
- Mortgage calculation, valuation, and real-estate agent subscription plans

## 5. Product Rules

- All UI copy, validation messages, and public values are English in v1.
- Currency values are integer MMK amounts. Do not use floating-point currency values.
- Published listings must be located in Yangon or Mandalay and have at least one image, a valid transaction type, a price, and contact details.
- Only the listing owner or an admin may edit its content or change its lifecycle state.
- Visitors see only `published` listings. Users see their own listings at all statuses; admins see all listings.
- Telephone numbers and addresses should be handled as personal data and exposed only as necessary to support a listing.

## Mobile experience

When the mobile client is introduced, it will provide the same version 1 marketplace experience on iOS and Android:

- Browse, search, filter, and sort published listings.
- View listing details, photo galleries, prices, and contact actions.
- Register and sign in with a securely persisted session.
- Create, edit, and manage a user's own listings, including photo selection and upload.

## Client experience

The browser experience provides a responsive, accessible home page, search results, listing details, authentication, a user dashboard, listing creation/editing, and admin moderation. It shows loading, empty, unauthorized, validation-error, and retryable network states.

## Feature acceptance criteria

- A visitor can find published Yangon or Mandalay properties by sale/rent type, property type, price, and bedroom count.
- A registered user can create and submit a valid sale or rental listing priced in integer MMK with at least one image.
- A user cannot edit another user’s listing, while an admin can moderate any listing.
- The web and mobile clients present the same published listing data when the mobile client is available.
