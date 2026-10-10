# Property Portal

Planning and orchestration repository for the Property Portal marketplace. Application source lives in independently deployable sibling repositories, referenced here as Git submodules.

## Repositories

- [property-api](https://github.com/naylynn00100/property-api) — backend API
- [property-web](https://github.com/naylynn00100/property-web) — browser client
- `property-mobile` — planned; not present yet

Clone this repository with submodules:

```bash
git clone --recurse-submodules https://github.com/naylynn00100/property.git
```

If you already cloned without submodules:

```bash
git submodule update --init --recursive
```

Product behavior is in [SPEC.md](SPEC.md). Engineering invariants are in [AGETNS.md](AGETNS.md). Setup commands are in each sibling README.
