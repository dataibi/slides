# Query Semplici

+++ 

## Query su DBpedia

http://dbpedia.org/sparql

+++

```sql

SELECT ?author  
WHERE { 
	?author <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/Writer> . 
} LIMIT 1000 
```

+++

```sql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dbo: <http://dbpedia.org/ontology/>

SELECT ?author  
WHERE { 
	?author rdf:type dbo:Writer . 
} LIMIT 1000 
```

+++

```sql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dbo: <http://dbpedia.org/ontology/>
								
SELECT ?author ?work  
WHERE { 
	?author rdf:type dbo:Writer . 
	?author dbo:notableWork ?work 
} LIMIT 1000 
```

+++

```sql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dbo: <http://dbpedia.org/ontology/>
								
SELECT ?author ?work  
WHERE { 
	?author rdf:type dbo:Writer . 
	OPTIONAL {?author dbo:notableWork ?work}  
} LIMIT 1000 
```

+++

Esercizio: estrarre da DBpedia tutte le triple che riguardano Palermo

+++

```sql
SELECT ?predicato ?oggetto 
WHERE { 
	<http://dbpedia.org/resource/Palermo> ?predicato ?oggetto . 
} 
```

+++

Ci sono tutte?

+++

```sql
								
SELECT ?soggetto ?predicato 
WHERE { 
	?soggetto ?predicato <http://dbpedia.org/resource/Palermo> . 
} 
```

+++

## Query su LinkedGeoData
http://linkedgeodata.org/sparql

+++

```sql
Prefix lgdo: <http://linkedgeodata.org/ontology/>
Prefix geom: <http://geovocab.org/geometry#>
Prefix ogc: <http://www.opengis.net/ont/geosparql#>
Prefix owl: <http://www.w3.org/2002/07/owl#>

Select * {
  ?s
    owl:sameAs <http://dbpedia.org/resource/Palermo> ;
    geom:geometry [
      ogc:asWKT ?sg
    ] .

  ?x
    a lgdo:Hospital ;
    rdfs:label ?l ;    
    geom:geometry [
      ogc:asWKT ?xg
    ] .


    Filter(bif:st_intersects (?sg, ?xg, 10)) .
}
```

+++

```sql
Prefix lgdo:<http://linkedgeodata.org/ontology/>
Prefix geom:<http://geovocab.org/geometry#>
Prefix ogc: <http://www.opengis.net/ont/geosparql#>
SELECT * WHERE {
    ?p a lgdo:Helipad ; geom:geometry/ogc:asWKT ?pgv .
    ?h a lgdo:Hospital; geom:geometry/ogc:asWKT ?hgv . 

    Filter(bif:st_intersects(?pgv, ?hgv, 0.5))
}
LIMIT 10
```

+++

## Query su ISTAT
http://datiopen.istat.it/sparqlIstat.php

+++

_lasciamo perdere...._

+++

_aspetta un attimo..._

+++

	http://datiopen.istat.it/sparql/oracle?query=<QUERY>&format=<FORMATO>

+++

	select * where { ?s ?p ?o } limit 10
	
https://meyerweb.com/eric/tools/dencoder/
	
	select%20*%20where%20%7B%20%3Fs%20%3Fp%20%3Fo%20%7D%20limit%2010

+++

http://datiopen.istat.it/sparql/oracle?query=select%20*%20where%20%7B%20%3Fs%20%3Fp%20%3Fo%20%7D%20limit%2010&format=csv

+++

	PREFIX ORACLE_SEM_FS_NS: <http://oracle.com/semtech#timeout=600,allow_dup=t,strict_default=f> 

	PREFIX+ORACLE_SEM_FS_NS%3A+%3Chttp%3A%2F%2Foracle.com%2Fsemtech%23timeout%3D600%2Callow_dup%3Dt%2Cstrict_default%3Df%3E%20

+++

http://datiopen.istat.it/sparql/oracle?query=PREFIX+ORACLE_SEM_FS_NS%3A+%3Chttp%3A%2F%2Foracle.com%2Fsemtech%23timeout%3D600%2Callow_dup%3Dt%2Cstrict_default%3Df%3E%20select%20*%20where%20%7B%20%3Fs%20%3Fp%20%3Fo%20%7D%20limit%2010&format=csv

+++

Comuni più alti d'Italia

