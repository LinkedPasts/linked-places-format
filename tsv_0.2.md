# Linked Places TSV v0.2 (LP-TSV)

_Draft for comment, 10 Sep 2019_

The following TSV place data format will be supported for contributions to the World-Historical Gazetteer (WHG) system as a simplified alternative to the more expressive default, [Linked Places (LP) format](README.md). LP-TSV is suitable for relatively simple place records, e.g. those without a) temporal scoping for names, geometries, types, or relations; and/or b) citations for name variants. [Samples](tsv_examples_0.2.md).

LP-TSV files are unicode text (utf-8). Fields (columns) must be delimited with a tab character. Where multiple values are allowed within a field (indicated below), they are unquoted and delimited with semicolons. The following fields will be parsed and converted to Linked Places format automatically upon upload to WHG.

-----
**NB** Records consisting of only the required __id__, __title__ and __title_source__ are exceedingly difficult to reconcile (i.e. match) with records in existing gazetteer data resources such as Getty TGN, Wikidata, DBpedia, and GeoNames. Additional context, e.g. **ccodes**, **variants**,  **types** and geometry help considerably.

-----

## Fields

### _## required ##_
**id**
	
Contributor's internal identifier. This must stay consistent throughout accessioning workflow, including subsequent updates

**title**

A single "preferred" place name, which acts as title for the record

**title\_source**

Label or short citation for source of the title toponym

### _## encouraged ##_
**title\_uri**

URI for the source of the title toponym

**ccodes**

One or more [ISO Alpha-2 two-letter codes] (https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for **modern** countries that overlap or cover the place in question. **N.B.** These are used to generate a constraining geometry for searches, and not interpreted as assertions of a relation between the entities.  _semicolon-delimited_

**matches**

One or more URIs for matching record(s) in place name authority resources; interpreted as [SKOS:closeMatch](https://www.w3.org/TR/2009/REC-skos-reference-20090818/#L4858), which is "used to link two concepts that are sufficiently similar that they can be used interchangeably in some information retrieval applications" and is inclusive its sub-property SKOS:exactMatch. _semicolon-delimited_

**variants**

One or more name and/or language variants; can be suffixed with language-script codes if available, per IETF best practices, [BCP 47](https://www.rfc-editor.org/rfc/bcp/bcp47.txt) using ISO639-1 2-letter codes for language and ISO15924 4-letter codes for script; e.g. {name}@lang-script. **NB** Omit script code if it is the default for a language. _semicolon-delimited_

**types**

One or more terms for place type (contributor's term, usually verbatim from the source, e.g. pueblo), followed by a '/' character then a Getty AAT integer ID from WHG's subset list of 160 place type concepts ([tsv](aat_whg-subset.tsv); [xlsx showing hierarchy](aat_whg-subset.xlsx). While not required, this mapping will make records more discoverable in WHG interfaces. _semicolon-delimited_

e.g. Pueblo/300008372 maps the original term 'Pueblo' to 'Village' in AAT.


**aat_types**		

One or more AAT integer IDs from WHG's subset list of 160 place type concepts ([tsv](aat_whg-subset.tsv); [xlsx showing hierarchy](aat_whg-subset.xlsx). While not required, this mapping will make records more discoverable in WHG interfaces. _semicolon-delimited_


### _## optional ##_

**parent_name**

A single toponym for a containing place

**parent_id**

URI for a web-published record of the parent_name above

**lon**				

Longitude, in decimal degrees

**lat**

Latitude, in decimal degrees

**geowkt**

Any geometry in OGC [WKT format](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry). 

>NOTES
>
>- polygons should ideally be simplified to aid rendering performance, using e.g. a GIS function or [MapShaper](https://mapshaper.org/)
>- geowkt will supercede lon/lat pair, if both are supplied; used typically for non-point geometry

**geo_source**

Label for source of the geometry, e.g. GeoNames

**geo_id**

URI identifier for the source of the geometry, e.g.  http://www.geonames.org/2950159

**start**

Earliest relevant date in ISO 8601 form (YYYY-MM-DD); omit month and/or year as req. BCE years must expressed as negative integer, e.g. -0320 for 320 BCE. To express a range, use a pair of dates, e.g. -0299/-0200 would indicate "starting in 3rd century BCE."


>**NOTES**
>
>- Start and end dates are _optional_ attributes referring to the entire place record, and used to indicate a valid period of existence or use given by its attestion in **title_source**. They correspond to a **timespan** within a "when" object _at the record level_ in the full [Linked Places JSON-LD format](https://github.com/LinkedPasts/linked-places).
>
>- Level 0 and much of Level 1 of the [Extended Date/Time Format (EDTF) specification](https://www.loc.gov/standards/datetime/edtf.html) of the US Library of Congress will be supported by WHGazetteer _upon its launch in Spring 2020_. Features include:
>    - Characters '?', '~' and '%', used to mean "uncertain", "approximate", and "uncertain as well as approximate" respectively can be appended to any date expression.
>    - Intervals with open or unknown start or end can be indicated by '..' or an empty string respectively, e.g. ‘1885/..’ (open end) or '/1885' (unknown start).
>    - Negative calendar years are already supported per above.


**end**

Latest relevant date, as above; pair indicates ending range

**description**

A short text description of the place

-----
_@kgeographer; 20190819_