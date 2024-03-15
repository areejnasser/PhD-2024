# PhD-2024-Toward Obsessive-Compulsive Disorder Classification System


This repository is dedicated to the development of an Obsessive-Compulsive Disorder (OCD) Ontology and a BERT-based classification system. It aims to advance OCD research by providing structured knowledge representation and efficient classification methodologies. 

The structure of the repository is as follows: 
Ontology Folder:  Contains sub-folders. OCD Ontology Version: Holds an .owl file for the OCD Ontology.
OCD Enriched Ontology: Contains resources for enriching the OCD Ontology with WordNet, including the WordNet resources used for enrichment and a SPARQL query for integration. A second sub-folder within enriched ontology is dedicated to contextual similarity, featuring a Word2Vec model trained on OCD forum data and a CSV with top-10 terms similar to OCD concepts. 
Ontology folder provide the OCD competency Questions(CQs) used to developed the ontology and Natural Language Defination of OCD Concepts and Description Logic
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

\begin{lstlisting}[language=SPARQL]

SELECT ?subject ?predicate ?object
WHERE {
  ?subject ?predicate ?object .
  FILTER (?subject = <example:YourSubject>)
}
\end{lstlisting}


The PREFIX is as follows:

\begin{lstlisting}[language=SPARQL]
PREFIX java: <http://evolizer.org/ontologies/seon/2009/06/java.owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX obo: <http://purl.obolibrary.org/obo#>
PREFIX ocd: <http://www.semanticweb.org/OCD#>
\end{lstlisting}

The subsequent sections detail SPARQL queries formulated to address the CQs outlined in Table \ref{tab:CQ-knowledge-pattern} in Chapter \ref{construction}. Accompanying each query, an explanation is provided to illustrate how each query aims to retrieve information pertinent to the posed questions, leveraging the structured knowledge within the OCD ontology. 

CQ1. What causes OCD? 


