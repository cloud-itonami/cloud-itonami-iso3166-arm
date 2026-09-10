(ns culture.facts
  "Country-level regional-culture catalog for Armenia (ARM) -- national
  dishes, protected products, beverages, crafts, festivals and heritage
  sites, per ADR-2607171400 addendum 2 (cloud-itonami-municipality-
  culture-catalog Wave 1, in com-junkawasaki/root). Sibling namespace to
  `marketentry.facts` / `statute.facts` (ADR-2607141700); city-level
  counterparts live in the cloud-itonami-municipality-* repos.

  Catalog is keyed by UPPERCASE ISO3 (mirrors `statute.facts`); entries
  carry no :culture/municipality (that attribute is city-level only).

  Every entry cites a source URL that was actually fetched and read on
  :culture/retrieved-at -- never fabricated. Summaries state only what the
  cited source confirms. An item not in this table has NO spec-basis, full
  stop; extend `catalog`, do not invent an id/url.")

(def catalog
  "iso3 -> vector of culture entries."
  {"ARM"
   [{:culture/id "arm.dish.harissa"
     :culture/name "Harissa"
     :culture/name-local "Հարիսա"
     :culture/country "ARM"
     :culture/kind :dish
     :culture/summary "Porridge of coarsely-ground wheat mixed with meat, of particular significance in Armenian cuisine, where it is traditionally served on Easter and regarded as a national dish; related versions (harees) are eaten across the Middle East."
     :culture/url "https://en.wikipedia.org/wiki/Harees"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "arm.dish.khorovats"
     :culture/name "Khorovats"
     :culture/name-local "Խորոված"
     :culture/country "ARM"
     :culture/kind :dish
     :culture/summary "Armenian grilled meat dish of skewered lamb, pork, beef, chicken or fish, typically served at festive occasions."
     :culture/url "https://en.wikipedia.org/wiki/Khorovats"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "arm.dish.lavash"
     :culture/name "Lavash"
     :culture/name-local "Լավաշ"
     :culture/country "ARM"
     :culture/kind :dish
     :culture/summary "Thin flatbread of West Asia and the South Caucasus, inscribed by UNESCO in 2014 as an expression of culture in Armenia; a separate 2016 listing acknowledges the bread is shared with Azerbaijan, Iran, Kazakhstan, Kyrgyzstan and Turkey."
     :culture/url "https://en.wikipedia.org/wiki/Lavash"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "arm.beverage.ararat"
     :culture/name "Ararat"
     :culture/name-local "ԱրԱրԱտ"
     :culture/country "ARM"
     :culture/kind :beverage
     :culture/summary "Brand of Armenian brandy produced since 1877, associated with the Yerevan Brandy Company."
     :culture/url "https://en.wikipedia.org/wiki/Ararat_(brandy)"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "arm.craft.khachkar"
     :culture/name "Khachkar"
     :culture/name-local "Խաչքար"
     :culture/country "ARM"
     :culture/kind :craft
     :culture/summary "Carved memorial cross-stone characteristic of medieval Christian Armenian art; khachkar symbolism and craftsmanship were inscribed on UNESCO's Intangible Cultural Heritage list in 2010."
     :culture/url "https://en.wikipedia.org/wiki/Khachkar"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "arm.festival.vardavar"
     :culture/name "Vardavar"
     :culture/name-local "Վարդավառ"
     :culture/country "ARM"
     :culture/kind :festival
     :culture/summary "Traditional Armenian water festival celebrated 98 days after Easter, in which participants drench each other with water, with roots in pre-Christian tradition."
     :culture/url "https://en.wikipedia.org/wiki/Vardavar"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "arm.heritage.haghpat"
     :culture/name "Haghpat Monastery"
     :culture/name-local "Հաղպատավանք"
     :culture/country "ARM"
     :culture/kind :heritage
     :culture/summary "Medieval Armenian monastery complex in Lori Province, inscribed with Sanahin as the UNESCO World Heritage Site Monasteries of Haghpat and Sanahin in 1996."
     :culture/url "https://en.wikipedia.org/wiki/Haghpat_Monastery"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}
    {:culture/id "arm.heritage.etchmiadzin"
     :culture/name "Etchmiadzin Cathedral"
     :culture/name-local "Էջմիածնի մայր տաճար"
     :culture/country "ARM"
     :culture/kind :heritage
     :culture/summary "Mother church of the Armenian Apostolic Church in Vagharshapat, built in the early fourth century and inscribed as a UNESCO World Heritage Site in 2000."
     :culture/url "https://en.wikipedia.org/wiki/Etchmiadzin_Cathedral"
     :culture/url-provenance :wikipedia-en
     :culture/retrieved-at "2026-07-17"}]})

(defn spec-basis [iso3] (get catalog iso3))

(defn coverage
  ([] (coverage (keys catalog)))
  ([iso3s]
   (let [have (filter catalog iso3s)
         missing (remove catalog iso3s)]
     {:requested (count iso3s)
      :covered (count have)
      :covered-jurisdictions (vec (sort have))
      :missing-jurisdictions (vec (sort missing))
      :note (str "cloud-itonami-iso3166-arm culture catalog "
                 "(ADR-2607171400 addendum 2, Wave 1): " (count (get catalog "ARM"))
                 " ARM entries, each with a fetched-and-read citation. "
                 "Extend `culture.facts/catalog`, never fabricate an id/url.")})))

(defn by-kind [iso3 kind]
  (filterv #(= (:culture/kind %) kind) (spec-basis iso3)))
