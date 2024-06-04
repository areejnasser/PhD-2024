# PhD-2024-Guided by Dr.Alia Abdelmoty- Toward Obsessive-Compulsive Disorder Classification System 


This repository is dedicated to developing an Obsessive-Compulsive Disorder (OCD) Ontology and a BERT-based classification system. It aims to advance OCD research by providing structured knowledge representation and efficient classification methodologies. 

The repository is organized into specific directories and subdirectories, each with a distinct purpose:

Ontology Folder: This directory comprises several subdirectories:

Initial_Task_Data_Collection: This subfolder is intended for storing the initial datasets and related files collected for the project. It likely contains raw data and preliminary analyses pertinent to the early stages of the project.

Second_Task_Conceptualisation: This subfolder is designated for files related to the conceptual phase. 

OCD Ontology Versions: This subfolder contains the .owl files for the OCD Ontology across different versions:

1. The initial version of the OCD ontology without importing or reusing other ontologies (ocd_version1.owl).
2. The ontology enriched with WordNet (ocd_version2_enriched_WordNet.owl.zip).
3. The ontology further enriched with WordEmbedding (ocd_version3_enriched_WordNet_Embedding.owl.zip).
4. The latest version of the OCD ontology, which includes imports from reused ontologies.



OCD Ontology-Enrichment: Inside, there are tools for augmenting the ontology, like WordNet resources for semantic enhancement and SPARQL queries for integration. Another subfolder contains elements related to contextual similarity, including a Word2Vec model trained on OCD forum data and a CSV file listing terms semantically close to OCD-related concepts. 
To describe the task of enrichment in detail:

Using WordNet:
We downloaded three files from WordNet, namely, synset, synonyms, and hypernym, using the following links which provide WordNet in RDF format:
WordNet Documentation: https://www.w3.org/TR/wordnet-rdf/
WordNet Download: https://www.w3.org/2006/03/wn/wn20/download/
We used a SPARQL query and the Lemon design pattern to integrate WordNet into the OCD ontology. The query is presented in WordNet folder.

For Contextual Similarity:
We trained a Word2Vec model using OCD forum data (presented in the 'Embedding-Similarity-Task' folder).
We then asked for the top 10 similar terms to OCD-related terms.
We used the FrAc design pattern and a SPARQL query to integrate these terms into the OCD ontology.

Evaluation Folder: Contains SPARQL queries, OOPs, survey of ontology evaluation, and subfolder for evaluation the ontology labelling by domain Experts. This 'Expert' subfolder includes a CSV file featuring posts annotated by the ontology and subsequently evaluated by domain experts for accuracy and relevance.  The survey folder consists of the survey, the results and visual analysis of the results. 

The repository's "Ontology_Annotation_&_Populations" directory is a compilation of resources and scripts used for the data annotation process, with the following contents:

Annotation 1 and Annotation 2 Subfolders: These contain the results of the annotation efforts.
Annotation 1: Includes a collection of files detailing the annotations of an OCD forum using the base OCD ontology.
The .col file holds the specific annotations.
The .ttl file provides these annotations in RDF (Resource Description Framework) format.
The .owl file contains the populated ontology with the new annotations integrated.

To describe this task, the prerequisites:
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
To convert .col to .ttl, run the following command 
tail -n+2 Final_Anno.col | nl | sed 's@^ *@{"post@; s@\t@":@; s@$@}@' | tr "'" '"' | jq -r '.[] |= map(gsub(" ";"_")) | (":"+(. | keys[]) + " a owl:Thing , :" + (.[][]) + " .")' >>Final_Anno11.ttl

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


