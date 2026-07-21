# ADR-0001: Architecture — Armenia market-entry compliance actor (`marketentry`)

**Status**: accepted
**Date**: 2026-07-21

## Context

`cloud-itonami-iso3166-arm` was published as a `:blueprint` (docs +
`blueprint.edn` only, then a country-level `culture.facts` catalog in a
separate Wave 1 batch) but carried ZERO `src/marketentry` or
`src/statute` content -- its `:public-sector/market-entry-compliance`
domain, declared in `blueprint.edn`, was unimplemented. This ADR closes
that gap, following the pattern established by
`cloud-itonami-iso3166-jpn` (origin) and `cloud-itonami-iso3166-bgr` /
`cloud-itonami-iso3166-aze` / `cloud-itonami-iso3166-alb` (the simpler,
no-`goyoukiki` shape this blueprint also uses -- `blueprint.edn`'s
`:required-technologies` does not list `:ontology`, so this fork skips
the `marketentry.goyoukiki` real-tender-fact bridge JPN carries).

## Decision

Build the full governed-actor architecture for `marketentry`, mirroring
JPN/BGR/AZE/ALB's harness verbatim (StateGraph node names, governor
hard/escalate contract, phase 0-3 rollout, `Store` protocol with
MemStore + DatomicStore parity) and researching Armenia's own real
market-entry rules from scratch for the country-specific content.

- **Store**: `marketentry.store`, MemStore + DatomicStore, proven parity
  via contract test.
- **Registry**: `marketentry.registry`, pure DRAFT-certificate
  construction via `unsigned-certificate`, jurisdiction-scoped sequence
  numbering (`ARM-DFT-000000`, `ARM-SUB-000000`). No additional numeric
  threshold function -- see the flagship-check discussion below for why.
- **Governor**: `:market-entry-compliance-governor` (family keyword from
  `blueprint.edn`).
- **Entity shape**: `engagement`, sequential draft -> submit on the same
  record. `high-stakes` = `#{:actuation/draft-filing
  :actuation/submit-filing}`.
- **Phase**: 0->3; `:filing/draft` and `:filing/submit` NEVER auto-
  commit at any phase.

### Flagship HARD check: `ineligible-bidder-listed` -- and what was deliberately NOT used

Researching Armenia's own Law of the Republic of Armenia "On
Procurement" (adopted 16 December 2016 -- WebFetch itself returned only
undecoded binary for both the WTO-hosted and the independently-mirrored
UNODC copy of this law's official English translation, so curl with a
standard user-agent + `pdftotext` were used to extract and directly
read the article text, the same fallback prior iterations of this loop
used for South Korea/Azerbaijan) surfaced Article 6, "Eligibility for
participation in procurement and qualification criteria", with SIX
distinct grounds of mandatory ineligibility. Two are notable:

