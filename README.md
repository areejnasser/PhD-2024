# PhD-2024-Guided by Dr.Alia Abdelmoty- Toward Obsessive-Compulsive Disorder Classification System 


This repository is dedicated to developing an Obsessive-Compulsive Disorder (OCD) Ontology and a BERT-based classification system. It aims to advance OCD research by providing structured knowledge representation and efficient classification methodologies. 

The repository is organized into specific directories and subdirectories, each with a distinct purpose:

Ontology Folder: This directory contains multiple subfolders:
OCD Ontology Version: This subfolder stores the .owl file of the OCD Ontology.
Data_for_ontology_development: consists of formal resources used for developying OCD ontology.
Ontology_Development: consists of the process of knowledge analysis, which includes identifying key concepts and their relationships, and creating RDF statements to formally represent these concepts. Additionally, it entails providing natural language definitions of OCD-related concepts and employing description logic to systematically outline the conceptualization. 
OCD Ontology-Enrichment: Inside, there are tools for augmenting the ontology, like WordNet resources for semantic enhancement and SPARQL queries for integration. Another subfolder contains elements related to contextual similarity, including a Word2Vec model trained on OCD forum data and a CSV file listing terms semantically close to OCD-related concepts. 



Evaluation Folder: Contains SPARQL queries, OOPs, survey of ontology evaluation, and subfolder for evaluation the ontology labelling by domain Experts. This 'Expert' subfolder includes a CSV file featuring posts annotated by the ontology and subsequently evaluated by domain experts for accuracy and relevance.  The survey folder consists of the survey, the results and visual analysis of the results. 

The repository's "Annotation" directory is a compilation of resources and scripts used for the data annotation process, with the following contents:

Annotation 1 and Annotation 2 Subfolders: These contain the results of the annotation efforts.
Annotation 1: Includes a collection of files detailing the annotations of an OCD forum using the base OCD ontology.
The .col file holds the specific annotations.
The .ttl file provides these annotations in RDF (Resource Description Framework) format.
The .owl file contains the populated ontology with the new annotations integrated.

To describe this task the prerequisites:
Python environment with necessary libraries installed (e.g., pandas, requests, tqdm).
Access to the Biportal API with a valid API_KEY.
Dataset of OCD forum posts.

Steps to Run the Python Script

Dataset Preparation:

Ensure the OCD forum posts dataset is available in the specified directory: ontology-based classification/PhD-2024/Toward Obsessive-Compulsive Disorder Classification System/OCD2(updated).json.
If necessary, adjust the directory path to match the location of your dataset.
API Key Configuration:

Obtain an API_KEY to access the Biportal API.
Update the Python script (NCBO-API.ipynb) with your API_KEY for accessing the Annotator Plus service.

Open and run the Python script (NCBO-API.ipynb).
The script processes the OCD forum posts and sends them to the Annotator Plus service for annotation.

Processing Annotation Results:
Convert the CSV file of the annotation results into .col and .ttl formats.
The .col file format is used to store the raw annotations.
The .ttl file format (Turtle) is used to represent the annotations where each post is typed as a class.

Now, the posts are represented with their associated annotations as ttl. To integrate ttl into OCD ontology, we upload ttl and OCD ontology owl file into GraphDP. We run SPARQL query illustrated in (population_post_into_ocd_ontology). We run the reasoning and extract posts with obsession and compulsion inferences. 

Annotation 2: Similar to Annotation 1 but uses the enriched OCD ontology.
The .txt file captures the annotations.
The .ttl file represents these annotations in RDF format.
The .owl file showcases the enriched ontology now populated with the annotations.



<img src="https://github.com/areejnasser/PhD-2024/assets/58149704/62e9b99e-599d-497e-b02e-1706919ac8c2" width="300" height="300">


Our study incorporates 12 pre-existing ontologies.




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


\end{lstlisting}




