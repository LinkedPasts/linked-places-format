## The Linked Places format (LPF)

*v1.1, 9 May 2019*

#### NOTES (14 Jul 2019)
+ An [**alternative TSV\-format**](tsv_0.2.md) will be supported by World-Historical Gazetteer, appropriate for relatively simple place records, e.g. those without temporally scoped names, geometries, etc., and without multiple name variants including citations.

+ LPF v1.1 is implemented in current versions of World-Historical Gazetteer and Pelagios projects, including Recogito. There is a need to make improvements in a Version 2 and to develop/write the underlying ontology. Please consider joining a small working group in that effort.

+ Development of a related **Linked Traces annotation format** is under way, discussed in [its own repo](https://github.com/LinkedPasts/linked-traces-format) ]

-----
The Linked Places format supercedes the [Pelagios Gazetteer Interconnection Format (PGIF)](https://github.com/pelagios/pelagios-cookbook/wiki/Pelagios-Gazetteer-Interconnection-Format) as a template for contributions to both [Pelagios](http://commons.pelagios.org) (for Recogito & Peripleo) and [World-Historical Gazetteer](http://whgazetteer.org). Although these place data aggregation projects have distinctive features, both are building software tools and services allowing their users to:

- search across different gazetteers
- find enough information to identify and disambiguate places
- annotate data with stable URIs to the most appropriate gazetteer

<img style="border:0px;" height=225 src="https://pbs.twimg.com/media/DalrBTXXUAAY_lx.jpg" align=right alt="linked gazetteer entries in Peripleo"/></a>
Our goal is not to define *The One* unified data model to represent gazetteers. Historical research projects producing gazetteer data have distinctive data models reflecting their source data and project-specific requirements. Linked Places format provides a uniform way to build links between different gazetteers, along with just enough additional metadata to support the three requirements above.

The Linked Places format and the earlier PGIF are both valid RDF, the cornerstone format for [Linked Open Data](https://en.wikipedia.org/wiki/Linked_data) and the Semantic Web. Linked Places differs from PGIF in these ways:

<a href="https://json-ld.org/" title="JSON-LD"><img style="border:0px;" width="48" align= right src="https://json-ld.org/images/json-ld-logo-64.png" alt="JSON-LD-logo-64"/></a>

- it is designed primarily around [JSON-LD syntax](https://json-ld.org/spec/latest/json-ld/), which makes it both valid RDF (XML, Turtle, etc.) and JSON
- it is valid [GeoJSON](https://tools.ietf.org/html/rfc7946), therefore readily rendered in many web mapping applications; in fact, it is an implementation of [GeoJSON-T](https://github.com/kgeographer/geojson-t), an experimental extension to GeoJSON that standardizes the representation of temporal attributes
- it provides for optional temporal scoping of names, geometry, place types, and place relations

### An example Linked Places record
Contributions take the form of a [GeoJSON-LD](http://geojson.org/geojson-ld/) FeatureCollection containing one or more Feature objects. In order to index metadata about place records from multiple gazetteers, Linked Places accommodates these attribute elements: **id**, **title**, **ccodes**, **names***, **types***, **geometry***, **relations***, **descriptions**, **depictions**, **links**, and **when** (asterisk * indicates elements that can be temporally scoped).

All property labels (keys) are aliases for terms formally defined in several linked ontologies; mappings for them are listed in [this JSON-LD context document](https://raw.githubusercontent.com/LinkedPasts/linked-places/master/linkedplaces-context-v1.jsonld), and informal notes about them appear below. Several terms introduced in the Linked Places format will need be defined in a new Linked Pasts Ontology (lpo:) [*coming soon*].

Various serializations of the following sample record can be [explored in the JSON-LD Playground](http://tinyurl.com/y23ezgdf). Because Linked Places format is valid GeoJSON, the collection is also mappable, as seen in this [geojson.io-generated Gist](https://gist.github.com/kgeographer/9cf3441a99b8aa3bc25d464a3de920db) and rendered [automatically in GitHub](https://github.com/LinkedPasts/linked-places/blob/master/linkedplaces-sample-v1.json).

```
{
  "type": "FeatureCollection",
  "@context": "https://raw.githubusercontent.com/LinkedPasts/linked-places/master/linkedplaces-context-v1.1.jsonld",
  "features": [
    { "@id": "http://mygaz.org/places/p_12345",
      "type": "Feature",
      "properties":{
        "title": "Abingdon (UK)",
        "ccodes": ["GB"]
      },
      "when": {
        "timespans": [{"start": {"in":"0676"},"end": {"in":"1066"}}],
        "periods": [
          { "name": "Anglo-Saxon Period, 449-1066",
            "@id": "periodo:p06c6g3whtg"},
          { "name": "Anglo-Saxon (culture or style)",
            "@id": "http://chronontology.dainst.org/period/O5r960WKERYr"}
        ],
        "label": "sample 'when' w/timespans, periods, duration",
        "duration": "P100Y"
      },
      "names": [
        { "toponym":"Abingdon",
          "lang":"en",
          "citations": [
            {"label": "Ye Olde Gazetteer (1635)",
             "@id":"http://archive.org/details/yeoldegazetteer"}],
          "when": { "timespans":[{"start":{"in":"1600"}}]}
        },
        { "toponym":"Abingdon-on-Thames", "lang":"en",
          "when": {"timespans":[{"start":{"in":"1600"}}]}
        }
      ],
      "types": [
        { "identifier": "aat:300008375",
          "label": "town",
          "sourceLabels": [{"label":"Market Town","lang":"en"}],
          "when": {"timespans":[{"start":{"in":"1600"}}]} }
      ],
      "geometry": {
        "type": "GeometryCollection",
        "geometries": [
            { "type": "Point",
              "coordinates": [-1.2879,51.6708],
              "when": {"timespans":[
                {"start":{"in":"1600"},"end":{"in":"1699"}}]},
              "citations": [
                {"label": "Getty TGN (retrieved 4 May 2018)",
                 "@id":"tgn:7011944"}],
              "certainty": "certain"
            },
            { "type": "Point",
              "coordinates": [-1.31,51.64],
              "geo_wkt": "POLYGON ((-1.3077 51.6542, -1.2555 51.6542, -1.2555 51.6908, -1.3077 51.6908, -1.3077 51.6542))",
              "when": {"timespans":[{"start":{"in":"1700"}}]},
              "certainty": "uncertain"
            }
        ]
      },
      "links": [
        {"type": "exactMatch", "identifier": "http://vocab.getty.edu/tgn/7011944"},
        {"type": "exactMatch", "identifier": "http://www.geonames.org/2657780/"},
        {"type": "closeMatch", "identifier": "http://somegaz.org/places/39847"},
        {"type": "primaryTopicOf", "identifier": "https://en.wikipedia.org/wiki/Abingdon-on-Thames"},
        {"type": "subjectOf", "identifier": "http://www.visionofbritain.org.uk/travellers/Camden/11#pn_3"},
        {"type": "seeAlso", "identifier": "https://en.wikipedia.org/wiki/%C3%86bbe_of_Coldingham"}
      ],
      "relations": [
        { "relationType": "gvp:broaderPartitive",
          "relationTo": "http://mygaz.org/places/p_9876",
          "label": "part of Berkshire (UK)",
          "when": {"timespans":[
            {"start":{"in":"1600"},"end":{"in":"1974"}}
          ]}
        },
        { "relationType": "gvp:broaderPartitive",
          "relationTo": "http://mygaz.org/places/p_3456",
          "label": "part of Oxfordshire (UK)",
          "when": {"timespans":[{"start":{"in":"1974"}}]}
        },
        { "relationType": "gvp:tgn3000_related_to",
          "relationTo": "http://mygaz.org/places/p_98765",
          "label": "Linked to Semington by Kennet and Avon Canal",
          "when":{"timespans":[
            {"start":{"in":"1790"} }]},
          "citations": [
            {"label": "Harrumph (1923)",
             "@id": "doi:10.1109/5.771073"}],
          "certainty": "certain"
        }
      ],
      "descriptions": [
        { "@id": "https://en.wikipedia.org/wiki/Abingdon-on-Thames",
          "value": "...a historic market town and civil parish...",
          "lang": "en"
        }
      ],
      "depictions": [
        { "@id": "https://commons.wikimedia.org/wiki/File:ThamesAtAbingdon.jpg",
          "title": "The River Thames at Abingdon, Oxfordshire",
          "license": "cc:by-sa/3.0/"
        }
      ]
    }
  ]
}

```

### Linked Places Feature elements

Feature elements are either ***required***, ***encouraged***, or ***optional***. The encouraged elements will facilitate reconciliation and/or provide richer search results and record displays in both World-Historical Gazetteer and Peripleo.

#### **`@context`** (_required_)

In JSON-LD, labels for object elements are aliases for terms formally defined in linked ontologies. For Linked Places, those mappings are listed in [this context document](http://linkedpasts.org/assets/linkedplaces-context.jsonld).

e.g. `"@context": "http://linkedpasts.org/assets/linkedplaces-context-v1.jsonld"`

#### **`@id`** (_required_)

A unique and permanent URI pointing to the contributor's published record of the place. All place records contributed to World-Historical Gazetteer will have a minimal landing page, so it can serve as publisher for smaller projects.

e.g. `"@id": "http://mygaz.org/places/p_12345"`

#### **`properties{}`** (_required_)

A **properties** element holding at least one key:value pair is required by GeoJSON. For Linked Places format, **title** is required and one or more **ccodes** are encouraged. Properties are typically displayed in popup windows upon clicking markers in web maps.

e.g. ```"properties":{ "title": "Abingdon (UK)", "ccodes": ["GB"]}```

#### **`title`** (_required_)

A label for the record, usually a 'preferred name' from among the toponyms associated with a place. The **title** is necessary for ordering records in some list displays. Note that for Pelagios and World-Historical Gazetteer interfaces, place records always include *all* available attested name variants and spellings.

#### **`ccodes[]`** (_encouraged_)

A set (list) of one or more two-letter codes ([ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)) indicating modern countries whose bounds contains or overlap the place. Invaluable for reconciliation against modern place name authority resources like Getty TGN, Wikidata, and GeoNames.

#### **`when{}`** (_optional_)

The optional **when** element can be used to temporally scope a) an entire Feature; b) a **name**; c) a **geometry** (representative point or extent); d) a **type**; or e) a **relation**, i.e. an instance of a *part-of* relation with another place

A **when** element, if present, must include either one or more **timespan** or one or more named **period**, referenced with a name and URI. Examples shown are from [PeriodO](http://perio.do/) and [Chronontology](http://chronontology.dainst.org/).

A **timespan** must have a start; if end is omitted, the timespan is interpreted as the interval described by the start. For example, ```{"start":{"in":"1832"}``` indicates "during 1832," and ```{"start":{"in":"1832-08-01"}``` indicates "on that day."

Valid values for "in," "earliest," and "latest" are ISO 8601 expressions as described by the [OWL-Time ontology](https://www.w3.org/TR/owl-time/). Linked Places also supports the use of three operators occuring at the end of an ISO 8601 date string, as defined in [the draft ISO 8601-2 extension](https://www.loc.gov/standards/datetime/iso-tc154-wg5_n0039_iso_wd_8601-2_2016-02-16.pdf): '**~**' for '**approximate**', '**?**' for '**uncertain**', and '**%**' for '**both approximate and uncertain**'. If one of these is included, its associated term will appear alongside the date expression. This limited support for ISO 8601-2 level 1 will afford interesting possibilities for temporal visualizations in the future.

Valid values for "duration" are strings wtih the letter 'P' followed by an integer, followed by one of Y, M, W, or D to indicate years, months, weeks, or days. E.g. **P100Y** indicates one century with unspecified bounds within an accompanying timespan.

The following annotated example indicates possible options:

```
"when": {
  "timespans": [
    { "start": { "in": "yyyy-mm?" },
      "end": {
          "earliest": "-yyyy%",
          "latest": "yyyy-mm-dd" }
    }
  ],
  "periods": [
    { "name": "Anglo-Saxon Period, 449-1066",
      "uri": "http://n2t.net/ark:/99152/p06c6g3whtg" }
  ],
  "label": "for a century during the Anglo-Saxon period",
  "duration": "P100Y"
}

```

#### **`names[]`** (_required_)

A set (list) of one or more attested toponyms. At least one must have a citation. Temporal scoping with an associated 'when' element is optional, as are citations for all but the first. This allows for lists of uncited named variants.

For example:

```
"names": [
  { "toponym":"Abingdon",
    "lang":"en",
    "citations": [{
      "label": "Ye Olde Gazetteer (1635)",
      "@id":"http://archive.org/details/yeoldegazetteer"
    }],
    "when": {"timespans":[{"start":{"in":"1600"}}]}
  },
  { "toponym":"Abingdon-on-Thames", "lang":"en",
    "when": {"timespans":[{"start":{"in":"1600"}}]}
  }
],
```
#### **`types[ ]`** (_encouraged_*)
A set (list) of one or more place types, where `"identifier"` and `"label"` refer to a concept in a published vocabulary. This example indicates a term from the Getty Institute Art and Architecture Thesaurus (AAT). The `"sourceLabels"` attribute can be used for terms from the original source (or the contributor's internal vocabulary). [NOTE: World Historical Gazetteer has developed a subset list of 160 AAT place type concepts for use in that platform ([tsv](aat_whg-subset.tsv); [xlsx showing hierarchy](aat_whg-subset.xlsx).]

*`sourceLabels` and `when` are optional

```
"types": [
  { "identifier": "http://vocab.getty.edu/aat/300008375",
    "label": "town",
    "sourceLabels": [{"label":"Market Town","lang":"en"}],
    "when": {"timespans":[{"start":{"in":"1600"}}]}
  }
],

```

#### **`geometry{}`** (_required_)
A GeoJSON GeometryCollection having one or more geometry elements, optionally temporally scoped with a "when" element. NB: if a geometry type is anything but a "Point" (a single coordinate pair), the dataset *will not currently validate as JSON-LD*. However, this will not prevent its indexing in either Pelagios or World-Historical Gazetteer, the APIs for which will both provide valid RDF serializations in any event.

A [Well-known text (WKT)](https://en.wikipedia.org/wiki/Well-known_text)  representation of geometry ("geo_wkt") can be supplied in place of a "coordinates" element (e.g. geometry #2 below) , but in this case *the entire dataset will not validate as GeoJSON*. It will however index successfully in Pelagios and World-Historical Gazetteer and render in the maps of those projects.

```
"geometry": {
  "type": "GeometryCollection",
  "geometries": [
      { "type": "Point",
        "coordinates": [-1.2879,51.6708],
        "geo_wkt": "POINT(-1.2879 51.6708)",
        "when": {"timespans":[{"start":"1600","end":"1699"}]},
        "certainty": "less-certain"
      },
      { "geo_wkt": "POLYGON ((-1.3077 51.6542, -1.2555 51.6542,
            -1.2555 51.6908, -1.3077 51.6908, -1.3077 51.6542))",
        "when": {"timespans":[{"start":"1700"}]},
        "certainty": "certain"
      }
  ]
}
```
* In the event the location for a place is unknown, the "geometries" array should contain "null", e.g.
* Values for the optional `certainty` attribute can be one of "certain", "less-certain" and "uncertain".


```
"geometry": {
  "type": "GeometryCollection",
  "geometries": ["null"]
}
```
#### **`links[]`** (_encouraged_)
Linked Places format supports five types of linked resources, as shown here. Exact and close matches are the principal means of linking places and gazetteer datasets and are therefore at least one of these is ***highly encouraged***. The reconciliation service APIs of Pelagios and World-Historical Gazetteer (future) can facilitate identifying closeMatch and exactMatch relations with place name authorities.

```
"links": [
  {"type": "exactMatch",
   "identifier": "http://vocab.getty.edu/tgn/7011944"},
  {"type": "closeMatch",
   "identifier": "http://somegaz.org/places/39847"},
  {"type": "primaryTopicOf",
   "identifier": "https://en.wikipedia.org/wiki/Abingdon-on-Thames"},
  {"type": "subjectOf",
   "identifier": "http://www.visionofbritain.org.uk/travellers/Camden/11#pn_3"},
  {"type": "seeAlso",
   "identifier": "https://en.wikipedia.org/wiki/%C3%86bbe_of_Coldingham"}
],

```

#### **`relations[]`** (_optional_)
A set (list) of one or more attestation. the relationType property must be de-referenceable to an existing vocabulary or ontology. E.g., in the [Getty Vocabulary Ontology](http://vocab.getty.edu/ontology), **broaderPartitive** relations are used to represent 'parents' in an administrative hierarchy; **tgn3317\_member\_of** and **tgn3318\_member\_is** relations can be used to represent political unions, empires, and regions. In this example Abingdon is shown as having been an administrative part of two counties over time; also, using the generic **tgn3000\_related_to**, as having been linked by canal to Semington.

* Values for the optional `certainty` attribute can be one of "certain", "less-certain" and "uncertain".

```
"relations": [
  { "relationType": "gvp:broaderPartitive",
    "relationTo": "mygaz:places/p_9876",
    "label": "part of Berkshire (UK)",
    "when": {"timespans":[
      {"start":{"in":"1600"},"end":{"in":"1974"}}
    ]}
  },
  { "relationType": "gvp:broaderPartitive",
    "relationTo": "mygaz:places/p_3456",
    "label": "part of Oxfordshire (UK)",
    "when": {"timespans":[{"start":{"in":"1974"}}]}
  },
  { "relationType": "gvp:tgn3000_related_to",
    "relationTo": "http://mygaz.org/places/p_98765",
    "label": "Linked to Semington by Kennet and Avon Canal",
    "when":{"timespans":[
      {"start":{"in":"1790"} }]},
    "citations": [{
      "label": "Harrumph (1923)",
      "@id": "doi:10.1109/5.771073"
    }],
    "certainty": "certain"
  }
],

```
#### **`descriptions[]`** (_encouraged_)
A set (list) of one or more brief descriptions.

e.g.

```
"descriptions": [
  {
    "value": "...a historic market town and civil parish in the ceremonial county of Oxfordshire, England",
    "lang": "en",
    "source": "https://en.wikipedia.org/wiki/Abingdon-on-Thames"
  }
]
```


#### **`depictions[]`** (_optional_)
A set (list) of one or more images of some part or aspect of the place.

```
"depictions": [
  {
    "@id": "https://commons.wikimedia.org/wiki/File:ThamesAtAbingdon.jpg",
    "title": "The River Thames at Abingdon, Oxfordshire",
    "license": "cc:by-sa/3.0/"
  }
]
```
