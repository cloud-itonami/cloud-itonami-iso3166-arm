# Operator Guide

## First Deployment

1. Confirm the client's incorporation/legal-entity status is complete
   (route to `cloud-itonami-M6910` or local counsel first if not).
2. Register the client's intake: business type, target public function,
   prior filing history in Armenia if any.
3. Run the advisor in read-only mode against ARMEPS
   (`https://www.armeps.am/`), governed by the Law of the Republic of
   Armenia "On Procurement" (adopted 16 December 2016).
4. Compare the checklist against the client's current documentation
   (State Register of Legal Persons extract, Taxpayer Identification
   Number (TIN) record, ARMEPS supplier/participant-profile
   registration).
5. Enable gated filing-draft assistance once the Market-Entry Compliance
   Governor contract is trusted; actual submission always requires human
   sign-off.

## Minimum Production Controls

- client-owned data store for business/tax registration documents
- clear provenance (official portal/regulation citation) for every
  requirement surfaced
- approval workflow for any portal registration or filing submission
- independent re-verification that the client is not on the Ministry
  of Finance's list of bidders ineligible to participate in the
  procurement process (Law on Procurement Article 6(1)(6)/(2)) before
  any `:filing/submit` -- never trust a claimed flag
- named referral relationship with Armenian-licensed counsel or a
  registered agent for anything beyond checklist/draft assistance
- monthly audit export
- disputes/appeals route to the person examining procurement-related
  appeals as defined by the Law on Procurement, not this actor -- it
  has no standing to file an appeal on the client's behalf

## Certification

Certified operators must prove data provenance, audit traceability, that
automated actions cannot bypass the Market-Entry Compliance Governor, and
a working referral relationship with Armenian-licensed counsel or a
registered agent for whatever licensed representation the law of
Armenia requires for actual public-procurement filings.
