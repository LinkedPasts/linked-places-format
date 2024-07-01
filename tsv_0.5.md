# LP-TSV v0.5 (Linked Places delimited derivative)

_updated 25 June 2024: LP-TSV now requires either an fclasses or aat_types property/column, and each row in the must have a value in either (or both)._

_updated 4 July 2023; added 13 new Getty AAT place types to better support urban-scale places in WHG: well, tribal area, temple, synagogue, square, residential district, park, pagoda, neighborhood, mosque, church, castle, native peoples reservation_

_updated 3 November 2021; added "attestation\_year" column and conditions for uses of "start", "end", and "attestation\_year"_

The following delimited place data format will be supported for contributions to the World Historical Gazetteer (WHG) system as a simplified alternative to the more expressive default, [Linked Places (LP) format](README.md). LP-Delimited is suitable for relatively simple place records, e.g. those without a) temporal scoping for names, geometries, types, or relations; and/or b) citations for name variants.

NOTE: These spreadsheet templates ([.xlsx](LP-TSV_template.xlsx) [.ods](LP-TSV_template.ods)) can be used to build a valid LP-Delimited file for upload. **As of Jan 2021, any .csv, .tsv, .xlsx or .ods file with fields and formats adhering to the specification below is suppported for upload to WHG. Tab-delimited files (.tsv) should have no quotes enclosing fields.**

1. Determine mappings between source and template columns
2. Copy/paste data accordingly
3. Fill in any missing **required** values, and **encouraged** or **optional** as desired, e.g. sort by your **type** and add mapped **aat_types** identifiers
4. Delete unused columns and extraneous notes
5. Save

These spreadsheets or alternatively, any .csv or .tsv file with fields and formats adhering to the specification below, can be uploaded in WHG. **All files must have UTF-8 encoding**. Where multiple values are allowed within a field (indicated below), they are unquoted and delimited with semicolons.

The following fields will be parsed and converted to Linked Places format automatically upon upload to WHG.

-----
**NB** Because place names are so often repeated globally, records consisting of only the required fields (__id__, __title__, __title_source__ and __start__ or __title\_source\_year__ ) are exceedingly difficult to reconcile (i.e. match) with records in existing gazetteer data resources such as Getty TGN, Wikidata, DBpedia, and GeoNames. Additional context, e.g. **variants** and **types**, geometry (**lon** and **lat** or **geowkt**), and **parent_name** helps considerably.

-----

## Fields

### _## required ##_
**id**

Contributor's internal identifier. This must stay consistent throughout accessioning workflow, including subsequent updates

**title**

A single "preferred" place name, which acts as title for the record

**title\_source**

Label or short citation for source of the title toponym, in any style; e.g. 'An Historical Atlas of Central Asia (Bregel, 2003)', 'The Historical Sample of the Netherlands (https://iisg.amsterdam/nl/hsn)'.

**fclasses \***

\* Each row *must* have either an **fclasses** _or_ **aat_types** value (see **aat_types** below).

One or more of the seven single-letter GeoNames Feature Classes below, delimited witha semicolon. e.g. ["P"], or ["P"; "A"]

**A: Administrative entities** (e.g. countries, provinces, municipalities);
**H: Water bodies** (e.g. rivers, lakes, bays, seas);
**L: Regions, landscape areas** (cultural, geographic, historical);
**P: Populated places** (e.g. cities, towns, hamlets);
**R: Roads, routes, rail**;
**S: Sites** (e.g. archaeological sites, buildings, complexes);
**T: Terrestrial landforms** (e.g. mountains, valleys, capes)

>NOTE
>
>The fclasses attribute is necessary to provide higher quality reconciliation results, by filtering out potential hits from irrelevant feature classes.

**aat_types \***		
\* Each row *must* have either a **fclasses** _or_ a **aat_types** value (see **fclasses** above).

One or more AAT integer IDs from WHG's subset list of 176 place type concepts ([tsv](feature-types-AAT_20230609.tsv); [xlsx showing hierarchy](feature-types-AAT_20230609.xlsx). While not required, this mapping is encouraged because it will make records more discoverable in WHG interfaces. _semicolon-delimited_.

>*NOTES ON TYPES*

>- A row can have a **types** value with no corresponding **aat\_types** value, but not the reverse; for every value in **aat\_types** there must be a corresponding value in **type**, 1-to-1
>- If there is no _aat\_type_ that corresponds to a _type_, leave its position empty. E.g. If there are 3 types for a record and only those in positions 2 and 3 have a corresponding aat\_type, this field's value would be something like **1234567;2345678**.
>- Do not replicate the AAT hierarchy; e.g. if a type is 'city', the aat_type is 300008389 (city). Do not include its parent 300008347 (inhabited place).

**attestation_year** and/or **start**

The **start** and **end** fields should be used specify a valid timespan for the place named by the title and variants, if they are known; otherwise they should be omitted. The **attestation_year** field is the publication year of the **title_source**.

Each row *must* have at least a **attestation_year** _or_ a **start**, and *may* have **start**, **end**, and **attestation_year**.
All of these *must* be written in ISO 8601 form (YYYY-MM-DD), omitting month and/or day where appropriate. BCE years must be written as a negative integer, e.g. -320 for 320 BCE.

>**NOTES**
>
>- Use start & end values in combination to indicate a valid period. A start value alone indicates "from", where end is unknown or is the present time. The start and end values correspond to a **timespan** within a "when" object _at the record level_ in the full [Linked Places JSON-LD format](https://github.com/LinkedPasts/linked-places).



### _## encouraged ##_
**title\_uri**

Permalink URI for the source of the title toponym, if available: a text, a map, an inscription, a dataset, etc.; e.g. _http://www.worldcat.org/oclc/890934416_ or _https://doi.org/10.7910/DVN/AMPGMW_ or _https://www.davidrumsey.com/luna/servlet/s/em7n8q_

**ccodes**

One or more [ISO Alpha-2 two-letter codes] (https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for **modern** countries that overlap or cover the place in question.

>*NOTES*
>
>- These are used to generate a constraining spatial extent for searches, and **not** interpreted as assertions of a relation between the entities.  _semicolon-delimited_
>- If omitted and geometry is provided (**lon** and **lat** or **geowkt**), these will be computed automatically upon upload.

**matches**

One or more URIs for matching record(s) in place name authority resources; interpreted as [SKOS:closeMatch](https://www.w3.org/TR/2009/REC-skos-reference-20090818/#L4858), which is "used to link two concepts that are sufficiently similar that they can be used interchangeably in some information retrieval applications" and is inclusive its sub-property SKOS:exactMatch. _semicolon-delimited_.

World Historical Gazetteer supports the name resources listed here; the aliases indicated (short URI prefixes) **must** be used in place of the URI base, e.g. `wd:Q5684` for the Wikidata Babylon record.

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

One or more terms for place type. This is the contributor's term, usually verbatim from the source, e.g. _pueblo_).  _semicolon-delimited_

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
_@kgeographer; 2024-06-24_
