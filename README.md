# Web Semantics Project: Movie Knowledge Graph

This repository contains a complete web semantics pipeline built around a movie domain. The project starts from web/API data collection, builds an RDF knowledge graph, links it to Wikidata, enriches it with external knowledge, applies symbolic reasoning and knowledge graph embedding, and finally evaluates graph-grounded question answering with RDF/SPARQL and a local LLM.

## Project pipeline

### Notebook 1 - Data crawling
Collects movie data from Wikidata and Wikipedia, retrieves summaries, and stores cleaned text for downstream processing.

### Notebook 2 - Cleaning, NER, and relation extraction
Cleans summaries, extracts named entities with NLP, and produces candidate subject-predicate-object relations.

### Notebook 3 - RDF graph and ontology construction
Builds the initial private RDF knowledge graph by combining structured metadata, extracted entities, and extracted relations.

### Notebook 4 - Entity linking and graph expansion
Aligns local entities and predicates with Wikidata and expands the graph with one-hop external knowledge.

### Notebook 5 - Reasoning and knowledge graph embedding
Applies SWRL/Pellet reasoning, materializes inferred facts, prepares the graph for KGE, trains embedding models, and evaluates them.

### Notebook 6 - RAG over RDF/SPARQL
Implements graph-grounded question answering with SPARQL over RDF and compares it to a baseline local LLM answer.

## Repository structure

```text
.
|-- notebooks/
|   |-- 01_data_crawling.ipynb
|   |-- 02_cleaning_ner_relation_extraction.ipynb
|   |-- 03_rdf_graph_ontology.ipynb
|   |-- 04_entity_linking_and_expansion.ipynb
|   |-- 05_reasoning_and_kge.ipynb
|   `-- 06_rag_over_rdf_sparql.ipynb
|-- artifacts/
|   |-- notebook_1/
|   |-- notebook_2/
|   |-- notebook_3/
|   |-- notebook_4/
|   |-- notebook_5/
|   `-- notebook_6/
|-- README.md
`-- .gitignore