\begin{lstlisting}[language=SPARQL]
{
SELECT  *
WHERE {
?subject ocd:hasRiskFactor ?riskFactor.
?subject rdf:type <http://purl.obolibrary.org/obo/DOID_10933>
}
\end{lstlisting}

The SPARQL query provided for CQ1, "What causes OCD?", is structured to retrieve information related to the risk factors associated with OCD.  The query should return all instances involved in the matching patterns specified within the WHERE clause. In this context, it means that for each matching pattern found, the query will return the subjects (instances of OCD) along with their associated risk factors. The core of the query, defining the pattern that must be matched in the ontology data for results to be returned.
?subject ocd:hasRiskFactor ?riskFactor.: This pattern seeks instances (?subject) that are linked to risk factors (?riskFactor) through the ocd:hasRiskFactor property. This property is presumed to connect OCD instances to various risk factors defined within the ontology. The pattern restricts the subjects to those that are of type OCD. 

CQ2. What is the difference between obsessions and compulsions?

\begin{lstlisting}[language=SPARQL]
SELECT ?instance ?emotion ?impairment ?duration
WHERE {
    {
        ?instance rdf:type ocd:Obsession.
        FILTER (
            ?instance rdf:type ocd:IntrusiveThought ||
            ?instance rdf:type ocd:IntrusiveImage ||
            ?instance rdf:type ocd:IntrusiveUrge
        )
    } UNION {
        ?instance rdf:type ocd:Thought.
        ?instance ocd:hasAssociatedEmotion ?emotion.
        ?instance ocd:hasAssociatedImpairment ?impairment.
        ?instance ocd:hasAssociatedDuration ?duration.
        FILTER EXISTS { ?emotion rdf:type ocd:NegativeEmotion. }
        FILTER EXISTS { ?impairment rdf:type ocd:Impairment. }
        FILTER EXISTS { ?duration rdf:type ocd:SeverityDurataionLevel. }
    }
}
\end{lstlisting}


\begin{lstlisting}[language=SPARQL]
SELECT ?instance ?emotion ?impairment ?duration
WHERE {
    {
        ?instance rdf:type ocd:Compulsion.
        FILTER (
            ?instance rdf:type ocd:Behavior ||
            ?instance rdf:type ocd:Activity
        )
    } UNION {
        ?instance rdf:type ocd:Activity.
        ?instance ocd:hasAssociatedEmotion ?emotion.
        ?instance ocd:hasAssociatedImpairment ?impairment.
        ?instance ocd:hasAssociatedDuration ?duration.
        FILTER EXISTS { ?emotion rdf:type ocd:NegativeEmotion. }
        FILTER EXISTS { ?impairment rdf:type ocd:Impairment. }
        FILTER EXISTS { ?duration rdf:type ocd:SeverityDurationLevel. }
    }
}
\end{lstlisting}

The two SPARQL queries provided illustrate the fundamental differences between obsession and compulsion as conceptualised within the OCD ontology. The query for obsession targets instances classified under intrusive thoughts, intrusive images, or intrusive urges. Alternatively, it identifies instances of thoughts, images, or urges that maintain specific associations, namely, negative emotion, impairment, and duration, encapsulating the essence of obsessions as involuntary and intrusive mental phenomena that significantly burden an individual's consciousness, often spurring significant distress or anxiety. 
Conversely, the query for compulsion focuses on retrieving instances classified as compulsive behaviors or activities, encompassing the physical or observable actions individuals feel compelled to perform in response to an obsession or according to rigid rules. This highlights the behavioral dimension of OCD, where compulsions are performed in an attempt to reduce the distress caused by obsessions or to prevent some feared event or situation, despite often being recognized as excessive or unreasonable. Together, these queries and their distinctions underline the interplay between obsessions and compulsions components of OCD. They showcase how SPARQL queries, tailored to the structured knowledge within an ontology, can distinguish and retrieve detailed information on these two critical aspects of OCD. 




CQ3. Is OCD a brain disease? 

\begin{lstlisting}[language=SPARQL]

SELECT ?ocdInstance ?class ?parentClass
WHERE {
  ?ocdInstance rdf:type ?class .
  OPTIONAL { ?class rdfs:subClassOf ?parentClass . }
  FILTER (?class = ocd:OCD || ?parentClass = ocd:OCD)
}
\end{lstlisting}

The SPARQL query crafted to extract instances of OCD along with their hierarchy within an ontology directly addresses the competency question (CQ3) of whether OCD is a brain disease. By retrieving instances classified under OCD and exploring their placement within the ontological structure, the query illuminates the nature of OCD as conceptualised within the ontology's framework. 

CQ4. Do persons with OCD experience changes in sleeping or eating habits?

\begin{lstlisting}[language=SPARQL]

SELECT ?subject ?class
WHERE {
  ?subject rdf:type ?class .
  FILTER (
    ?class = ocd:OCD || 
    ?class = ocd:Obsession || 
    ?class = ocd:Compulsion ||
    ?class rdfs:subClassOf*<http://purl.obolibrary.org/obo/SYMP_0000462>
  )
}
\end{lstlisting}


The SPARQL query helps to answer the CQ4 by identifying individuals with OCD (or related conditions like obsessions or compulsions) and symptom. By retrieving instances that meet these criteria, the query provides evidence of the manifestation of general symptoms such as change in eating habit among individuals with OCD, thereby addressing the question of whether changes in sleeping or eating habits are experienced by persons with OCD. 

CQ5. Do individuals with OCD frequently experience intrusive thoughts disrupting social activities?

\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  ?individual rdf:type ocd:IntrusiveThought .
  ?individual ocd:hasAssociatedImpairment ?impairment .
  ?impairment rdf:type/rdfs:subClassOf* ocd:Functional_Impairment.
}

\end{lstlisting}


The SPARQL query designed to address CQ5 identifies individuals with OCD who are affected by intrusive thoughts and whose social engagements are adversely impacted. By querying for instances that meet both criteria—having intrusive thoughts and experiencing social impairment—this query reveals how intrusive thoughts disrupt social activities among those with OCD. 

