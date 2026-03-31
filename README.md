# Web Semantics Project: Movie Knowledge Graph

An end-to-end Semantic Web project in the movie domain, covering data acquisition, information extraction, RDF knowledge graph construction, Wikidata linking, SWRL reasoning, knowledge graph embeddings, and graph-grounded question answering over RDF/SPARQL.

## Overview

This repository implements a complete six-stage pipeline:

1. Data crawling from Wikidata and Wikipedia
2. Cleaning, named entity recognition, and relation extraction
3. RDF graph and ontology construction
4. Entity linking and graph expansion with Wikidata
5. SWRL reasoning and knowledge graph embedding
6. Graph-grounded QA with RDF/SPARQL and a local Ollama model

The project was rerun on a larger configuration centered on approximately 1000 films, and the repository contains the final notebooks and generated artifacts used for submission.

## Project Screenshot

![Example movie subgraph](images/movie_subgraph.png)

## Main Results

- Initial RDF graph built around **1030 film nodes**
- Combined expanded graph size: **42958 triples**
- Reasoned graph loaded for QA: **42989 triples**
- **28** inferred `AwardWinningFilm` instances from SWRL reasoning
- KGE subset prepared with **3213 triples**, **1826 entities**, and **61 relations**
- Two KGE models evaluated: **TransE** and **DistMult**
- Graph-grounded QA answered **6/6** evaluation questions successfully

## Repository Structure

```text
.
├─ notebooks/
│  ├─ 01_data_crawling.ipynb
│  ├─ 02_cleaning_ner_relation_extraction.ipynb
│  ├─ 03_rdf_graph_ontology.ipynb
│  ├─ 04_entity_linking_and_expansion.ipynb
│  ├─ 05_reasoning_and_kge.ipynb
│  └─ 06_rag_over_rdf_sparql.ipynb
├─ artifacts/
│  ├─ notebook_1/
│  ├─ notebook_2/
│  ├─ notebook_3/
│  ├─ notebook_4/
│  ├─ notebook_5/
│  └─ notebook_6/
├─ images/
│  └─ movie_subgraph.png
├─ reports/
├─ requirements.txt
├─ README.md
└─ .gitignore