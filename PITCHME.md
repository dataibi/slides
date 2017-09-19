# Query Semplici

+++ 

### Query su DBpedia

http://dbpedia.org/sparql
+++


	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
	PREFIX dbo: <http://dbpedia.org/ontology/>
	SELECT ?author  
	WHERE { 
		?author rdf:type dbo:Writer . 
	} LIMIT 1000 

+++

	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
	PREFIX dbo: <http://dbpedia.org/ontology/>
								
	SELECT ?author ?work  
	WHERE { 
		?author rdf:type dbo:Writer . 
		?author dbo:notableWork ?work 
	} LIMIT 1000 

+++

	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
	PREFIX dbo: <http://dbpedia.org/ontology/>
								
	SELECT ?author ?work  
	WHERE { 
		?author rdf:type dbo:Writer . 
		OPTIONAL {?author dbo:notableWork ?work}  
	} LIMIT 1000 

+++

## Query su LinkedGeoData
http://linkedgeodata.org/sparql

+++

## Query su ISTAT
http://datiopen.istat.it/sparqlIstat.php

+++

_lasciamo perdere...._

+++

## Query su Camera dei Deputati
http://dati.camera.it/sparql

+++

### Tutte le legislature

	SELECT ?leg WHERE {
		?leg a ocd:legislatura
	} 

+++

## Tutte le legislature per cui ci sono dati sulle votazioni

	SELECT ?leg WHERE {
		?votazione a ocd:votazione .
		?votazione ocd:rif_leg ?leg
	}

+++

## Tutte le legislature per cui ci sono dati sulle votazioni

	SELECT distinct ?leg WHERE {
		?votazione a ocd:votazione .
		?votazione ocd:rif_leg ?leg
	}

+++

	SELECT distinct * WHERE {
		?votazione a ocd:votazione; 
		dc:date ?data; 
		dc:title ?denominazione; 
		dc:description ?descrizione;
		ocd:votanti ?votanti;
		ocd:rif_leg <http://dati.camera.it/ocd/legislatura.rdf/repubblica_17>
	} 
	ORDER BY DESC(?data)

---

## Query Utili

Chiedere a uno SPARQL Endpoint quali RDF Graphs sono disponibili

+++

	SELECT DISTINCT ?g
	WHERE {
		GRAPH ?g { ?s ?p ?o . }
	}

---						

# Applicazioni

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