CQ6. Do you ever experience unwanted, repetitive, and persistent thoughts that cause you anxiety?

\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  ?individual rdf:type ocd:IntrusiveThought .
}
or
SELECT ?individual
WHERE {
  ?individual rdf:type ocd:Thought .
  ?individual ocd:hasAssociatedEmotion ocd:NegativeEmotion .
}
\end{lstlisting}

These queries are designed to retrieve specific instances that reflect the criteria described in CQ6 . Query 1 straightforwardly fetches instances classified under Intrusive Thought, directly addressing the concept of persistent and unwanted thoughts. Query 2, on the other hand, takes a broader approach by looking for instances of Thought that are specifically linked to NegativeEmotion, encapsulating the emotional distress aspect associated with such thoughts. Both approaches effectively demonstrate how the ontology can answer the question about experiencing distressing, repetitive thoughts.  

CQ7. Do you experience intrusive thoughts that are aggressive (i.e., harm to yourself or
others) or about taboo topics such as porn?

\begin{lstlisting}[language=SPARQL]
SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:AggressiveThought .
  } UNION {
    ?individual rdf:type ocd:MorbidThought .
  }
}
\end{lstlisting}

This query is designed to retrieve specific instances reflective of aggressive or taboo thoughts. 


CQ8. Do you spend at least one hour a day thinking obsessive thoughts or performing ritualistic behavior in an attempt to avoid angst? If so, how often?

\begin{lstlisting}[language=SPARQL]
SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:IntrusiveThought .
    ?individual ocd:hasAssociatedDuration ?duration .
    ?duration a ocd:Severity_Duration_Level .
  } UNION {
    ?individual rdf:type ocd:CompulsiveBehavior .
    ?individual ocd:hasAssociatedDuration ?duration .
    ?duration a ocd:Severity_Duration_Level .
  }
}

\end{lstlisting}


This query is constructed to extract instances of both Intrusive Thought and Compulsive Behavior that are linked with a duration classified as Severity Duration Level, indicating a severe impact on daily functioning, consistent with spending a significant part of the day engaged in these behaviors or thoughts.  The inclusion of the hasAssociatedDuration property, coupled with the filter for Severity DurationLevel, specifically targets those instances that reflect a substantial time investment in these activities, aligning with the criteria specified in the competency question. By executing this query, we can identify instances where individuals experience either intrusive thoughts or engage in compulsive behaviors for extended periods. 


CQ9.  Do people with OCD believe their irrational thoughts?



\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  ?individual rdf:type ?thoughtClass .
  ?thoughtClass rdfs:subClassOf* ocd:Thought .
  ?individual ocd:hasAssociatedAppraisal ?appraisal .
  ?appraisal rdf:type ?appraisalType .
  ?appraisalType rdfs:subClassOf* ocd:ThoughtAppraisal .
}


\end{lstlisting}

This query targets instances identified as type of thought. The rdfs:subClassOf* pattern is employed to ensure inclusivity of all sub-classes derived from the Thought class, capturing a wide range of thought types associated with OCD. The key aspect of this query is the hasAssociatedAppraisal property, which connects each thought instance to a specific appraisal, signifying the individual's evaluation or belief regarding the thought. The ?appraisal rdf:type ?appraisalType and subsequent ?appraisalType rdfs:subClassOf* ocd:ThoughtAppraisal patterns are critical for identifying the nature of this appraisal, ensuring that it falls within the spectrum of ThoughtAppraisal or any of its sub-classes. This structure allows for the extraction of thoughts that are not only present but are also subject to an appraisal process. 


CQ10. Do individuals with OCD excessively worry about dirt, germs, or chemicals? 

\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:Contamination_Thought .
    ?individual ocd:hasAssociatedEmotion ?emotion .
    ?individual ocd:hasDuration ?duration .
  }
  UNION
  {
    ?individual rdf:type ocd:Contamination_Intrusive_Thought .
  }
}

\end{lstlisting}

