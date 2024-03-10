# PhD-2024-Toward Obsessive-Compulsive Disorder Classification System


This repository is dedicated to the development of an Obsessive-Compulsive Disorder (OCD) Ontology and a BERT-based classification system. It aims to advance OCD research by providing structured knowledge representation and efficient classification methodologies. 

The structure of the repository is as follows: 
Ontology Folder: Contains two sub-folders. OCD Ontology Version: Holds an .owl file for the OCD Ontology.
OCD Enriched Ontology: Contains resources for enriching the OCD Ontology with WordNet, including the WordNet resources used for enrichment and a SPARQL query for integration. A second sub-folder within enriched ontology is dedicated to contextual similarity, featuring a Word2Vec model trained on OCD forum data and a CSV with top-10 terms similar to OCD concepts. 

The development process follows methodologies inspired by XOD and SABiO, emphasizing knowledge gathering, concept analysis, and logical consistency using Description Logic (DL). Reuse strategies involve integrating existing ontological components and enriching the ontology with design patterns from WordNet and contextual similarities. This approach ensures a comprehensive representation of OCD in both the ontology and the classification system.

<img src="https://github.com/areejnasser/PhD-2024/assets/58149704/62e9b99e-599d-497e-b02e-1706919ac8c2" width="300" height="300">


Our study incorporates 12 pre-existing ontologies, as demonstrated in the subsequent figure: 

<img src="/Toward%20Obsessive-Compulsive%20Disorder%20Classification%20System/Ontology/OntoOCD-version/re-using-ontologies.png" width="500" height="auto">



To enrich the ontology, we employed Lemon and FrAC models, integrating WordNet and embeddings for contextual similarity.

![Ontology-Enrichment WordNet lemon-core](/Toward%20Obsessive-Compulsive%20Disorder%20Classification%20System/Ontology/Ontology-Enrichment/WordNet/lemon-core.png)


<img src="/Toward%20Obsessive-Compulsive%20Disorder%20Classification%20System/Ontology/Ontology-Enrichment/Contextual-similarity/ontolex-frac.png" width="500">


The SPARQL qury used for integrating those resources into OCD ontology is:

```sparql
PREFIX : <http://www.semanticweb.org/ontoOCD#>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX decomp: <http://www.w3.org/ns/lemon/decomp#>
PREFIX frac: <http://www.w3.org/ns/lemon/frac#>
PREFIX lexinfo: <http://www.lexinfo.net/ontology/2.0/lexinfo#>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX spif: <http://spinrdf.org/spif#>
PREFIX wn20schema: <http://www.w3.org/2006/03/wn/wn20/schema/>
PREFIX wn20instances: <http://www.w3.org/2006/03/wn/wn20/instances/>
#SELECT *
#FROM <ocd>

#CONSTRUCT
DELETE {
?x  rdfs:label ?y
}
INSERT
{
?x
    a owl:Class ;
    a frac:Observable ;
    rdfs:label ?xlbl ;
    ?p ?o .


## labels Word

?xlblword
    a ontolex:Word ;
    a ontolex:LexicalEntry ;
    a frac:Observable ;
    ontolex:sense ?wn20 ;
    ontolex:evokes ?wn20ss ;
    frac:embedding ?embed .

?xlblmulti
    a ontolex:MultiWord ;
    a ontolex:LexicalEntry ;
    a frac:Observable ;
    decomp:constituent ?wlbl .


## components

?wlbl
    a decomp:Component ;
    a frac:Observable ;
    ontolex:sense ?wn20 ;
    ontolex:evokes ?wn20ss ;
    frac:embedding ?embed .


## senses and evocations

?wn20
    a ontolex:LexicalSense ;
    a frac:Observable ;
    ontolex:isLexicalizedSenseOf ?wn20ss ;
    ontolex:reference <http://www.w3.org/2006/03/wn/wn20/> . 

?wn20ss
    a ontolex:LexicalConcept ;
    a frac:Observable ;
    skos:definition ?def ;
    lexinfo:hypernym ?wn20sshypo ;
    ontolex:lexicalizedSense ?wn20 .


## embeddings

## similarities and attestations

}
WHERE {
	?x a owl:Class .
	OPTIONAL {
		?x rdfs:label ?y
	}
	FILTER(STRSTARTS(STR(?x),STR(:)))

	?x ?p ?o .
	FILTER(?p != rdfs:label)

	BIND(LCASE(STRAFTER(STR(COALESCE(?y,?x)),STR(:))) AS ?xbas)
	BIND(IRI(CONCAT(STR(:),?xbas)) AS ?xlbl)

	?word spif:split(?xbas '_') .
	BIND(STRLANG(?word,'en-US') AS ?enword)

	BIND(IF(CONTAINS(?xbas, '_'), IRI(CONCAT(STR(:),?word)), ?nil) AS ?wlbl)
	BIND(IF(CONTAINS(?xbas, '_'), ?word, ?xbas) AS ?wbas)
	BIND(IF(CONTAINS(?xbas, '_'), ?nil, ?xlbl) AS ?xlblword)
	BIND(IF(CONTAINS(?xbas, '_'), ?xlbl, ?nil) AS ?xlblmulti)

	BIND(IRI(CONCAT(STR(:),?wbas,'_embed')) AS ?embed)
	BIND(IRI(CONCAT(STR(:),?wbas,'_attst')) AS ?attst)

	OPTIONAL {
		?wn20 rdfs:label ?enword .
		?wn20ss wn20schema:containsWordSense ?wn20 ;
			wn20schema:gloss ?def .
		OPTIONAL {
			?wn20ss wn20schema:hyponymOf ?wn20sshypo .
		}
	}

	BIND(CONCAT(REPLACE(?xbas, '_', ' ')) AS ?quot)
}) in read me file as code





