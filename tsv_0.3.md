# Linked Places TSV v0.3 (LP-TSV)

_15 Sep 2019_; _updated 18 Jan 2021_

The following TSV place data format will be supported for contributions to the World Historical Gazetteer (WHG) system as a simplified alternative to the more expressive default, [Linked Places (LP) format](README.md). LP-TSV is suitable for relatively simple place records, e.g. those without a) temporal scoping for names, geometries, types, or relations; and/or b) citations for name variants. 

NOTE: These spreadsheet templates ([.xlsx](LP-TSV_template.xlsx) [.ods](LP-TSV_template.ods)) can be used to build a valid LP-TSV file for upload. 

1. Determine mappings between source and template columns
2. Copy/paste data accordingly
3. Fill in any missing **required** values, and **encouraged** or **optional** as desired, e.g. sort by your **type** and add mapped **aat_types** identifiers
4. Delete unused columns and extraneous notes
5. Save 

These spreadsheets, **or alternatively, any .csv, .tsv, .xlsx or .ods file with fields and formats adhering to the specification below** can be uploaded. All LP-TSV files must have UTF-8 encoding. Where multiple values are allowed within a field (indicated below), they are unquoted and delimited with semicolons. 

The following fields will be parsed and converted to Linked Places format automatically upon upload to WHG.

-----
**NB** Because place names are so often repeated globally, records consisting of only the required fields (__id__, __title__, __title_source__ and __start__) are exceedingly difficult to reconcile (i.e. match) with records in existing gazetteer data resources such as Getty TGN, Wikidata, DBpedia, and GeoNames. Additional context, e.g. **ccodes**, **variants**,  **types**, geometry (**lon** and **lat** or **geo_wkt**), and **parent_name** helps considerably.

-----

## Fields

### _## required ##_
**id**

Contributor's internal identifier. This must stay consistent throughout accessioning workflow, including subsequent updates

**title**

A single "preferred" place name, which acts as title for the record

**title\_source**

Label or short citation for source of the title toponym, in any style; e.g. 'An Historical Atlas of Central Asia (Bregel, 2003)', 'The Historical Sample of the Netherlands (https://iisg.amsterdam/nl/hsn)'.

**start**

Earliest relevant date in ISO 8601 form (YYYY-MM-DD); omit month and/or year as req. BCE years must expressed as negative integer, e.g. -0320 for 320 BCE.


