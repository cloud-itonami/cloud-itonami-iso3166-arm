# cloud-itonami-iso3166-arm

Open ISO 3166 Blueprint for **ARM**: Republic of Armenia --
**`:implemented`**.

This repository designs **and implements** a forkable OSS business for
an independent public-sector market-entry consultant: an already-
incorporated operator (e.g. a `cloud-itonami-cofog-{code}`,
`cloud-itonami-isco-{code}`, `cloud-itonami-unspsc-{segment}` or
`cloud-itonami-{ISIC}` blueprint fork) gets a Compliance Advisor +
independent **Market-Entry Compliance Governor** to navigate public-
procurement registration, local business/tax registration, and
regulatory-compliance rules in Armenia, so the operator can win and
service a government contract without hiring a full in-house compliance
department.

## Official surface (WebFetch/curl-verified 2026-07-21)

- Procurement: ARMEPS (`https://www.armeps.am/`), the unified electronic
  system of public procurement, operated under the Ministry of Finance
  of the Republic of Armenia (the "authorised body" per the Law of the
  Republic of Armenia "On Procurement", adopted 16 December 2016).
  Mandatory electronic-system use for state/community-majority-owned
  contracting authorities has been in force since 1 April 2017. Note:
  the 2016 law's own Article 2(14) names `www.procurement.am` as the
  "bulletin" -- this repo checked that URL directly rather than
  assuming it is still live, and found it now serves a bare 5-byte
  placeholder page (confirmed dead/superseded); ARMEPS is the real,
  live, operating system today.
- Business/tax identity: the Agency for the State Register of Legal
  Persons (an agency of the Staff of the Ministry of Justice,
  `e-register.am`), under the Law "On State Registration of Legal
  Persons ..." (Law No. HO-169, adopted 3 April 2001, as amended).
  This repo specifically investigated, rather than assumed, whether
  registration and Taxpayer Identification Number (TIN) issuance are
  one act or two: the State Revenue Committee's OWN submission to the
  OECD states the TIN "is issued ... by State Register" at the moment
  of registration -- a SINGLE act performed by the registration
  authority (Ministry of Justice), not a separate subsequent act by the
  tax authority itself.
- Dispute/exclusion mechanism: Law on Procurement Article 6(1)(6) +
  6(2) establishes a list of bidders ineligible to participate in the
  procurement process, operated by the Ministry of Finance (confirmed
  still current by a February-2025 secondary source, Legal500's Armenia
  procurement guide) for a 2-year time limit on inclusion.

## Implementation (R0)

| Piece | Location |
|---|---|
| Actor namespaces | `src/marketentry/*` |
| Governor | `:market-entry-compliance-governor` |
| Ops | `:engagement/intake` · `:jurisdiction/assess` · `:filing/draft` · `:filing/submit` |
| Flagship HARD check | `ineligible-bidder-listed` (Law on Procurement Article 6(1)(6)/(2), an unconditional registry-membership check independently re-verified -- see `docs/adr/0001-architecture.md`) |
| Compliance catalog | `src/statute/facts.cljc` -- Law on Limited Liability Companies, Labour Code, Law on Protection of Personal Data |
| Tests | `clojure -M:dev:test` |
| Demo | `clojure -M:dev:run` |
| Architecture ADR | [`docs/adr/0001-architecture.md`](docs/adr/0001-architecture.md) |

`:filing/submit` is never in any phase's `:auto` set -- human sign-off
is structural, not a rollout milestone.

## No robotics premise -- digital/data service exemption

Market-entry and procurement-compliance navigation is a pure data/software
service with no physical-domain work (portal registration, document
checklists, regulatory-change monitoring) -- the same exemption class as
`cloud-itonami-6310` (HR SaaS replacement) and `cloud-itonami-gtin-*`.
`blueprint.edn` sets `:itonami.blueprint/robotics false` and
`:required-technologies` lists only real capabilities (`:identity`,
`:forms`, `:dmn`, `:bpmn`, `:audit-ledger`), no `:robotics`.

## Core Contract

```text
operator intake + prior filing history
        |
        v
Compliance Advisor -> Market-Entry Compliance Governor -> filing draft, or human sign-off
        |
        v
gated portal registration / filing submission + audit ledger
```

No automated proposal can submit a portal registration or filing the
governor refuses, suppress a compliance record, or claim a legal/tax
conclusion the governor has not cleared. `:filing/submit` is never in any
phase's `:auto` set -- it always requires human sign-off.

## What this is NOT

- **Not the government of Armenia.** This blueprint is an independent
  operator the government contracts with or that bids into its
  procurement -- never the government itself, and never an official
  channel.
- **Not legal or tax advice.** Every regulatory claim must cite the
  official source and route final filings to Armenian-licensed counsel
  or a registered agent where the law requires licensed representation.

## Capability layer

Required capabilities (`blueprint.edn`):

- :identity
- :forms
- :dmn
- :bpmn
- :audit-ledger

See [`docs/business-model.md`](docs/business-model.md) and
[`docs/operator-guide.md`](docs/operator-guide.md).

## License

AGPL-3.0-or-later.

## Culture catalog

Alongside the market-entry / statute catalogs, this repo carries a
**country-level regional-culture catalog** (ADR-2607171400 addendum 2,
`cloud-itonami-municipality-culture-catalog` Wave 1, in
`com-junkawasaki/root`) -- national dishes, protected products, beverages,
crafts, festivals and heritage sites for Armenia:

- `src/culture/facts.cljc` -- the catalog, source of truth (keyed by
  uppercase ISO3, mirroring `statute.facts`).
- `schema/culture.edn` -- DataScript schema.
- `data/culture-tx.edn` -- derived DataScript tx-data (regenerated from
  the catalog, never hand-edited).

City-level counterparts live in the `cloud-itonami-municipality-*` repos.
Same provenance discipline as the compliance catalogs: every entry cites a
source URL that was actually fetched and read on `:culture/retrieved-at`;
summaries state only what the cited source confirms. An item not in
`culture.facts/catalog` has no spec-basis -- never fabricate one.
