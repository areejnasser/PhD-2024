# PhD-2024
Toward Obsessive-Compulsive Disorder Classification System

This repository contains the OCD Ontology and a classification system based on BERT. The structure is as follows: Ontology Folder: Contains two sub-folders.
Ontology Version: Holds the .owl file for the OCD Ontology.
Enriched Ontology: Contains resources for enriching the OCD Ontology with WordNet, including the WordNet resources and a SPARQL query for integration. A second sub-folder within enriched ontology is dedicated to contextual similarity, featuring a Word2Vec model trained on OCD forum data and a CSV with top-10 terms similar to OCD concepts.

The development process follows methodologies inspired by XOD and SABiO, emphasizing knowledge gathering, concept analysis, and logical consistency using Description Logic (DL). Reuse strategies involve integrating existing ontological components and enriching the ontology with design patterns from WordNet and contextual similarities. This approach ensures a comprehensive representation of OCD in both the ontology and the classification system.

![Toward Obsessive-Compulsive Disorder Classification System](https://github.com/areejnasser/PhD-2024/assets/58149704/62e9b99e-599d-497e-b02e-1706919ac8c2)

The Pattern Design used for enriching the ontology with WordNet and Contextual similarity derived from Embedding model are Lemon and FrAC models. 