>**NOTES**
>
>- A **start** date is _required_ and **end** date is _optional_ . Both refer to the entire place record. Use start & end values in combination to indicate a valid period. A start value alone typically indicates the publication date of the **title_source**. The start and end values correspond to a **timespan** within a "when" object _at the record level_ in the full [Linked Places JSON-LD format](https://github.com/LinkedPasts/linked-places).



### _## encouraged ##_
**title\_uri**

Permalink URI for the source of the title toponym, if available: a text, a map, an inscription, a dataset, etc.; e.g. _http://www.worldcat.org/oclc/890934416_ or _https://doi.org/10.7910/DVN/AMPGMW_ or _https://www.davidrumsey.com/luna/servlet/s/em7n8q_

**ccodes**

One or more [ISO Alpha-2 two-letter codes] (https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for **modern** countries that overlap or cover the place in question. 

>*NOTES*
>
>- These are used to generate a constraining spatial extent for searches, and **not** interpreted as assertions of a relation between the entities.  _semicolon-delimited_
>- If omitted and geometry is provided (**lon** and **lat** or **geo_wkt**), these will be computed automatically upon upload.

**matches**

One or more URIs for matching record(s) in place name authority resources; interpreted as [SKOS:closeMatch](https://www.w3.org/TR/2009/REC-skos-reference-20090818/#L4858), which is "used to link two concepts that are sufficiently similar that they can be used interchangeably in some information retrieval applications" and is inclusive its sub-property SKOS:exactMatch. _semicolon-delimited_. 

World Historical Gazetteer supports the name resources listed here; the aliases indicated (short URI prefixes) can be used in place of the URI base, e.g. `wd:Q5684` for the Wikidata Babylon record.

```

    {"bnf": Bibliothèque nationale de France, "https://data.bnf.fr/"} 
    {"cerl": Consortium of European Research Libraries, "https://data.cerl.org/thesaurus/"}
    {"dbp": DBpedia, "http://dbpedia.org/resource/"}
    {"gn": GeoNames, "http://www.geonames.org/"}
    {"gnd": Deutschen Nationalbibliothek, "http://d-nb.info/gnd/"}
    {"gov": The Geneaological Gazetteer, "http://gov.genealogy.net/" }
    {"loc": Library of Congress, "http://id.loc.gov/authorities/subjects/"}
    {"pl": Pleiades, "https://pleiades.stoa.org/places/"}
    {"tgn": Getty Thesaurus of Geographic Names, "http://vocab.getty.edu/page/tgn/"}
    {"viaf": Virtual International Authority File, "http://viaf.org/viaf/"}
    {"wd": Wikidata, "https://www.wikidata.org/wiki/"}
    {"wp": Wikipedia, "https://wikipedia.org/wiki/"}
```

**variants**

One or more name and/or language variants; can be suffixed with language-script codes if available, per IETF best practices, [BCP 47](https://www.rfc-editor.org/rfc/bcp/bcp47.txt) using ISO639-1 2-letter codes for language and ISO15924 4-letter codes for script; e.g. {name}@lang-script. **NB** Omit script code if it is the default for a language. _semicolon-delimited_. 

**types**

One or more terms for place type (contributor's term, usually verbatim from the source, e.g. _pueblo_). _semicolon-delimited_.


**aat_types**		

One or more AAT integer IDs from WHG's subset list of 160 place type concepts ([tsv](feature-types-AAT_20210118.tsv); [xlsx showing hierarchy](feature-types-AAT_20210118.xlsx). While not required, this mapping will make records more discoverable in WHG interfaces. _semicolon-delimited_.

>*NOTES*
>
>- **aat_types** should correspond to **types**, 1-to-1. If there is no corresponding aat\_type, leave its position empty. E.g. If there are 3 types for a record and only those in positions 2 and 3 have a corresponding aat\_type, this field's value would be something like **1234567;2345678**.
>- Do not replicate the AAT hierarchy; e.g. if a type is 'city', the aat_type is 300008389 (city). Do not include its parent 300008347 (inhabited place).

### _## optional ##_

**parent_name**

A containing administrative area or region, such as a province, state, country - alone or in some combination; e.g. _Lombardy_ or _Lombardia, Italia_ or _Illyricum_. For modern country alone, use **ccodes** (above). Provides useful context for alignment/reconciliation steps.

**parent_id**

URI for a web-published record of the parent_name above.

_FUTURE_: Pointer to another record in the same dataset file, consisting of a '#' character followed by the **id** of the parent record; e.g. _#1234_


**lon**

Longitude, in WGS84 decimal degrees (EPSG:4326); e.g. _85.3214_ or _-112.4536_

**lat**

Latitude, in WGS84 decimal degrees (EPSG:4326); e.g. _59.3345_ or _-12.7658_

**geowkt**

Any geometry in OGC [WKT format](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry).

>*NOTES*
>
>- polygons should ideally be simplified to aid rendering performance, using e.g. a GIS function or [MapShaper](https://mapshaper.org/)
>- geowkt will supercede lon/lat pair if both are present; used typically for non-point geometry

**geo_source**

Label for source of the geometry, e.g. _GeoNames_ or _digitized title_source map_

**geo_id**

URI identifier for the source of the geometry, e.g.  http://www.geonames.org/2950159

**end**

Latest relevant date, as above

**description**

A short text description of the place

-----
_@kgeographer; 2020-12-25_