The query is structured to achieve two main objectives: the first segment of the query targets instances of Contamination thought and further filters these instances based on associated emotional responses and the duration of these thoughts. The inclusion of ?individual ocd:hasAssociatedEmotion ?emotion and ?individual ocd:hasDuration ?duration criteria aims to capture not just the presence of such thoughts but also their emotional significance and persistence. This mirrors the "excessive" aspect by highlighting thoughts that not only occur but linger and evoke strong emotional reactions. 



CQ11. Is there a concern with symmetry and order in OCD individuals?

\begin{lstlisting}[language=SPARQL]
SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:SymmetryThought .
  }
  UNION
  {
    ?individual rdf:type ocd:SymmetryMentalImage .
  }
  UNION
  {
    ?individual rdf:type ocd:SymmetryUrge .
  }
}
\end{lstlisting}

This query retrieves instances that are explicitly related to the theme of symmetry; by targeting instances of Symmetry thought, Symmetry mental image, and Symmetry urge. 


CQ12. Do you ever fear contamination (i.e., germs) from people or the environment and engage in excessive cleaning? If so, how often?

\begin{lstlisting}[language=SPARQL]
SELECT ?individual
WHERE {
  ?individual ocd:hasThought ?thought .
  ?thought rdf:type ocd:ContaminationThought .
  
  ?individual ocd:exhibitsBehavior ?behavior .
  ?behavior rdf:type ocd:CompulsiveCleaning .
}
\end{lstlisting}

This query identifies instances representing individuals who harbor thoughts of contamination as well as those who exhibit compulsive cleaning behaviors; by the extraction of individuals categorised under Contamination thought and Compulsive cleaning. 

CQ13. Do you have unwanted thoughts of a sexual nature that you find inappropriate?


\begin{lstlisting}[language=SPARQL]
SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:IntrusveSexualThought .
  }
  UNION
  {
    ?individual rdf:type ocd:IntrusiveSexualMentalImage .
  }
  UNION
  {
    ?individual rdf:type ocd:IntrusiveSexualUrge .
  }
\end{lstlisting}


This SPARQL query targets instances of intrusive sexual thoughts, mental images, or urges, thereby directly responding to CQ13.

CQ14. Do individuals with OCD encounter distressing, unwanted mental images?

\begin{lstlisting}[language=SPARQL]
SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:IntrusveSMental Image .
  }
\end{lstlisting}


This specific query is crafted to retrieve individuals who are categorised under class of intrusive mental imagery.



CQ15. Do individuals with OCD struggle with uncontrollable urges consuming significant time?

\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:Intrusiv_urge .
    ?individual ocd:hasDuration ?duration .
    ?duration  rdf:type ocd:Severity_Duration_Level
  }
}

\end{lstlisting}

CQ16. How challenging is it to resist OCD thoughts?

\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  {
    ?individual rdf:type ocd:Thought
    ?individual ocd:hasResistance ?resistance .
    ?resistance  rdf:type ocd:Severity_Resistance_Level
  }
}

\end{lstlisting}


CQ17. Do Intrusive thought or repetitive behavior significantly interfere with normal routine, occupational, or academic functioning? 


\begin{lstlisting}[language=SPARQL]
SELECT ?individual ?impairment
WHERE {
  {
    ?individual rdf:type ocd:IntrusiveThought .
    ?individual ocd:hasAssociatedImpairment ?impairment .
    ?impairment rdf:type ocd:FunctionalImpairment .
  }
  UNION
  {
    ?individual rdf:type ocd:CompulsiveBehavior .
    ?individual ocd:hasAssociatedImpairment ?impairment .
    ?impairment rdf:type ocd:FunctionalImpairment .
  }
}

\end{lstlisting}

CQ18. Do you spend at least one hour a day thinking obsessive thoughts or performing ritualistic behavior in an attempt to avoid angst? If so, how often?