1. **Article 6(1)(2)**: a tax/social-security-arrears provision
   structurally similar to Bulgaria's flagship check ("those who have
   overdue liabilities amounting up to one percent of the price
   proposal submitted thereby ... but in the amount not exceeding fifty
   thousand drams of the Republic of Armenia"). This would have made a
   genuinely fourth distinct check shape (percentage-of-BID-PRICE
   rather than Bulgaria's percentage-of-ANNUAL-TURNOVER), but this
   iteration deliberately did NOT implement it: a February-2025
   secondary source (Legal500's Armenia public-procurement guide)
   enumerates the CURRENT "grounds of mandatory exclusion" and omits
   this provision entirely, while independently confirming an amendment
   elsewhere in the SAME article (the criminal-conviction lookback
   period, 3 years in the 2016 text vs. 5 years reported by the 2025
   guide) -- proving Article 6 has genuinely been amended since 2016.
   This iteration's repeated attempts to reach Armenia's own National
   Assembly site (`parliament.am`) to check the current text directly
   were met with a TCP-level connection reset on every attempt (both
   HTTP and HTTPS), the same class of environment-level unreachability
   prior iterations hit for `etender.gov.az`. Rather than cite a
   possibly-repealed provision as if it were current law, this
   iteration narrowed scope and left it out. `marketentry.registry`'s
   own docstring documents this decision so a future iteration does not
   have to re-derive it from scratch.
2. **Article 6(1)(6) + 6(2)**: the list of bidders ineligible to
   participate in the procurement process -- a bidder is added for
   violating a contract/procurement-process obligation (leading to
   unilateral rescission or removal from the process), refusing to
   conclude a contract as selected bidder, or refusing further
   participation after bid opening; inclusion lasts a 2-year time
   limit. This mechanism IS confirmed still current: independently
   corroborated by BOTH the 2016 primary text (read directly, as
   above) AND the February-2025 Legal500 guide, which separately
   states the register "is operated by the Ministry of Finance of RA".
   `ineligible-bidder-listed-violations` independently re-verifies the
   engagement's own declared `:on-ineligible-bidders-list?` flag rather
   than trusting it -- the same UNCONDITIONAL (not gated behind a
   `:requires-X?` flag), boolean-registry-membership SHAPE Azerbaijan's
   `unreliable-supplier-listed` check already established for this
   family (a third instance of it). This is an honest choice grounded
   in confirmable currency, not a failure to look for a fourth distinct
   shape -- see point 1 above for the shape this iteration found but
   could not confirm.

### Other HARD checks (all unoverridable)

1. **spec-basis** -- never invent a jurisdiction's market-entry
   requirements (`marketentry.facts` G2 catalog: ARMEPS, State Register
   of Legal Persons, TIN for ARM).
2. **evidence-incomplete** -- draft/submit require a full assessment
   checklist on file.
3. **ineligible-bidder-listed** -- see above (FLAGSHIP).
4. **engagement-fee-mismatch** -- recompute `base-fee + monthly-rate ×
   monitoring-months` (ground-truth-recompute discipline).
5. **tin-unverified** -- conditional on `:requires-tin?` (Taxpayer
   Identification Number, issued by the Agency for the State Register
   of Legal Persons AT THE SAME MOMENT as state registration -- see
   `marketentry.facts` for the one-act-vs-two-act investigation).
6. **already-drafted / already-submitted** -- dedicated booleans, never
   a `:status` value.

### `rep-spec-basis`: real but narrow, like Bulgaria's and Albania's

Law on Procurement Article 6(1)(3) mandates disqualification for
conviction of specified serious offenses (terrorism financing, child
exploitation, human trafficking, criminal association, bribery), within
a lookback period of the bid-submission date, and extends this personal
exclusion ground to a representative of the bidder's executive body who
has been so convicted -- real (curl/pdftotext-verified against the
2016 law's own text), but NOT a claim that Armenia mandates a resident/
domestic representative for public-tender participation itself. Note:
the exact lookback period is reported with mixed confidence -- 3 years
per the 2016 primary text this iteration read directly, HIGH
confidence; 5 years per the Legal500 2025 secondary source, MODERATE
confidence only, since `parliament.am` was unreachable to confirm the
amending law's own text.

### `statute.facts` (second, orthogonal catalog)

Three Armenian statutes, all confirmed via arlis.am (Armenia's own
Legal Information System, run by the Ministry of Justice) or a
government body's own hosted document, unlike Azerbaijan/Albania/South
Korea's law portals in this loop, arlis.am's `documentview.aspx?docid=`
pages render as plain, fully-readable HTML on the FIRST WebFetch attempt
-- no JS-SPA blocker, no PDF-extraction fallback needed for two of the
three entries. This iteration specifically investigated (rather than
assumed, mirroring Azerbaijan's/Albania's own structural investigations)
whether Armenia folds commercial-entity law into its Civil Code
(Azerbaijan's pattern) or carries a standalone Commerce Act (Bulgaria/
Germany's pattern) or a standalone Commercial Companies Law covering all
entity types (Albania's pattern) -- and found a FOURTH structural
pattern: Armenia's Civil Code (HO-239, 1998) carries only general
legal-persons provisions, while the specific company-law rules for its
predominant entity type (the limited liability company) live in a
SEPARATE standalone Law on Limited Liability Companies (adopted 24
October 2001), confirmed via curl+pdftotext directly against Armenia's
own Translation Centre (a state body under the Ministry of Justice) and
cross-corroborated by 4 independent secondary sources on the same date.

## Consequences

- `src/` now genuinely exists with real, tested, WebFetch/curl-cited
  content for this blueprint's declared domain (`:public-sector/
  market-entry-compliance`) -- moves this repo's
  `manifest/itonami-fleet-audit.edn` `:prod-ready?` signal from `:stub`
  to `:active`.
- The existing `culture.facts` catalog (Wave 1, unrelated batch) is
  untouched.
- The tax-arrears provision found in Article 6(1)(2) is a genuine,
  verified, NOT-implemented extension point for a future iteration --
  only if that iteration can independently confirm current legal
  validity (e.g. by reaching `parliament.am` from a different
  environment, or locating a dated, unambiguous consolidated text).
- Sibling country blueprints can continue forking JPN/BGR/AZE/ALB/ARM
  and swapping in their own genuinely-researched `marketentry.facts` /
  `statute.facts` content and whichever flagship check their own law
  actually supports -- this ADR is itself evidence that the flagship
  check should be chosen from real, currency-checked research, not
  copied by rote, and that finding a provision is not the same as
  confirming it is still law.
