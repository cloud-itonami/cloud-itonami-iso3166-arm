# Business Model: Independent Public-Sector Market-Entry & Procurement Compliance Service — Republic of Armenia

## Classification

- Repository: `cloud-itonami-iso3166-arm`
- ISO 3166: `ARM` (Armenia)
- Activity: public-procurement market-entry and ongoing regulatory-
  compliance navigation for an already-incorporated operator
- Social impact: [:sme-market-access :public-spend-transparency :cross-border-friction-reduction]

## Customer

- an already-incorporated `cloud-itonami-cofog-{code}` /
  `cloud-itonami-isco-{code}` / `cloud-itonami-unspsc-{segment}` /
  `cloud-itonami-{ISIC}` operator wanting to bid on an Armenian public
  contract
- a foreign SME or civic-tech vendor entering the public sector in
  Armenia for the first time
- a `cloud-itonami-M6910` client that has just completed incorporation
  and now needs public-sector market access

## Offer

- registration walkthrough for ARMEPS (the unified electronic system
  of public procurement, mandatory under the Law on Procurement,
  adopted 16 December 2016), operated under the Ministry of Finance of
  the Republic of Armenia
- business/tax registration checklist: Agency for the State Register
  of Legal Persons registration (Staff of the Ministry of Justice),
  which issues the Taxpayer Identification Number (TIN) in the SAME
  act as registration
- ineligible-bidders-list screening: independent verification that an
  operator is not on the Ministry of Finance's list of bidders
  ineligible to participate in the procurement process (Law on
  Procurement Article 6(1)(6)/(2))
- ongoing regulatory-change monitoring subscription
- compliance-audit export package for the client's own records

## Revenue

- per-engagement market-entry fee (one-time registration + checklist
  completion)
- recurring regulatory-change monitoring subscription
- compliance-audit export package

## Trust Controls

- any actual portal registration or filing submission requires
  Market-Entry Compliance Governor clearance and always escalates to
  human sign-off (`:filing/submit` is never automated at any phase)
- a false or fabricated regulatory-requirement claim is a HARD hold that
  cannot be overridden by human approval alone -- it must be corrected
  against a cited official source first
- membership on the Law on Procurement Article 6(1)(6) list of
  ineligible bidders is a HARD hold on `:filing/submit`, independently
  re-verified rather than trusted from a claimed flag -- this is an
  unconditional, categorical bar under Armenian law, not gated behind
  any other engagement field
- this service does **not** provide legal or tax advice; characterization
  and filing on the client's behalf beyond checklist/draft assistance
  routes to Armenian-licensed counsel or a registered agent

## Boundary with adjacent actors (read before forking)

- **`cloud-itonami-M6910`**: helps a client BECOME a legal entity
  (incorporation, ISIC 6910) -- a prior, different regulatory phase
  (company law). This blueprint assumes incorporation is already done and
  handles public-procurement market entry (a different regulatory domain).
- **`cloud-itonami-cofog-{code}`**: a jurisdiction-agnostic operator
  template for ONE public function. This blueprint is the orthogonal
  jurisdiction-specific axis -- the two compose (fork a COFOG-function
  blueprint AND this one to operate in Armenia).
