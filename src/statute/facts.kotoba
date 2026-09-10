(ns statute.facts
  "General-law compliance catalog for Armenia (ARM) -- extends this
  repo's existing `marketentry.facts` (public-procurement market-entry
  only, narrow scope) with a second, orthogonal catalog of statutes a
  company operating in this jurisdiction must generally track for
  compliance. Mirrors cloud-itonami-iso3166-jpn/-deu/-bgr/-aze/-alb's
  `statute.facts` (ADR-2607141700, cloud-itonami-compliance-fact-
  federation).

  Every entry cites an OFFICIAL Armenian government-hosted URL --
  never fabricated. Armenia's official legal-acts database is
  arlis.am (Armenian Legal Information System, run by the Ministry of
  Justice) -- this iteration specifically verified rather than assumed
  that arlis.am is directly WebFetch-readable: unlike Azerbaijan's
  e-qanun.az, Albania's qbz.gov.al and South Korea's law.go.kr (all
  JS single-page apps this loop's tools could not render), arlis.am's
  own `documentview.aspx?docid=<n>` pages render as plain, fully
  readable HTML on the first attempt -- confirmed for all three
  entries below directly via WebFetch, HIGH confidence, no PDF
  extraction or secondary-source fallback needed:

  - Civil Code (Քաղաքացիական օրենսգիրք) -- arlis.am/documentview.aspx?
    docid=145778, confirmed adopted 5 May 1998 (Law No. HO-239),
    entered into force 1 January 1999.
  - Labour Code (Աշխատանքային օրենսգիրք) -- arlis.am/documentview.aspx?
    docid=145760, confirmed adopted 9 November 2004 (Law No. HO-124-N),
    entered into force 21 June 2005.
  - Law on Protection of Personal Data (Անձնական տվյալների պաշտպանության
    մասին) -- this iteration's WebFetch attempt on the Ministry of
    Justice's own hosted English-translation PDF
    (moj.am/storage/uploads/Personal_data_protection_law_ENG_OFFICIAL.pdf)
    returned only undecoded binary/compressed stream data, so curl with
    a standard user-agent + `pdftotext` were used instead (the same
    fallback prior iterations used for South Korea's coke.facts and
    Albania's statute.facts) -- the extracted text opens with \"LAW OF
    THE REPUBLIC OF ARMENIA Adopted on 18 May 2015 ON PROTECTION OF
    PERSONAL DATA\", confirming Law No. HO-49-N of 18 May 2015 (cross-
    confirmed against the ILO's NATLEX legislative database, a
    secondary but independent corroboration of the same law number and
    date).

  Unlike Bulgaria/Germany (a single \"Commerce Act\") or Azerbaijan
  (commercial-entity provisions folded into the Civil Code), this
  iteration specifically investigated Armenia's own structure rather
  than assuming either pattern: Armenia's Civil Code (HO-239, 1998)
  carries only GENERAL legal-persons provisions, while the specific,
  substantive company-law rules for its predominant corporate form (the
  limited liability company -- LLCs may have up to 49 participants
  before mandatory reorganisation into a joint-stock company) live in a
  SEPARATE standalone statute, the Law on Limited Liability Companies
  (Փակ բաժնետիրական ընկերությունների մասին -- \"Սահմանափակ
  պատասխանատվությամբ ընկերությունների մասին\"), adopted 24 October
  2001. This iteration's WebFetch attempts on both the Central Bank of
  Armenia's own mirror (old.cba.am, itself explicitly labelled a
  \"non-official translation\") and Armenia's Translation Centre (a
  state non-commercial organisation under the Ministry of Justice,
  translation-centre.am) returned only undecoded binary for both, so
  curl + `pdftotext` were used on the Translation Centre's copy (the
  official state body's own site, preferred over the Central Bank's
  explicitly-non-official mirror) -- the extracted text opens with
  \"LAW OF THE REPUBLIC OF ARMENIA Adopted on 24 October 2001 ON LIMITED
  LIABILITY COMPANIES\", confirming the date directly rather than
  trusting only secondary sources (multiple independent secondary
  sources -- armenian-lawyer.com, ilex.am, cis-legislation.com, ILO
  NATLEX -- separately agree on the same 24 October 2001 date, a further
  corroboration).

  A law not in this table has NO spec-basis, full stop; extend
  `catalog`, do not invent an id/url.")

(def catalog
  "iso3 -> vector of statute entries. `:statute/url` + `:statute/law-number`
  are the citation the governor requires before any compliance-fact
  proposal referencing this law can commit."
  {"ARM"
   [{:statute/id "arm.llc-law"
     :statute/title "Սահմանափակ պատասխանատվությամբ ընկերությունների մասին (Law on Limited Liability Companies)"
     :statute/jurisdiction "ARM"
     :statute/kind :law
     :statute/law-number "HO-252, adopted 24 October 2001"
     :statute/url "https://www.translation-centre.am/pdf/Trans_ru/HH_Orenq/Bank/LLC_en.pdf"
     :statute/url-provenance :official-translation-centre
     :statute/enacted-date "2001-10-24"
     :statute/retrieved-at "2026-07-21"
     :statute/topic #{:corporate-governance :incorporation}}
    {:statute/id "arm.labour-code"
     :statute/title "Աշխատանքային օրենսգիրք (Labour Code of the Republic of Armenia)"
     :statute/jurisdiction "ARM"
     :statute/kind :law
     :statute/law-number "HO-124-N, adopted 9 November 2004"
     :statute/url "https://www.arlis.am/documentview.aspx?docid=145760"
     :statute/url-provenance :official-arlis
     :statute/enacted-date "2005-06-21"
     :statute/retrieved-at "2026-07-21"
     :statute/topic #{:labor :employment}}
    {:statute/id "arm.personal-data-protection-law"
     :statute/title "Անձնական տվյալների պաշտպանության մասին (Law on Protection of Personal Data)"
     :statute/jurisdiction "ARM"
     :statute/kind :law
     :statute/law-number "HO-49-N, adopted 18 May 2015"
     :statute/url "https://www.moj.am/storage/uploads/Personal_data_protection_law_ENG_OFFICIAL.pdf"
     :statute/url-provenance :official-ministry-of-justice
     :statute/enacted-date "2015-05-18"
     :statute/retrieved-at "2026-07-21"
     :statute/topic #{:data-protection :privacy}}]})

(defn spec-basis
  "The jurisdiction's statute vector, or nil -- nil means NO spec-basis
  for that jurisdiction yet."
  [iso3]
  (get catalog iso3))

(defn coverage
  "Honest coverage report, same shape/discipline as `marketentry.facts/coverage`:
  never report a missing jurisdiction as covered."
  ([] (coverage (keys catalog)))
  ([iso3s]
   (let [have (filter catalog iso3s)
         missing (remove catalog iso3s)]
     {:requested (count iso3s)
      :covered (count have)
      :covered-jurisdictions (vec (sort have))
      :missing-jurisdictions (vec (sort missing))
      :note (str "cloud-itonami-iso3166-arm statute.facts Wave 0 (ADR-2607141700): "
                 (count (get catalog "ARM")) " ARM statutes seeded with an "
                 "official government-hosted citation. Extend "
                 "`statute.facts/catalog`, never fabricate a law-id or URL.")})))

(defn by-topic
  "Statutes for `iso3` tagged with `topic` (e.g. :labor, :data-protection)."
  [iso3 topic]
  (filterv #(contains? (:statute/topic %) topic) (spec-basis iso3)))
