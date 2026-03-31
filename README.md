# Web Semantics Project: Movie Knowledge Graph

Movie knowledge graph project with RDF, Wikidata linking, SWRL reasoning, KGE, and SPARQL-grounded RAG.

## Overview

This repository contains an end-to-end semantic web pipeline in the movie domain. The project starts from web/API data collection, builds a private RDF knowledge graph, links it to Wikidata, enriches it with external knowledge, applies symbolic reasoning and knowledge graph embeddings, and finally evaluates graph-grounded question answering over RDF/SPARQL with a local Ollama model.

The goal was not to build a perfect knowledge base, but to implement a complete and defensible pipeline across crawling, information extraction, graph construction, alignment, reasoning, embeddings, and graph-grounded QA.

## Project Pipeline

### Notebook 1 — Data Crawling
Collects film data from Wikidata and plot/summary text from Wikipedia, stores raw outputs, cleaned summaries, and missing-summary logs.

Main outputs:
- `artifacts/notebook_1/wikidata_films.json`
- `artifacts/notebook_1/wiki_plots.jsonl`
- `artifacts/notebook_1/wiki_missing_summaries.csv`
- `artifacts/notebook_1/cleaned_plots.jsonl`

### Notebook 2 — Cleaning, NER, and Relation Extraction
Cleans summaries, applies named entity recognition, and extracts candidate relations for downstream graph construction.

Main outputs:
- `artifacts/notebook_2/extracted_entities.csv`
- `artifacts/notebook_2/extracted_relations.csv`
- `artifacts/notebook_2/extracted_knowledge.csv`

Key results:
- Cleaned summaries: **580**
- Entity mentions: **5262**
- Candidate relations: **655**

Interpretation:
This stage produces useful candidate structured knowledge, but the extraction is noisy and should be interpreted as support for KG construction rather than as perfect fact extraction.

### Notebook 3 — RDF Graph and Ontology Construction
Builds the initial private RDF graph by combining structured movie metadata with extracted entities and candidate relations.

Main outputs:
- `artifacts/notebook_3/ontology.ttl`
- `artifacts/notebook_3/initial_graph.ttl`
- `artifacts/notebook_3/initial_graph.nt`
- `artifacts/notebook_3/combined_graph.ttl`

Key graph statistics:
- Film nodes: **1030**
- `directedBy`: **666**
- `hasCastMember`: **3008**
- `hasGenre`: **856**
- `hasCountry`: **985**
- `wonAward`: **68**
- `producedBy`: **271**
- `followedBy`: **10**
- `precededBy`: **22**
- `mentionsEntity`: **5095**
- Extracted relation edges: **655**

Interpretation:
This is the first substantial project knowledge graph: film-centered, text-enriched, and suitable for linking and downstream reasoning.

### Notebook 4 — Entity Linking and Graph Expansion
Links local entities and predicates to Wikidata and expands the graph with one-hop external knowledge.

Main outputs:
- `artifacts/notebook_4/alignment_graph.ttl`
- `artifacts/notebook_4/expanded_graph.ttl`
- `artifacts/notebook_4/expanded_graph.nt`
- `artifacts/notebook_4/combined_expanded_graph.ttl`

Key results:
- Alignment graph triples: **4850**
- `owl:sameAs` links: **4841**
- `owl:equivalentProperty` links: **9**
- Expanded graph triples: **19875**
- Combined expanded graph triples: **42958**

Interpretation:
This notebook is a major transition point in the pipeline. The local RDF graph becomes a linked and enriched KG that supports reasoning, embeddings, and graph-grounded QA.

### Notebook 5 — Reasoning and Knowledge Graph Embedding
Applies SWRL reasoning and trains knowledge graph embedding models on a cleaned subset of the graph.

Main outputs:
- `artifacts/notebook_5/reasoned_graph.ttl`
- `artifacts/notebook_5/reasoning_input.owl`
- `artifacts/notebook_5/movie_swrl_input.json`
- `artifacts/notebook_5/movie_swrl_output.json`
- `artifacts/notebook_5/train.txt`
- `artifacts/notebook_5/valid.txt`
- `artifacts/notebook_5/test.txt`
- `artifacts/notebook_5/split_summary.json`
- `artifacts/notebook_5/model_comparison.csv`
- `artifacts/notebook_5/model_comparison.json`
- `artifacts/notebook_5/size_sensitivity.csv`

#### SWRL reasoning
Family demo rule:
- `Parent(?p), hasChild(?p, ?c) -> Caregiver(?p)`

Movie rule:
- `Film(?f) ∧ wonAward(?f, ?a) → AwardWinningFilm(?f)`

Movie reasoning results:
- Inferred unique `AwardWinningFilm` instances: **28**

#### KGE dataset
- Triples kept for KGE: **3213**
- Unique entities: **1826**
- Unique relations: **61**

Split summary:
- Train: **2782**
- Valid: **223**
- Test: **208**

Coverage:
- No unseen entities or relations in validation/test versus train

#### KGE model comparison
**TransE**
- MRR: **0.0574**
- Hits@1: **0.0000**
- Hits@3: **0.0865**
- Hits@10: **0.1346**

**DistMult**
- MRR: **0.0287**
- Hits@1: **0.0120**
- Hits@3: **0.0264**
- Hits@10: **0.0625**

Best model by MRR:
- **TransE**

Interpretation:
The embedding results are usable but weak in absolute terms. This is understandable given graph sparsity, extraction noise, alignment imperfections, and mixed entity types. TransE consistently outperformed DistMult, but the embeddings should not be oversold.

#### Size sensitivity
Performance improved as graph size increased:
- 1000 triples subset: MRR **0.013281**
- 2000 triples subset: MRR **0.025176**
- Full 3213-triple subset: MRR **0.047353**

This supports the claim that additional relational evidence improves KGE quality.

### Notebook 6 — RAG over RDF/SPARQL
Implements graph-grounded QA over RDF/SPARQL using a local Ollama model.

Main outputs:
- `artifacts/notebook_6/baseline_vs_rag_evaluation.csv`
- `artifacts/notebook_6/baseline_vs_rag_evaluation.json`
- `artifacts/notebook_6/schema_summary.json`

Environment used:
- Ollama model: **`gemma3:4b`**
- Graph used: `artifacts/notebook_5/reasoned_graph.ttl`

Key result:
- Loaded graph size: **42989 triples**

Evaluation:
The final graph-grounded pipeline successfully answered all **6/6** evaluation questions. Compared with baseline local LLM answers, the RDF/SPARQL-grounded answers were more specific, transparent, and traceable to graph content.

Interpretation:
This notebook is in a solid submission state. However, answer quality is still bounded by graph quality and upstream extraction/alignment noise.

---

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
│  └─ tsne_embedding.png
├─ reports/
├─ requirements.txt
├─ README.md
└─ .gitignore