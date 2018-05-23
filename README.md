## Linked Places Interconnection Format

*Draft, for comment*

The Linked Places Interconnection format (LPIF) supercedes the [Pelagios Gazetteer Interconnection Format (PGIF)](https://github.com/pelagios/pelagios-cookbook/wiki/Pelagios-Gazetteer-Interconnection-Format), as a template for contributions to both [Pelagios](http://http://pelagios) and [World-Historical Gazeetteer](http://whgazetteer.org). While these projects have distinctive features, both are building software tools and services to allow everyone to:

- search across different gazetteers
- find enough information in order to identify and disambiguate places
- annotate data with stable URIs to the most appropriate gazetteer

Our goal is not to define *The One* unified data model to represent gazetteers. Historical research projects producing gazetteer data have distinctive data models reflecting their source data and project-specific requirements. What we aim for is a uniform way to build links between different gazetteers, along with just enough additional metadata to support the three requirements above.

Both LPIF and the earlier PGIF are valid RDF, the cornerstone format for [Linked Open Data]() and the Semantic Web. LPIF differs from PGIF in these ways:

- it uses [JSON-LD syntax](https://json-ld.org/spec/latest/json-ld/), as opposed to Turtle or RDF/XML, and is therefore also valid JSON
- it is valid GeoJSON, therefore readily rendered in many web mapping applications; in fact, it is an implementation of [GeoJSON-T](https://github.com/kgeographer/geojson-t), an experimental extension to GeoJSON that standardizes the representation of temporal attributes
- it provides for optional temporal scoping of place names, geometry (location/extent), and *part-of* relations

### The LPIF model of Place
Contributions take the form of a GeoJSON FeatureCollection ([spec](https://tools.ietf.org/html/rfc7946)) containing one or more GeoJSON Feature object. In order to index metadata about place records from multiple gazetteers, LPIF accomodates the following object elements. Each is described more fully in sections below. 

- **`@context`** (required): Property labels are aliases for terms formally defined in several linked ontologies; mappings for a FeatureCollection are defined in [this context document]([http://linkedpasts.org/assets/lpif-context.jsonld)
- **`@id`** (required): A unique and permanent URI pointing to the contributor's published record of the place
- **properties** (required):
  - **title** (required): A label for the record; usually a 'preferred' name from among the names associated with a place
  - **ccode** (encouraged): A two-letter code ([ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)) for the modern containing or principal overlapping country; used to aid disambiguation
- **when** (optional): Timespans and/or named periods relevant for the place; can be used in multiple locations within a Feature
- **namings** (required) One or more toponym, with optional temporal scope
- **placetypes** (required): One or more place type
- **geometry** (required): A GeoJSON GeometryCollection, having one or elements, optionally temporal scoped; can have a single element with empty coordinates when location is unknown
- **descriptions** (encouraged): A brief textual description
- **parthood** (optional): One or more assertions of a place's position in an administrative hierachy, with optional temporal scope
- **related** (encouraged): URI(s) to one or more related resources 
- **depictions** (optional): URI(s) to one or more images

### LPIF Feature elements

#### **`@context`**

In JSON-LD, labels for object elements are aliases for terms formally defined in several linked ontologies. For LPIF, those mappings are defined in [this context document](http://linkedpasts.org/assets/lpif-context.jsonld). 

e.g. `"@context": "http://linkedpasts.org/assets/lpif-context.jsonld"`

#### **`@id`**

A unique and permanent URI pointing to the contributor's published record of the place. Because not every project can readily stand up such resource landing pages, a "csv-to-published LPIF" tool will be developed soon.

e.g. `"@id": "mygaz:places/p_12345"`

#### **`properties{}`**

The **properties** element is required by GeoJSON. LPIF requires **title** within the properties element, and encourages **ccode**. Properties are typically displayed in popup windows upon clicking markers in web maps.

e.g. ```"properties":{ "title": "Abingdon (UK)", "ccode": "GB"}```

#### **`title`**

A label for the record; usually a 'preferred' name from among the names associated with a place. The **title** is necessary for order records in some list displays; place records always include all available attested name variants and spellings.

#### **`ccode`** 

A two-letter code ([ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)) indicating a modern country that contains or overlaps the place. Invaluable for disambiguation against modern authority listings.

#### **`when{}`**

A **when** element can be used to temporally scope a) an entire Feature; b) a **naming**; c) a **geometry** (representative point or extent); or d) a **parthood**, i.e. *part-of* relation with another place.

A **when** element can consist of one or more **timespan** and/or one or more named **period**, referenced as a [Perio.do]() URI.

Valid values for "in," "earliest," and "latest" are ISO-8601 expressions as described by the OWL-Time ontology.

Valid values for "duration" are an integer followed by one of Y, M, W, or D to indicate years, months, weeks, or days.

The following annotated example (##) indicates possible options:

```
"when": {
  "timespans": [
		{  
		  "start": { "in": "yyyy-mm" },
		  "end": {	## if omitted, typically interpreted as today()
	      	"earliest": "-yyyy",
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
  "duration": "10Y",	## optional
  "follows": "mygaz:places/p_9876"	## optional URI to indicate sequence
}

```

#### **`namings[]`**

A set (list) of one or more names elements, optionally temporally scoped. For example:
 
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
A set (list) of one or more place types


#### **`geometry`**
A GeoJSON GeometryCollection, with one or more geometry elements, optionally temporally scoped. NB: if a geometry type is anything but a "Point" having a single coordinate pair, the dataset *will not validate as JSON-LD*. This will not prevent its indexing in either Pelagios or World-Historical Gazetteer, the APIs for which will both offer valid RDF serializations in any event.

In the event there is no "coordinates" element, a WKT representation of geometry ("geo_wkt") will be used to place the Feature on maps. NB: in the event "coordinates" is absent, the entire dataset *will not validate as GeoJSON*. It will however index successfully in Pelagios and World-Historical Gazetteer.

```
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
A set (list) of one or more brief descriptions, e.g.
 
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
A set (list) of one or more *has-parent* relations in an administrative hierarchy, e.g.

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
A set (list) of one or more web-accessible resources related to the place in one or more of the following ways (valid values are URIs):

```
"relations": [
	{"exact_match": "" },
	{"close_match": "" },
	{"primary_topic_of": "" },
	{"subject_of": "" },
	{"see_also": "" },
]
```

#### **`depictions[]`**
A set (list) of one or more images of some part or aspect of the place, e.g.

```
"depictions": [
	{
	  "@id": "https://commons.wikimedia.org/wiki/File:ThamesAtAbingdon.jpg",
	  "title": "The River Thames at Abingdon, Oxfordshire",
	  "license": "cc:by-sa/3.0/"
	}
]
```