```sql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ter: <http://datiopen.istat.it/odi/ontologia/territorio/>
PREFIX cen: <http://datiopen.istat.it/odi/ontologia/censimento/>
PREFIX qb: <http://purl.org/linked-data/cube#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#> 

SELECT  ?Comune ?Codice_Istat_Comune ?Provincia (replace(str(?Alt),"<http://www.w3.org/2001/XMLSchema#decimal>", "") as ?Altitudine) 
WHERE { 
	?com rdf:type ter:EntitaTerritoriale .
	?com ter:haNome ?Comune . 
	?com ter:haCodIstat ?Codice_Istat_Comune . 
	?com ter:haAltitudineMaxCOM ?Alt .
	?com ter:provincia_di_COM ?prov . 
	?prov ter:haNome ?Provincia .
	FILTER (?Alt > 4000)
} ORDER BY DESC (?Alt) 
```

+++

http://datiopen.istat.it/sparql/oracle?query=PREFIX+ORACLE_SEM_FS_NS%3A+%3Chttp%3A%2F%2Foracle.com%2Fsemtech%23timeout%3D600%2Callow_dup%3Dt%2Cstrict_default%3Df%3E%20PREFIX%20rdf%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ter%3A%20%3Chttp%3A%2F%2Fdatiopen.istat.it%2Fodi%2Fontologia%2Fterritorio%2F%3E%0APREFIX%20cen%3A%20%3Chttp%3A%2F%2Fdatiopen.istat.it%2Fodi%2Fontologia%2Fcensimento%2F%3E%0APREFIX%20qb%3A%20%3Chttp%3A%2F%2Fpurl.org%2Flinked-data%2Fcube%23%3E%0APREFIX%20xsd%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2001%2FXMLSchema%23%3E%20%0ASELECT%20%20%3FComune%20%3FCodice_Istat_Comune%20%3FProvincia%20(replace(str(%3FAlt)%2C%22%3Chttp%3A%2F%2Fwww.w3.org%2F2001%2FXMLSchema%23decimal%3E%22%2C%20%22%22)%20as%20%3FAltitudine)%20%0AWHERE%20%7B%20%0A%3Fcom%20rdf%3Atype%20ter%3AEntitaTerritoriale%20.%0A%3Fcom%20ter%3AhaNome%20%3FComune%20.%20%0A%3Fcom%20ter%3AhaCodIstat%20%3FCodice_Istat_Comune%20.%20%0A%3Fcom%20ter%3AhaAltitudineMaxCOM%20%3FAlt%20.%0A%3Fcom%20ter%3Aprovincia_di_COM%20%3Fprov%20.%20%0A%3Fprov%20ter%3AhaNome%20%3FProvincia%20.%0AFILTER%20(%3FAlt%20%3E%204000)%0A%7D%20ORDER%20BY%20DESC%20(%3FAlt)+limit+10&format=csv

+++

Elenco località della provincia di Palermo con numero di stranieri di origine asiatica

```sql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ter: <http://datiopen.istat.it/odi/ontologia/territorio/>
PREFIX cen: <http://datiopen.istat.it/odi/ontologia/censimento/>
PREFIX qb: <http://purl.org/linked-data/cube#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#> 

SELECT  ?Localita (replace(str(?Stranieri),"<http://www.w3.org/2001/XMLSchema#decimal>", "") as ?Stranieri_origine_Asiatica) 
WHERE {
	?loc rdf:type ter:LOC . 
	?loc ter:haNome ?Localita .
	?loc ter:provincia_di_LOC ?prov .
	?loc ter:haIndicatoreCensimento ?o .   
	?o cen:provieneDa cen:Asia .
	?o cen:haStranieriApolidiResidentiItalia ?Stranieri.
	FILTER(?prov = <http://datiopen.istat.it/odi/risorsa/territorio/province/Palermo>) 
} ORDER BY DESC (?Stranieri)
```

+++

http://datiopen.istat.it/sparql/oracle?query=PREFIX+ORACLE_SEM_FS_NS%3A+%3Chttp%3A%2F%2Foracle.com%2Fsemtech%23timeout%3D600%2Callow_dup%3Dt%2Cstrict_default%3Df%3E%20PREFIX%20rdf%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ter%3A%20%3Chttp%3A%2F%2Fdatiopen.istat.it%2Fodi%2Fontologia%2Fterritorio%2F%3E%0APREFIX%20cen%3A%20%3Chttp%3A%2F%2Fdatiopen.istat.it%2Fodi%2Fontologia%2Fcensimento%2F%3E%0APREFIX%20qb%3A%20%3Chttp%3A%2F%2Fpurl.org%2Flinked-data%2Fcube%23%3E%0APREFIX%20xsd%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2001%2FXMLSchema%23%3E%20%0ASELECT%20%20%3FLocalita%20(replace(str(%3FStranieri)%2C%22%3Chttp%3A%2F%2Fwww.w3.org%2F2001%2FXMLSchema%23decimal%3E%22%2C%20%22%22)%20as%20%3FStranieri_origine_Asiatica)%20%0AWHERE%20%7B%0A%3Floc%20rdf%3Atype%20ter%3ALOC%20.%20%0A%3Floc%20ter%3AhaNome%20%3FLocalita%20.%0A%3Floc%20ter%3Aprovincia_di_LOC%20%3Fprov%20.%0A%3Floc%20ter%3AhaIndicatoreCensimento%20%3Fo%20.%20%20%20%0A%3Fo%20cen%3AprovieneDa%20cen%3AAsia%20.%0A%3Fo%20cen%3AhaStranieriApolidiResidentiItalia%20%3FStranieri.%0AFILTER(%3Fprov%20%3D%20%3Chttp%3A%2F%2Fdatiopen.istat.it%2Fodi%2Frisorsa%2Fterritorio%2Fprovince%2FPalermo%3E)%20%0A%7D%20ORDER%20BY%20DESC%20(%3FStranieri)%0A&format=csv

