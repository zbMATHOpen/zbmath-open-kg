## Source Code

This directory contains the source code for constructing and evaluating the knowledge graph (KG), including data harvesting, RDF transformation, preprocessing, and statistical analysis.

| File / Directory | Description |
|---|---|
| [`harvest-by-id.py`](harvest-by-id.py) | Harvests raw data by identifier. |
| [`fetch-msc.py`](fetch-msc.py) | Retrieves Mathematics Subject Classification (MSC) data. |
| [`create-rdf-v2.py`](create-rdf-v2.py) | Transforms harvested data into RDF. |
| [`preprocess.py`](preprocess.py) | Preprocesses data before KG construction. |
| [`stats.py`](stats.py) | Calculates statistics for the knowledge graph. |
| [`retrieval-tasks/`](./src/retrieval-tasks/) | Source code and SPARQL queries for KG retrieval tasks and use case examples. |
| [`evaluation/`](./src/evaluation/) | Source code and SPARQL queries for KG data validation and evaluation. |
