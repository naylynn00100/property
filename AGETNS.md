# Agent Engineering Notes

This file contains the project's durable engineering context. Keep product behavior and changing feature decisions in [SPEC.md](SPEC.md). The filename is intentionally `AGETNS.md` to match the repository convention.

## Repository layout

The project is composed of independently deployable codebases:

- `property-api/` — backend API.
- `property-web/` — browser frontend.
- `property-mobile/` — planned mobile client; it does not exist in the current checkout.

This planning root is not a monorepo. Do not introduce workspace links or imports between the application repositories. Clients communicate with the API through its versioned REST contract.

## Stable technology choices

- TypeScript is used across application code.
- The API uses Node.js, Express, Prisma 7, and SQLite for local development. Its database configuration remains environment-based so production can use another supported server database without changing the API contract.
- The web client uses React, Vite, React Router, and TanStack Query. The intended component system is shadcn/ui with preset `b6sUen30`.
- The mobile client will use React Native, Expo, and TypeScript.
- The API exposes JSON REST endpoints under `/api/v1` and maintains an OpenAPI contract.
- API authentication uses bearer tokens; passwords are stored only as secure hashes.
- Local image storage is acceptable for development, but the storage boundary remains replaceable for production.

## Engineering invariants

- Keep API, web, and mobile repositories independently deployable.
- Do not commit secrets. Each application repository should provide an `.env.example` when environment configuration is needed.
- Use integer MMK values; never use floating-point currency values.
- Validate every write endpoint and preserve a stable error response shape.
- Enforce ownership and admin authorization on the server, not only in clients.
- Use migrations and seed data for local API development.
- Include automated coverage for API authorization, listing validation, filtering, and lifecycle rules, plus important client flows where practical.
- Use HTTPS in deployed environments, a CORS allowlist, authentication rate limiting, secure headers, and basic protection against common web attacks.
- Keep API changes backward-compatible within v1 or version them explicitly.

## Local development expectations

Run commands from the relevant subproject directory. Each subproject README is the user-facing source for setup, environment variables, and available scripts:

- [property-api/README.md](property-api/README.md)
- [property-web/README.md](property-web/README.md)

When changing behavior, update [SPEC.md](SPEC.md) if the feature scope changed, and update the affected subproject README if its user workflow or setup changed.