+++

## Query su Camera dei Deputati
http://dati.camera.it/sparql

+++

### Tutte le legislature
```sql
	SELECT ?leg WHERE {
		?leg a ocd:legislatura
	} 
```

+++

### Tutte le legislature per cui ci sono dati sulle votazioni
```sql
	SELECT ?leg WHERE {
		?votazione a ocd:votazione .
		?votazione ocd:rif_leg ?leg
	}
```

+++

### Tutte le legislature per cui ci sono dati sulle votazioni
```sql
	SELECT distinct ?leg WHERE {
		?votazione a ocd:votazione .
		?votazione ocd:rif_leg ?leg
	}
```

+++

```sql
	SELECT distinct * WHERE {
		?votazione a ocd:votazione; 
		dc:date ?data; 
		dc:title ?denominazione; 
		dc:description ?descrizione;
		ocd:votanti ?votanti;
		ocd:rif_leg <http://dati.camera.it/ocd/legislatura.rdf/repubblica_17>
	} 
	ORDER BY DESC(?data)
```

---

# Query Utili

+++

Chiedere a uno SPARQL Endpoint quali RDF Graphs sono disponibili

+++

```sql
	SELECT DISTINCT ?g
	WHERE {
		GRAPH ?g { ?s ?p ?o . }
	}
```

---						

# Usare i risultati

+++

## Google spreadsheet	

+++				
					=IMPORTDATA("http://factforge.net/repositories/ff-news?query=%23+F4%3A+Top-lev
el+industries+by+number+of+companies%0A%23+-+benefits+from+the+mapping+a
nd+consolidation+of+industry+classifications%0A%23+++and+predicates+in+DBPedi
a+done+in+the+FactForge%0A%23+-+benefits+from+reasoning+-+transitive+and+s
ymmetric+properties+across%0A%23+++the+industry+classification+taxonomy+of+
FactForge%0A%0APREFIX+dbo%3A+%3Chttp%3A%2F%2Fdbpedia.org%2Fontology%
2F%3E%0APREFIX+ff-map%3A+%3Chttp%3A%2F%2Ffactforge.net%2Fff2016-mappin
g%2F%3E%0A%0ASELECT+DISTINCT+%3Ftop_industry+(COUNT(*)+AS+%3Fcount)
%0A%7B%0A+++%3Fcompany+dbo%3Aindustry+%3Findustry+.%0A+++%3Findust
ry+%5Eff-map%3AindustryVariant+%2F+ff-map%3AindustryCenter+%3Ftop_industry
+.%0A%7D%0AGROUP+BY+%3Ftop_industry+ORDER+BY+DESC(%3Fcount)+")

+++

## cURL

+++
						
	curl -H Accept:text/tab-separated-values "http://factforge.net/repositories/ff-news?infer=true&sameAs=false&query=PREFIX+dbo%3A+%3Chttp%3A%2F%2Fdbpedia.org%2Fontology%2F%3E%0ASELECT+%3Findustry+%7B%24company+dbo%3Aindustry+%3Findustry%7D%0A&$company=<http://dbpedia.org/resource/Google>"

--- 

### e tanto altro ancora...
					
+++

Spraklis
					
http://www.irisa.fr/LIS/ferre/sparklis/
						
http://www.irisa.fr/LIS/ferre/sparklis/osparklis.html?endpoint=http%3A//dbpedia.org/sparql&regexp_hidden_URIs=%5E%28http%3A//www.w3.org/2002/07/owl%23%7Chttp%3A//www.openlinksw.com/%7CnodeID%3A//%7C%23%7Chttp%3A//www.wikidata.org/%29

+++

NLquery
http://nlquery.ayoungprogrammer.com/
						
Hokudai
http://wnews.ist.hokudai.ac.jp/wc3?search=Songs+written+by+Paul+McCartney
						
Quepy
http://quepy.machinalis.com/
