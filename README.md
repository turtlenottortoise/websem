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


Environment Setup

Install dependencies from requirements.txt:

pip install -r requirements.txt
python -m spacy download en_core_web_sm
How to Run Each Module

Run the notebooks in order.

1. Data Crawling

Notebook: notebooks/01_data_crawling.ipynb

Main tasks

collect film metadata from Wikidata
retrieve Wikipedia summaries
save cleaned text and missing-summary logs

Main outputs

artifacts/notebook_1/wikidata_films.json
artifacts/notebook_1/wiki_plots.jsonl
artifacts/notebook_1/wiki_missing_summaries.csv
artifacts/notebook_1/cleaned_plots.jsonl
2. Cleaning, NER, and Relation Extraction

Notebook: notebooks/02_cleaning_ner_relation_extraction.ipynb

Main tasks

clean collected summaries
extract named entities
generate candidate relations for graph construction

Main results

Cleaned summaries: 580
Entity mentions: 5262
Candidate relations: 655

Main outputs

artifacts/notebook_2/extracted_entities.csv
artifacts/notebook_2/extracted_relations.csv
artifacts/notebook_2/extracted_knowledge.csv
3. RDF Graph and Ontology Construction

Notebook: notebooks/03_rdf_graph_ontology.ipynb

Main tasks

build the initial private RDF graph
define the ontology
combine structured movie metadata with extracted entities and relations

Main graph statistics

Film nodes: 1030
directedBy: 666
hasCastMember: 3008
hasGenre: 856
hasCountry: 985
wonAward: 68
producedBy: 271
followedBy: 10
precededBy: 22
mentionsEntity: 5095
Extracted relation edges: 655

Main outputs

artifacts/notebook_3/ontology.ttl
artifacts/notebook_3/initial_graph.ttl
artifacts/notebook_3/initial_graph.nt
artifacts/notebook_3/combined_graph.ttl
4. Entity Linking and Graph Expansion

Notebook: notebooks/04_entity_linking_and_expansion.ipynb

Main tasks

align local entities and predicates with Wikidata
expand the graph with external one-hop knowledge

Main results

Alignment graph triples: 4850
owl:sameAs links: 4841
owl:equivalentProperty links: 9
Expanded graph triples: 19875
Combined expanded graph triples: 42958

Main outputs

artifacts/notebook_4/alignment_graph.ttl
artifacts/notebook_4/expanded_graph.ttl
artifacts/notebook_4/expanded_graph.nt
artifacts/notebook_4/combined_expanded_graph.ttl
5. Reasoning and Knowledge Graph Embedding

Notebook: notebooks/05_reasoning_and_kge.ipynb

Main tasks

apply SWRL reasoning
materialize inferred graph knowledge
prepare a cleaned subset for KGE
train and compare embedding models

SWRL reasoning

Family rule demo: Parent(?p), hasChild(?p, ?c) -> Caregiver(?p)
Movie rule: Film(?f) ∧ wonAward(?f, ?a) → AwardWinningFilm(?f)
Inferred unique AwardWinningFilm instances: 28

KGE subset

Triples kept for KGE: 3213
Unique entities: 1826
Unique relations: 61

Train / validation / test split

Train: 2782
Valid: 223
Test: 208

Model comparison

TransE

MRR: 0.0574
Hits@1: 0.0000
Hits@3: 0.0865
Hits@10: 0.1346

DistMult

MRR: 0.0287
Hits@1: 0.0120
Hits@3: 0.0264
Hits@10: 0.0625

Best model by MRR

TransE

Size sensitivity

Subset size 1000 → MRR 0.013281
Subset size 2000 → MRR 0.025176
Full subset 3213 → MRR 0.047353

Main outputs

artifacts/notebook_5/reasoned_graph.ttl
artifacts/notebook_5/reasoning_input.owl
artifacts/notebook_5/movie_swrl_input.json
artifacts/notebook_5/movie_swrl_output.json
artifacts/notebook_5/train.txt
artifacts/notebook_5/valid.txt
artifacts/notebook_5/test.txt
artifacts/notebook_5/split_summary.json
artifacts/notebook_5/model_comparison.csv
artifacts/notebook_5/model_comparison.json
artifacts/notebook_5/size_sensitivity.csv
6. Graph-Grounded QA over RDF/SPARQL

Notebook: notebooks/06_rag_over_rdf_sparql.ipynb

Main tasks

run graph-grounded QA over the RDF graph
use SPARQL to answer natural-language questions
compare graph-grounded answers with baseline local LLM responses

Final setup

Local model: gemma3:4b
Graph used: artifacts/notebook_5/reasoned_graph.ttl
Loaded graph size: 42989 triples

Evaluation summary

Final evaluation questions: 6
Graph-grounded answers returned successfully: 6/6

Main outputs

artifacts/notebook_6/baseline_vs_rag_evaluation.csv
artifacts/notebook_6/baseline_vs_rag_evaluation.json
artifacts/notebook_6/schema_summary.json
How to Run the RAG / RDF-SPARQL Module

Notebook 6 uses a local Ollama model.

Start Ollama:

ollama serve

Pull the model used in this project:

ollama pull gemma3:4b
ollama list

Then run:

notebooks/06_rag_over_rdf_sparql.ipynb
Hardware Requirements
Data crawling is network-bound rather than GPU-bound
RDF processing, reasoning, and KGE are feasible on a standard CPU setup
Ollama requires sufficient local RAM for the selected model
GPU acceleration is not necessary for the crawling stage
Reproducibility Notes
Dependencies are listed in requirements.txt
The repository includes the main generated artifacts used by downstream stages
Crawling depends on external APIs and can be affected by rate limits or missing Wikipedia pages
The included artifacts are the main reference outputs for review and grading
Key Files for Review

Most important notebooks

notebooks/03_rdf_graph_ontology.ipynb
notebooks/04_entity_linking_and_expansion.ipynb
notebooks/05_reasoning_and_kge.ipynb
notebooks/06_rag_over_rdf_sparql.ipynb

Most important final artifacts

artifacts/notebook_4/combined_expanded_graph.ttl
artifacts/notebook_5/reasoned_graph.ttl
artifacts/notebook_5/model_comparison.csv
artifacts/notebook_6/baseline_vs_rag_evaluation.csv
Final Deliverables

This repository is intended to be submitted together with:

the notebook set in notebooks/
the generated artifacts in artifacts/
the final report in reports/
the final GitHub release tagged as v1.0-final