\begin{lstlisting}[language=SPARQL]
SELECT ?individual ?durationValue
WHERE {
  {
    ?individual rdf:type ocd:IntrusiveThought .
    OPTIONAL { ?individual ocd:hasAssociatedDuration ?duration . 
               ?duration ocd:hasDurationValue ?durationValue . }
  } UNION {
    ?individual rdf:type ocd:CompulsiveBehavior .
    OPTIONAL { ?individual ocd:hasAssociatedDuration ?duration . 
               ?duration ocd:hasDurationValue ?durationValue . }
  }
  FILTER (xsd:string(?durationValue))
}
\end{lstlisting}


CQ19. What differentiates normal from compulsive checking in OCD?

\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  ?individual rdf:type/rdfs:subClassOf* ocd:Checking .
  ?individual rdf:type ocd:CompulsiveChecking .
}
\end{lstlisting}

CQ20. Do you experience the need to constantly check on something (i.e., repeatedly checking to be sure doors are locked, light switches, and/or appliances are off) or arrange the order of things (a shelf in a bedroom or a kitchen cabinet, for example)? 

\begin{lstlisting}[language=SPARQL]

SELECT ?individual
WHERE {
  ?individual rdf:type/rdfs:subClassOf* ocd:Checking .
  ?individual rdf:type ocd:CompulsiveChecking .
}
\end{lstlisting}



CQ21. What thought patterns are common in OCD individuals exhibiting reassurance behavior? 

\begin{lstlisting}[language=SPARQL]
SELECT ?reassuranceBehavior ?thoughtClass
WHERE {
  ?reassuranceBehavior rdf:type ocd:ReassuranceBehavior .
  ?reassuranceBehavior rdf:type  ?thought .
  ?thought rdf:type ?thoughtClass .
  FILTER(?thoughtClass != owl:NamedIndividual)
}

\end{lstlisting}

To identify common thought patterns among OCD individuals exhibiting reassurance-seeking behaviors, we queried for "ReassuranceBehavior" instances and their related thought classes. 

CQ22. How does OCD impact life quality?

\begin{lstlisting}[language=SPARQL]
SELECT ?individual ?impairmentType
WHERE {
  ?individual rdf:type ocd:OCD .
  ?individual ocd:hasImpairment ?impairment .
  ?impairment rdf:type ?impairmentType .
  ?impairmentType rdfs:subClassOf* ocd:FunctionalImpairment .
}
\end{lstlisting}

Investigating how OCD affects life quality, we linked individuals with OCD to their specific types of functional impairments. 

CQ23. What psychological disorders often co-occur with OCD?


\begin{lstlisting}[language=SPARQL]
SELECT ?individual ?comorbidityType
WHERE {
  ?individual rdf:type ocd:OCD.
  ?individual ocd:hasComorbidity ?comorbidity.
  ?comorbidity rdf:type ?comorbidityType.
  FILTER EXISTS { ?comorbidityType rdfs:subClassOf* ocd:MentalDisorder }
}

\end{lstlisting}

We examined the prevalence of co-occurring psychological disorders with OCD by identifying OCD instances and their comorbidities. This exploration into "OCD" individuals having "Comorbidity" with different "MentalDisorder" classes illuminated the complex interplay between OCD and other mental health challenges.

CQ24. DO thought, impulse or image come from your own mind?

\begin{lstlisting}[language=SPARQL]
SELECT ?instance ?trigger ?triggerType
WHERE {
  {
    ?instance rdf:type ocd:Thought .
    ?instance ocd:hasTrigger ?trigger .
    ?trigger rdf:type/rdfs:subClassOf* ocd:Trigger .

  }
  UNION
  {
    ?instance rdf:type ocd:MentalImage .
    ?instance ocd:hasTrigger ?trigger .
    ?trigger rdf:type/rdfs:subClassOf* ocd:Trigger .

  }
  UNION
  {
    ?instance rdf:type ocd:Urgent .
    ?instance ocd:hasTrigger ?trigger .
    ?trigger rdf:type/rdfs:subClassOf* ocd:Trigger .

  }
}

\end{lstlisting}




