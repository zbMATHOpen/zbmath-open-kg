# The zbMATH Open Knowledge Graph
## Overview

The zbMATH Open Knowledge Graph (KG) is a large-scale RDF knowledge graph constructed from the zbMATH Open platform. Its combination of temporal depth spanning centuries of mathematical scholarship and expert-curated, domain-specific content provides an open semantic infrastructure for studying the development of mathematical knowledge and tracing scholarly connections across centuries of scholarship. 

The full zbMATh Open KG RDF data dumps are available via [Zenodo](https://doi.org/10.5281/zenodo.21497975). The knowledge graph is constructed using established Semantic Web vocabularies and ontologies and designed to align with FAIR principles; see our [paper](https://arxiv.org/abs/2609.00969) for details. 

**Update**: The zbMATH Open Knowledge Graph has been updated; this description reflects the changes and features introduced in the latest version. For previous version, see [v01](./v01/).

---
## Contents

- [Ontology and Metadata Descriptions](#ontology-and-metadata-descriptions)
- [Key Statistics](#key-statistics)
- [Key Features](#key-features)
- [Construction and Setup](#construction-and-setup)
  - [Prerequisites](#prerequisites)
  - [Data Harvesting](#data-harvesting)
  - [RDF Construction](#rdf-construction)
  - [RDF Triple Store Setup](#rdf-triple-store-setup)
- [Query Examples](#query-examples)
- [Repository Structure](#repository-structure)
- [Citation](#citation)
- [License](#license)
- [Contact and Maintenance](#contact-and-maintenance)

## Ontology and Metadata Descriptions

We provide machine-readable ontology and dataset metadata descriptions for zbMATH Open KG:

- [zbMATH Open Ontology (OWL)](./data/v01/zbmath-kg-ontology.ttl)
- [DCAT + VoID + PROV-O Description](./data/zbmath-kg-dcat-void-prov.ttl) — A DCAT + VoID dataset metadata file with PROV-O provenance and full ETL pipeline documentation

## Key Statistics

The following statistics are reported as of July 2026. The current KG was constructed using the January 2026 [zbMATH API](https://oai.zbmath.org/) data dump.

- **Temporal Span**: 1763~2025.
  - See [`decade aggregation count`](./src/retrieval-tasks/decade-aggregation-count.csv) for the per-year distribution.
  - Although the dataset includes some records from earlier years, its coverage is only complete from 1868 to the present day, according to [zbmath.org](https://zbmath.org/about/). 
- **Triples**: 168M+
- **Distinct Entities**: 34M+
- **Publications**: 4M+
- **Structured identifiers (zbMATH ID, DOI)**: 4M+
- **Disambiguated Person (Authors & Reviewers)**: 1M+
- _(expert-curated)_ **Reviews**: 3M+
- _(expert-curated)_ **Subject classifications (MSC)**: 6.7K+
- _(expert-curated)_ **Keywords**: 3M+
- _(expert-curated)_ **Software records**: 30K+
- **Organizations/Publishers**: 3.5K+
- **Periodicals**: 7.3K+ ... (and more)

## Key Features

- **RDF-Based Knowledge Graph**  
  The zbMATH Open KG is built entirely from RDF triples using widely adopted Semantic Web vocabularies and ontologies (e.g., ``schema:, dcterms:, skos:, cito:``), supporting interoperability and adhering to Linked Open Data principles. In addition, URI resolution is available for core entities (i.e., publications, scholars, software references), with cross-links with external URLs and persistent identifiers (e.g., DOI). 
 The full RDF dumps is published on [**Zenodo**](https://doi.org/10.5281/zenodo.21497975). A sample of 200 records is available here: [`data/subset-200.ttl`](./data/subset-200-v2.ttl). 

- **Expert-Curated Mathematical Metadata**  
  In addition to standard bibliographic metadata, the KG incorporates annotated mathematical publications with expert-curated reviews and controlled keywords, software references, disambiguated authors, and *Mathematics Subject Classification* (MSC) codes -- a fine-grained ontology for mathematical subject classification.

- **KG Validation**  
  The KG was validated at three complementary levels: (i) _syntactic validity_ of the RDF serialization, (ii) _structural consistency_, and (iii) _Competency Question-based evaluation_ for testing the coverage. See [CQs](./cqs) for the full set of CQs and SPARQL queries used to execute them.

- **Historically-Grounded Scholarly Discovery and Exploration**  
  Its extensive temporal coverage and expert-curated semantic contents supports the exploration of mathematical knowledge across generations of scholarship, e.g., identifying connections beyond citation and tracing conceptual lineages across (_sub_)fields. See our [paper](https://arxiv.org/abs/2609.00969) for examples of these use cases.

- **SPARQL Query Interface**  
  A SPARQL endpoint (temporarily at [**SPARQL endpoint url**](http://212.227.170.235:8890/sparql)) for directly executing queries over the KG.
  
## Construction and Setup

### Prerequisites

- Python 3.12+
- Docker (for running RDF triple stores)  
- Python libraries: `rdflib`, `SPARQLWrapper`, and others (see requirements.txt)  
  
### Raw Data Harvesting

To harvest raw jsonl data by zbMATH ID (e.g., ID list of zbMATH open access subset: [zbMATH OA subset](https://zenodo.org/records/8021789)), run:

```bash
python src/harvest-by-id.py 
```

For bulk download (via _sickle_), refers to: [zbMATHOpen Harvester](https://github.com/zbMATHOpen/mscHarvester)

### RDF Construction

Using raw `.jsonl` zbMATH data obtained from the API (see example: [`data/subset-200.jsonl`](./data/subset-200.jsonl)), run the following commands to automatically generate the RDF KG:

```bash
# Option 1: Run the Python script
python src/create-rdf-v2.py --inp data/subset-200.jsonl --out data/subset-200.ttl --nt 0

# Option 2: Run the shell script for batch processing
run-convert.sh

```

### RDF Triple Store Setup

To explore the knowledge graph and query the data using SPARQL, the RDF data needs to be loaded into an RDF triple store. After downloading the full RDF dumps from [**Zenodo**](https://doi.org/10.5281/zenodo.21497975), extract the archive and load the RDF data into your preferred RDF triple store. Examples are provided below. 

As the full dataset is large, we also provide a small sample subset of the zbMATH Open KG data for convenience: [`data/subset-200.ttl`](./data/subset-200-v2.ttl). We provide setup examples for loading the knowledge graph into the following RDF triple stores: 

- [Openlink Virtuoso](https://virtuoso.openlinksw.com/): see [`zb-virtuoso`](./zb-virtuoso) directory.
- [Apache Jena Fuseki](https://jena.apache.org/documentation/fuseki2/): see the [`front/`](./front) directory.

## Query Examples

We provide SPARQL query examples for exploring the knowledge graph, covering its main retrieval and relational capabilities, including bibliographic information, scholarly relationships, and temporal and historical information; please refer to [CQs](./cqs) for these query examples.

## Repository Structure

- [`cqs/`](./cqs) – All competency questions along with their SPARQL queries and description.
- [`data/`](./data) – `.jsonl` raw data and `.ttl` RDF KG (subset), ontology files (`.ttl`), etc.
- [`front/`](./front) – Fuseki triple store setup for serving the RDF subset (example only — SPARQL endpoint runs on Virtuoso for scalability)
- [`src/`](./src) – Source code for KG construction (data harvest, statistics calculation, RDF transformation, etc).
- [`src/retrieval-tasks/`](./src/retrieval-tasks/) – Source code and SPARQL queries for historically-grounded scholarly exploration and discovery.
- [`use-case/`](./use-case) – Evaluation and Use case-specific results and visualizations
- [`v01/`](./v01) – Earlier version of the KG
- [`zb-virtuoso/`](./zb-virtuoso) – Docker settings for the Virtuoso triple store
- [`run-convert.sh`](./run-convert.sh) – Shell script to convert raw data into RDF format
- [`README.md`](./README.md) – Project documentation

## Citation

```
@misc{susanti2026zbmathopenknowledgegraph,
      title={The zbMATH Open Knowledge Graph: Tracing Centuries of Mathematical Research}, 
      author={Yuni Susanti and Moritz Schubotz},
      year={2026},
      eprint={2609.00969},
      archivePrefix={arXiv},
      primaryClass={cs.DL},
      url={https://arxiv.org/abs/2609.00969}, 
}
```

## License

All content generated by zbMATH Open KG is distributed under [CC-BY-SA 4.0.](https://creativecommons.org/licenses/by-sa/4.0/), in accordance with the specification at [zbMATH Open OAI-PMH API](https://oai.zbmath.org/):
```
Content generated by zbMATH Open, such as reviews, classifications, software, or author disambiguation data,are distributed under CC-BY-SA 4.0.
This defines the license for the whole dataset, which also contains non-copyrighted bibliographic metadata and reference data derived from I4OC (CC0).
Note that the API only provides a subset of the data in the zbMATH Open Web interface.
In several cases, third-party information, such as abstracts, cannot be made available under a suitable license through the API.
In those cases, we replaced the data with the string "zbMATH Open Web Interface contents unavailable due to conflicting licenses."
```

## Contact and Maintenance:

The construction process is designed as a reproducible pipeline over the continuously maintained zbMATH Open platform. The knowledge graph is planned to be updated periodically, approximately twice per year subject to the availability of the underlying data stream, resource and infrastructure constraint.

📧 yuni.susanti@fiz-karlsruhe.de
