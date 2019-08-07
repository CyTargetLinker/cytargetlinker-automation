# Creation of publication and journal linkset:

Look up WikiData identifier for every author that you want to include
e.g. Q27987764 for Martina Summer-Kutmon (https://www.wikidata.org/wiki/Q27987764)

Run the following query on query.wikidata.org (adapt ?work wdt:P50 wd:xxxxx with the author id)
```
SELECT
  ?work ?workLabel ?pmid
  (GROUP_CONCAT(DISTINCT ?type_label; separator=", ") AS ?type)
  (SAMPLE(?pages_) AS ?pages)
  ?venue ?venueLabel
  (GROUP_CONCAT(DISTINCT ?author_label; separator=", ") AS ?authors)
WHERE {
  ?work wdt:P50 wd:Q27987764.
  ?work wdt:P50 ?author .
  ?work wdt:P31 wd:Q13442814 .
  OPTIONAL {
    ?author rdfs:label ?author_label_ . FILTER (LANG(?author_label_) = 'en')
  }
  BIND(COALESCE(?author_label_, SUBSTR(STR(?author), 32)) AS ?author_label)
  
  OPTIONAL { ?work wdt:P1104 ?pages_ }
  OPTIONAL { ?work wdt:P1433 ?venue }
  OPTIONAL { ?work wdt:P698 ?pmid }
 
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en,da,de,es,fr,jp,no,ru,sv,zh". }  
}

GROUP BY ?work ?workLabel ?venue ?venueLabel ?pmid
ORDER BY DESC(?date)    
```

Put the results of all authors into one spreadsheet and run the linkset-creator to create one author-publication and one publication-journal linkset. 
https://github.com/CyTargetLinker/linksetCreator/releases
