## Linked Places TSV v0.2: sample place file data

Row columns are tab-delimited; multiple values within a field are semicolon-delimited

### HGIS de las Indias
```
id	title	title_source	type	aat_type	ccodes	lon	lat	geo_source	matches
1000001	Mexico	GERHARD, Peter: A Guide to the Historical Geography of New Spain (Cambridge 1972)	Ciudad	300008347	MX	-99.13313445	19.43378643	GERHARD, Peter: A Guide to the Historical Geography of New Spain (Cambridge 1972)	http://vocab.getty.edu/page/tgn/7007227
1000002	Guadalajara	GERHARD, Peter: The North Frontier of New Spain (Princeton 1982)	Ciudad	300008347	MX	-103.346998	20.676143	GERHARD, Peter: The North Frontier of New Spain (Princeton 1982) http://www.geonames.org/4005539, http://www.wikidata.org/entity/Q9022;http://vocab.getty.edu/page/tgn/7007110
1000003	Chihuahua	GERHARD, Peter: The North Frontier of New Spain (Princeton 1982)	Villa	300008347	MX	-106.083947	28.654323	GERHARD, Peter: The North Frontier of New Spain (Princeton 1982)  http://vocab.getty.edu/page/tgn/1017179;http://www.wikidata.org/entity/Q61302;http://www.geonames.org/4014338
1000004	Durango	GERHARD, Peter: The North Frontier of New Spain (Princeton 1982)	Ciudad	300008347	MX	-104.66986	24.02484	GERHARD, Peter: The North Frontier of New Spain (Princeton 1982)	http://www.wikidata.org/entity/Q112813;http://www.geonames.org/4011743;http://vocab.getty.edu/page/tgn/1017362
1000005	Real de Guanajuato	GERHARD, Peter: A Guide to the Historical Geography of New Spain (Cambridge 1972)	Villa	300008347	MX	-101.25272	21.01661	GERHARD, Peter: A Guide to the Historical Geography of New Spain (Cambridge 1972)	http://vocab.getty.edu/page/tgn/1017522
```

### Local parent_id example
```
id	title	title_source	title_uri	types	aat_types	parent_name	parent_id	matches
1001	Oxfordshire	OS Map Series 123	http://somemapsource.com/os/123	County	300000771			http://www.geonames.org/2640726
3001	Abingdon	Ye Olde Gazetteer (1635)	http://archive.org/details/yeoldegazetteer	Market Town	300008375	Oxfordshire	#1001
```


### Alcedo Gazetteer
```
id	title	title_source	title_uri types	aat_types	parent_name	matches
00005	Abbots	Alcedo, A. Diccionario geográfico-histórico de las Indias Occidentales (1788)  https://archive.org/details/diccionariogeogr03alcerich  rio 300008707		
00006	Abacachis	Alcedo, A. Diccionario geográfico-histórico de las Indias Occidentales (1788)   https://archive.org/details/diccionariogeogr03alcerich	pueblo	300008375	País de las Amazonas	
00007	Abacoa	Alcedo, A. Diccionario geográfico-histórico de las Indias Occidentales (1788)  https://archive.org/details/diccionariogeogr03alcerich				
00008	Abacqua	Alcedo, A. Diccionario geográfico-histórico de las Indias Occidentales (1788) https://archive.org/details/diccionariogeogr03alcerich	pueblo	300008375	Buenos Ayres	https://www.hgis-indias.net/dokuwiki/doku.php?id=gazetteer:9000431
00009	Abacu	Alcedo, A. Diccionario geográfico-histórico de las Indias Occidentales (1788)   https://archive.org/details/diccionariogeogr03alcerich	punta	300008850		
```

### Epirus

```
id	title	title_source	title_uri	variants	types	aat_types	ccodes	lon	lat	start	end
1	Κωνσταντινούπολη (Kōnstantinoupolē)	Ali Pasha Papers	https://www.ascsa.edu.gr/research/personal-papers-and-archives/gennadius-archival-collections#AliPasha	Πόλη;Βασιλεύουoa;Constantinopoli	settlement	300008347	GR;AL;TR;MK;BG;IT;AT;RU;RO	28.94966	41.01384	1780	1820
2	Πρέβεζα (Prebeza)	Ali Pasha Papers	https://www.ascsa.edu.gr/research/personal-papers-and-archives/gennadius-archival-collections#AliPasha	Prevesa	settlement	300008347	GR;AL;TR;MK;BG;IT;AT;RU;RO	20.7505	38.95617	1780	1820
3	Άργυρόκαστρο (Argyrokastro)	Ali Pasha Papers	https://www.ascsa.edu.gr/research/personal-papers-and-archives/gennadius-archival-collections#AliPasha	Κάστρο;Άργυρούκαστρο;Ραγυρούκαστρο	province	300000774	GR;AL;TR;MK;BG;IT;AT;RU;RO	20.13889	40.07583	1780	1820
4	Γαλλίαγαλλικός (Galliagallikos)	Ali Pasha Papers	https://www.ascsa.edu.gr/research/personal-papers-and-archives/gennadius-archival-collections#AliPasha	Φράντζα;Φράνφα;Φραγκιά;Φαργκιά;Φιράντζα;Francia	settlement	300008347	GR;AL;TR;MK;BG;IT;AT;RU;RO	2	47	1780	1820
5	Λάρισα (Larisa)	Ali Pasha Papers	https://www.ascsa.edu.gr/research/personal-papers-and-archives/gennadius-archival-collections#AliPasha	Λάρσα;Λάρσι;Γενί Σερή;Γενί Σεχέρ;Γενισεχερί;Γενισεγήρ	province	300000774	GR;AL;TR;MK;BG;IT;AT;RU;RO	22.41761	39.63689	1780	1820
```
