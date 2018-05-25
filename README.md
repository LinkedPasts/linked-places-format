## Linked Places Interconnection Format

*Draft for comment, 25 May 2018. A new Linked Places Annotation Format to follow*

The Linked Places Interconnection format (LPIF) supercedes the [Pelagios Gazetteer Interconnection Format (PGIF)](https://github.com/pelagios/pelagios-cookbook/wiki/Pelagios-Gazetteer-Interconnection-Format) as a template for contributions to both [Pelagios](http://commons.pelagios.org) and [World-Historical Gazeetteer](http://whgazetteer.org). Although these place data aggregation projects have distinctive features, both are building software tools and services to allow everyone to:

- search across different gazetteers
- find enough information to identify and disambiguate places
- annotate data with stable URIs to the most appropriate gazetteer

<img style="border:0px;" height=225 src="https://pbs.twimg.com/media/DalrBTXXUAAY_lx.jpg" align=right alt="linked gazetteer entries in Peripleo"/></a> 
Our goal is not to define *The One* unified data model to represent gazetteers. Historical research projects producing gazetteer data have distinctive data models reflecting their source data and project-specific requirements. What we aim for is a uniform way to build links between different gazetteers, along with just enough additional metadata to support the three requirements above.

Both LPIF and the earlier PGIF are valid RDF, the cornerstone format for [Linked Open Data](https://en.wikipedia.org/wiki/Linked_data) and the Semantic Web. LPIF differs from PGIF in these ways:

<a href="https://json-ld.org/" title="JSON-LD"><img style="border:0px;" width="48" align= right src="https://json-ld.org/images/json-ld-logo-64.png" alt="JSON-LD-logo-64"/></a> 

- it is designed primarily around [JSON-LD syntax](https://json-ld.org/spec/latest/json-ld/), which makes it both valid RDF (XML, Turtle, etc.) and JSON
- it is valid [GeoJSON](https://tools.ietf.org/html/rfc7946), therefore readily rendered in many web mapping applications; in fact, it is an implementation of [GeoJSON-T](https://github.com/kgeographer/geojson-t), an experimental extension to GeoJSON that standardizes the representation of temporal attributes
- it provides for optional temporal scoping of place names, geometry (location/extent), and *part-of* relations

### An example LPIF record
Contributions take the form of a [GeoJSON-LD](http://geojson.org/geojson-ld/) FeatureCollection containing one or more Feature objects. In order to index metadata about place records from multiple gazetteers, LPIF accomodates these attribute elements: **id**, **title**, **ccode**, **namings**, **parthood**, **placetypes**, **geometry**, **descriptions**, **depictions**, **relations**, and **when**.

All property labels (keys) are aliases for terms formally defined in several linked ontologies; mappings for them are listed in [this context document]([http://linkedpasts.org/assets/lpif-context.jsonld), and informal notes about them appear below. Several terms introduced by LPIF will be defined in a new Linked Pasts Ontology (lpo:) [*coming soon*]. 

Various serializations of the following example can be [explored in the JSON-LD Playground](https://tinyurl.com/y9rwrpvc). The collection/record is also mappable, as seen in this [geojson.io-generated Gist](https://gist.github.com/kgeographer/9cf3441a99b8aa3bc25d464a3de920db) and rendered [automatically in GitHub](https://github.com/LinkedPasts/lpif/blob/master/example.json).

```
{
  "type": "FeatureCollection",
  "@context": "http://linkedpasts.org/assets/lpif-context.jsonld",
  "features": [
    { "@id": "mygaz:places/p_12345",
      "type": "Feature",
      "properties":{
        "title": "Abingdon (UK)",
        "ccode": "GB"
      },
      "namings": [
        { "toponym":"Abingdon", "lang":"en",
          "attestation": {
            "publisher": "http://pub.org/",
            "evidence": "http://pub.org/pubs/321/"
          },
          "when": {"timespans":[{"start":"1600"}]}
        },
        { "toponym":"Abingdon-on-Thames", "lang":"en",
          "when": {"timespans":[{"start":"1600"}]}
        }
      ],
      "parthood": [
        { "parent": "mygaz:places/p_9876",
          "parentLabel": "Berkshire (UK)",
          "when": {"timespans":[{"start":"1600","end":"1974"}]}
        },
        { "parent": "mygaz:places/p_3456",
          "parentLabel": "Oxfordshire (UK)",
          "when": {"timespans":[{"start":"1974"}]}
        }
      ],
      "placetypes": [
          {
            "@id": "aat:300008347",
            "label": "inhabited place"
          }
      ],
      "geometry": {
        "type": "GeometryCollection",
        "geometries": [
            { "type": "Point",
              "coordinates": [-1.2879,51.6708],
              "geo_wkt": "POINT(-1.2879 51.6708)",
              "when": {"timespans":[{"start":"1600","end":"1699"}]}
            },
            { "type": "Point",
              "coordinates": [-1.30,51.68],
              "geo_wkt": "POINT(-1.30 51.68)",
              "when": {"timespans":[{"start":"1700"}]}
            }
        ]
      },
      "descriptions": [
        {
          "value": "...a historic market town and civil parish in the ceremonial county of Oxfordshire, England",
          "lang": "en",
          "source": "https://en.wikipedia.org/wiki/Abingdon-on-Thames"
        }
      ],
      "depictions": [
        {
          "@id": "https://commons.wikimedia.org/wiki/File:ThamesAtAbingdon.jpg",
          "title": "The River Thames at Abingdon, Oxfordshire",
          "license": "cc:by-sa/3.0/"
        }
      ],
      "related": [
        {"exactMatch": "http://vocab.getty.edu/tgn/7011944" },
        {"closeMatch": "http://somegaz.org/places/39847" },
        {"primaryTopicOf": "https://en.wikipedia.org/wiki/Abingdon-on-Thames" },
        {"subjectOf": "http://www.visionofbritain.org.uk/travellers/Camden/11#pn_3" },
        {"seeAlso": "https://en.wikipedia.org/wiki/%C3%86bbe_of_Coldingham" }
      ],
      "when": {
        "timespans": [
          {  
            "start": {"in": "0676"}, "end": {"in": "1066"}
          }
        ],
        "periods": [
          {
            "name": "Anachronistic Period",
            "uri": "http://n2t.net/ark:/99152/p49ko209is"
          }
        ],
        "label": "'when' object to illustrate named period, duration, and sequence",
        "duration": "P100Y",
        "follows": "mygaz:places/p_9876"
      }
    }
  ]
}

```

### LPIF Feature elements

Feature elements are either ***required***, ***encouraged***, or ***optional***. The encouraged elements will facilitate reconciliation and/or provide richer search results and record displays in both World-Historical Gazetteer and Peripleo.

#### **`@context`**

In JSON-LD, labels for object elements are aliases for terms formally defined in linked ontologies. For LPIF, those mappings are defined in [this context document](http://linkedpasts.org/assets/lpif-context.jsonld). (***required***)

e.g. `"@context": "http://linkedpasts.org/assets/lpif-context.jsonld"`

#### **`@id`**

A unique and permanent URI pointing to the contributor's published record of the place. Because not every project can readily stand up such resource landing pages, a "csv-to-published LPIF" tool will be developed soon. (***required***)

e.g. `"@id": "mygaz:places/p_12345"`

#### **`properties{}`**

A **properties** element holding at least one key:value pair is required by GeoJSON. For LPIF, **title** is required and **ccode** is encouraged. Properties are typically displayed in popup windows upon clicking markers in web maps. (***required***)

e.g. ```"properties":{ "title": "Abingdon (UK)", "ccode": "GB"}```

#### **`title`**

A label for the record, usually a 'preferred name' from among the names associated with a place. The **title** is necessary for ordering records in some list displays. For Pelagios and World-Historical Gazetteer interfaces, place records always include *all* available attested name variants and spellings. (***required***)

#### **`ccode`** 

A two-letter code ([ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)) indicating a modern country that contains or overlaps the place. Invaluable for reconciliation against modern place name authority resources like Getty TGN and GeoNames. (***encouraged***)

#### **`when{}`**

The optional **when** element can be used to temporally scope a) an entire Feature; b) a **naming**; c) a **geometry** (representative point or extent); or d) a **parthood**, i.e. an instance of a *part-of* relation with another place.

A **when** element can consist of one or more **timespan** and/or one or more named **period**, referenced as a [PeriodO](http://perio.do/) URI.

Valid values for "in," "earliest," and "latest" are ISO 8601 expressions as described by the [OWL-Time ontology](https://www.w3.org/TR/owl-time/). LPIF also supports the use of three operators occuring at the end of an ISO 8601 date string, as defined in the draft ISO 8601-2 extension: '**~**' for '**approximate**', '**?**' for '**uncertain**', and '**%**' for '**both approximate and uncertain**'. If one of these is included, its associated term will appear alongside the date expression. This limited support for ISO 8601 level 1 will afford interesting possibilities for temporal visualizations in the future.

Valid values for "duration" are strings wtih the letter 'P' followed by an integer, followed by one of Y, M, W, or D to indicate years, months, weeks, or days. E.g. **P100Y** indicates one century with unspecified bounds within an accompanying timespan.

The following annotated example (##) indicates possible options:

```
"when": {
  "timespans": [
    {  
      "start": { "in": "yyyy-mm?" },
      "end": {  ## if omitted, typically interpreted as today()
          "earliest": "-yyyy%",
          "latest": "yyyy-mm-dd"
        },
    }
  ],
  "periods": [
    {
      "name": "Anachronistic Period",
      "uri": "http://n2t.net/ark:/99152/p0mn2ndq6bv"
    }
  ],
  "label": "for a decade during Anachronistic period",
  "duration": "P10Y",  ## optional
  "follows": "mygaz:places/p_9876"  ## optional URI to indicate sequence
}

```
(***optional***)

#### **`namings[]`**

A set (list) of one or more attested toponyms. Temporal scoping with an associated 'when' element is optional. (***required***)

For example:
 
```
"namings": [
  { "toponym":"Abingdon", "lang":"en",
    "attestation": {
      "publisher": "http://pub.org/",
      "evidence": "http://pub.org/pubs/321/"
    },
    "when": {"timespans":[{"start":"1600"}]}
  },
  { "toponym":"Abingdon-on-Thames", "lang":"en",
    "when": {"timespans":[{"start":"1600"}]}
  }
],
```
#### **`placetypes[]`**
A set (list) of one or more place types, where `"@id"` and "label" refer to concepts in a published vocabulary (in this example, the Getty Institute Art and Architecture Thesaurus (AAT). (***encouraged***)

```
"placetypes": [
    {
      "@id": "aat:300008347",
      "label": "inhabited place"
    }
]

```


#### **`geometry`**
A GeoJSON GeometryCollection having one or more geometry elements, optionally temporally scoped. NB: if a geometry type is anything but a "Point" (a single coordinate pair), the dataset *will not validate as JSON-LD*. However, this will not prevent its indexing in either Pelagios or World-Historical Gazetteer, the APIs for which will both provide valid RDF serializations in any event. (***required***)

A [Well-known text (WKT)](https://en.wikipedia.org/wiki/Well-known_text)  representation of geometry ("geo_wkt") can be supplied in place of a "coordinates" element (e.g. geometry #2 below) , but in this case *the entire dataset will not validate as GeoJSON*. It will however index successfully in Pelagios and World-Historical Gazetteer and render in the maps of those projects. 

```
"geometry": {
  "type": "GeometryCollection",
  "geometries": [
      { "type": "Point",
        "coordinates": [-1.2879,51.6708],
        "geo_wkt": "POINT(-1.2879 51.6708)",
        "when": {"timespans":[{"start":"1600","end":"1699"}]}
      },
      { "geo_wkt": "POLYGON ((-1.3077 51.6542, -1.2555 51.6542, 
            -1.2555 51.6908, -1.3077 51.6908, -1.3077 51.6542))",
        "when": {"timespans":[{"start":"1700"}]}
      }
  ]
}
```
In the event a place has no known coordinate location, the "geometries" array should contain a single JSON "null" value, e.g.

```
"geometry": {
  "type": "GeometryCollection",
  "geometries": ["null"]
}
```


#### **`descriptions[]`**
A set (list) of one or more brief descriptions. (***encouraged***)

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

#### **`parthood[]`**
A set (list) of one or more *has-parent* relations in an administrative hierarchy (***optional***). In this example, Abingdon is attested as having been an administrative part of two counties over time.

```
"parthood": [
  { "parent": "mygaz:places/p_9876",
    "parentLabel": "Berkshire (UK)",
    "when": {"timespans":[{"start":"1600","end":"1974"}]}
  },
  { "parent": "mygaz:places/p_3456",
    "parentLabel": "Oxfordshire (UK)",
    "when": {"timespans":[{"start":"1974"}]}
  }
]
```

#### **`related[]`**
A set (list) of one or more web-accessible resources related to the place in one or more of the following ways (valid values are URIs). Highly (***encouraged***); this is the central means of linking places and gazetteer datasets. Reconciliation service APIs of Pelagios and World-Historical Gazetteer will facilitate identifying closeMatch and exactMatch relations with place name authorities.

```
"relations": [
  {"exactMatch": "" },
  {"closeMatch": "" },
  {"primaryTopicOf": "" },
  {"subjectOf": "" },
  {"seeAlso": "" },
]
```

#### **`depictions[]`**
A set (list) of one or more images of some part or aspect of the place. (***optional***)

```
"depictions": [
  {
    "@id": "https://commons.wikimedia.org/wiki/File:ThamesAtAbingdon.jpg",
    "title": "The River Thames at Abingdon, Oxfordshire",
    "license": "cc:by-sa/3.0/"
  }
]
```
