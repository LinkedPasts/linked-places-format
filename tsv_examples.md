## Linked Places TSV: sample place file data

Rows are tab-delimited; multiple values in a field comma-delimited

### HGIS de las Indias (lugares)
```
id	title	name_src	type	aat_type	ccodes	lon	lat	geom_src	close_matches
1000001	Mexico	gerhardNE	Ciudad	inhabited place	MX	-99.13313445	19.43378643	gerhardNE	http://vocab.getty.edu/page/tgn/7007227
1000002	Guadalajara	gerhard-north	Ciudad	inhabited place	MX	-103.346998	20.676143	gerhard-north http://www.geonames.org/4005539, http://www.wikidata.org/entity/Q9022, http://vocab.getty.edu/page/tgn/7007110
1000003	Chihuahua	gerhard-north	Villa	inhabited place	MX	-106.083947	28.654323	gerhard-north	http://vocab.getty.edu/page/tgn/1017179, http://www.wikidata.org/entity/Q61302, http://www.geonames.org/4014338
1000004	Durango	gerhard-north	Ciudad	inhabited place	MX	-104.66986	24.02484	gerhard-north	http://www.wikidata.org/entity/Q112813, http://www.geonames.org/4011743, http://vocab.getty.edu/page/tgn/1017362
1000005	Real de Guanajuato	gerhardNE	Villa	inhabited place	MX	-101.25272	21.01661	gerhardNE	http://vocab.getty.edu/page/tgn/1017522
```

### Alcedo Gazetteer
```
id	title	name_src	type	aat_types	parent	close_match
00005	Abbots	alcedo	rio	300008707		
00006	Abacachis	alcedo	pueblo	300008375	País de las Amazonas	
00007	Abacoa	alcedo				
00008	Abacqua	alcedo	pueblo	300008375	Buenos Ayres	https://www.hgis-indias.net/dokuwiki/doku.php?id=gazetteer:9000431
00009	Abacu	alcedo	punta	300008850		
```

### Epirus

```
id	title	name_src	variants	type	aat_type	ccodes	lon	lat	min	max
1	Κωνσταντινούπολη (Kōnstantinoupolē)	ali-pasha	Πόλη, Βασιλεύουoa, Constantinopoli	settlement	inhabited places	GR, AL, TR, MK, BG, IT, AT, RU, RO	28.94966	41.01384	1780	1820
2	Πρέβεζα (Prebeza)	ali-pasha	Prevesa	settlement	inhabited places	GR, AL, TR, MK, BG, IT, AT, RU, RO	20.7505	38.95617	1780	1820
3	Άργυρόκαστρο (Argyrokastro)	ali-pasha	Κάστρο, Άργυρούκαστρο, Ραγυρούκαστρο	province	provinces	GR, AL, TR, MK, BG, IT, AT, RU, RO	20.13889	40.07583	1780	1820
4	Γαλλίαγαλλικός (Galliagallikos)	ali-pasha	Φράντζα, Φράνφα, Φραγκιά, Φαργκιά, Φιράντζα, Francia	settlement	inhabited places	GR, AL, TR, MK, BG, IT, AT, RU, RO	2	47	1780	1820
5	Λάρισα (Larisa)	ali-pasha	Λάρσα, Λάρσι, Γενί Σερή, Γενί Σεχέρ, Γενισεχερί, Γενισεγήρ	province	provinces	GR, AL, TR, MK, BG, IT, AT, RU, RO	22.41761	39.63689	1780	1820
```

## Linked Places TSV: sample sources file data

A selection of source records related to the above place data files. Typically, each place data file is accompanied by one source data file relevant for its records.

```
id	title	citation
alcedo	Diccionario geográfico-histórico de las Indias Occidentales o América	ALCEDO, Antonio de: Diccionario geográfico-histórico de las Indias Occidentales o América, 5 vols. (Madrid 1786-1789).
GerhardNE	A Guide to the Historical Geography of New Spain	GERHARD, Peter: A Guide to the Historical Geography of New Spain (Cambridge 1972).
ali-pasha	Ali Pasha Papers	https://www.ascsa.edu.gr/index.php/news/newsDetails/the-ali-pasha-papers-published/
```