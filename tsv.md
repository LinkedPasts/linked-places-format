The following is a TSV alternative to [the JSON-LD based Linked Places format](README.md) for contributions to World-Historical Gazetteer, and consists of two files: one for places and one for sources. [Samples](tsv_examples.md).

Both place and source files must be tab-delimited unicode text (utf-8). The following fields will be parsed and converted to Linked Places format automatically upon upload.


**NB** A record consisting of only the required __id__, __title__ and __name_src__ will almost always be difficult to reconcile (i.e. match) with existing gazetteer data resources.


## Place data files

### required
#### id         
	
Contributor's internal identifier. This must stay consistent throughout accessioning workflow, including subsequent updates

#### title

A single "preferred" place name, which acts as title for the record

#### name_src

Contributor's identifier for the source of the title toponym; foreign key to a record in separate contributed "sources" file

### encouraged

#### exact_matches

One or more URIs for published linked data records for places; comma-delimited

#### close_matches

One or more URIs for published linked data records for places; comma-delimited

#### ccodes

One or more [ISO Alpha-2 two-letter codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for **modern** countries that overlap or cover the place in question; comma-delimited

#### variants

One or more name variants; comma-delimited

#### types

One or more place type terms (contributor's term, e.g. pueblo); comma-delimited


#### aat_types		

One or more URI identifiers from a web-published vocabulary corresponding to the above types; ideally these would come from [this list of ~160 types]() drawn from the Getty AAT vocabulary; comma-delimited.


### optional

#### parent

A toponym for a "containing" place, e.g. province, historical polity

#### lon					

Longitude, decimal degrees

#### lat

Latitude, decimal degrees

#### geom_src

Contributor's identifier for the source of the geometry; e.g. http://www.geonames.org/2950159

#### min

Earliest relevant date (YYYY-MM-DD); omit month and/or year as req. BCE years must expressed as negative integer, e.g. -0320 for 320 BCE

#### max

Latest relevant date, as above

## Source data files

### required

#### id
An alpha-numeric identifier (internal) corresponding to 'name\_src' or 'geom\_src' in the accompanying places file.

#### title
Text string
	
#### citation
Any citation format, or URI
