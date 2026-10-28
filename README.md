# The zbMATH Open Knowledge Graph
## Overview

The zbMATH Open Knowledge Graph (KG) is a large-scale RDF knowledge graph constructed from the zbMATH Open platform. Its combination of temporal depth spanning centuries of mathematical scholarship and expert-curated, domain-specific content provides an open semantic infrastructure for studying the development of mathematical research across centuries of scholarship. The zbMATH Open KG is designed to align with Semantic Web ecosystem and FAIR principles; see our [paper](https://arxiv.org/abs/2609.00969) for details.

The full zbMATh Open KG RDF data dumps are available via Zenodo: [DOI:10.5281/zenodo.21497975](https://doi.org/10.5281/zenodo.21497975). 

**Update**: The zbMATH Open KG has been updated; this description reflects the changes and features introduced in the latest version. For previous version, see [v01](./v01/).

---
## Contents

- [Ontology and Metadata Descriptions](#ontology-and-metadata-descriptions)
- [Key Statistics](#key-statistics)
- [Key Features](#key-features)
- [Data Construction and Validation](#data-construction-and-validation)
  - [Prerequisites](#prerequisites)
  - [Data Harvesting](#data-harvesting)
  - [Data Construction](#data-construction)
  - [RDF Triple Store Setup](#rdf-triple-store-setup)
  - [Data Validation](#data-validation)
- [Query and Use Case Examples](#query-and-use-case-examples)
- [Repository Structure](#repository-structure)
- [Citation](#citation)
- [License](#license)
- [Contact and Maintenance](#contact-and-maintenance)

## Ontology and Metadata Descriptions

We provide machine-readable ontology and dataset metadata descriptions for zbMATH Open KG:

- [zbMATH Open Ontology (OWL)](./data/zbmath-ontology-v1.1.ttl)
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
  
## Data Construction and Validation

### Prerequisites

- Python 3.12+
- Docker (for running RDF triple stores)  
- Python libraries: `rdflib`, `SPARQLWrapper`, and others (see requirements.txt)  
  
### Data Harvesting

To harvest raw jsonl data by zbMATH ID (e.g., ID list of zbMATH open access subset: [zbMATH OA subset](https://zenodo.org/records/8021789)), run:

```bash
python src/harvest-by-id.py 
```

For bulk download (via _sickle_), refers to: [zbMATHOpen Harvester](https://github.com/zbMATHOpen/mscHarvester)

### Data Construction

Using raw `.jsonl` zbMATH data obtained from the API (see example: [`data/subset-200.jsonl`](./data/subset-200.jsonl)), run the following commands to automatically generate the RDF KG:

```bash
# Option 1: Run the Python script
python src/create-rdf-v2.py --inp data/subset-200.jsonl --out data/subset-200.ttl --nt 0

# Option 2: Run the shell script for batch processing
src/run-convert.sh

```

### RDF Triple Store Setup

To explore the knowledge graph and query the data using SPARQL, the RDF data needs to be loaded into an RDF triple store. After downloading the full RDF dumps from [**Zenodo**](https://doi.org/10.5281/zenodo.21497975), extract the archive and load the RDF data into your preferred RDF triple store. Examples are provided below. 

As the full dataset is large, we also provide a small sample subset of the zbMATH Open KG data for convenience: [`data/subset-200.ttl`](./data/subset-200-v2.ttl). We provide setup examples for loading the knowledge graph into the following RDF triple stores: 

- [Openlink Virtuoso](https://virtuoso.openlinksw.com/): see [`zb-virtuoso`](./zb-virtuoso) directory.
- [Apache Jena Fuseki](https://jena.apache.org/documentation/fuseki2/): see the [`front/`](./front) directory.

### Data Validation

To validate the structural consistency of the knowledge graph, run the queries in [`structural-validity.sparql`](./src/evaluation/structural-validity.sparql). These checks targeted various aspects of structural consistency, such as missing required properties, invalid or unexpected datatypes, and inconsistent class assignments.

Additionally, the coverage and validity was evaluated using 45 competency questions (CQs) defined by expert to assess the graph's core retrieval and relational capabilities. For each CQ, a SPARQL query was executed, and the results were manually compared with the expected answers. See the CQs and corresponding SPARQL queries here: [CQs](./cqs)

## Query and Use Case Examples

We provide SPARQL query examples for exploring the knowledge graph, covering its core retrieval capabilities, including bibliographic information, scholarly relationships, and temporal and historical information; refer to [CQs](./cqs) for these query examples. Use case examples are provided: [./src/retrieval-tasks/](./src/retrieval-tasks/).

## Repository Structure

- [`cqs/`](./cqs) – All competency questions along with their SPARQL queries and description.
- [`data/`](./data) – `.jsonl` raw data and `.ttl` RDF KG (subset), ontology files (`.ttl`), etc.
- [`front/`](./front) – Triple store setup (Fuseki) for serving the RDF subset (example only; endpoint runs on Virtuoso for scalability).
- [`src/`](./src) – Source code for KG construction (data harvesting, construction, statistics calculation, RDF transformation, preprocessing, etc).
  - [`src/evaluation/`](./src/evaluation/) – Source code and queries for RDF validation.
  - [`src/retrieval-tasks/`](./src/retrieval-tasks/) – Source code and queries for retrieval, exploration and use case examples.
- [`v01/`](./v01) – Earlier version of the KG.
- [`zb-virtuoso/`](./zb-virtuoso) – Docker settings for the Virtuoso triple store.
- [`README.md`](./README.md) – Project documentation.
- [`requirements.txt`](requirements.txt) – Library and dependency to run the source code in the repository.

## Citation

```
@misc{susanti2026zbmathkg,
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

The zbMATH Open KG is distributed under the [CC-BY-SA 4.0.](https://creativecommons.org/licenses/by-sa/4.0/) license, in accordance with the licensing terms of the underlying data source, refer to the detail [here](https://oai.zbmath.org/). The source code accompanying the zbMATH Open KG is distributed under the [MIT license](LICENSE).

## Contact and Maintenance:

The zbMATH Open KG is  maintained by FIZ Karlsruhe (Berlin) and is planned to be updated periodically, annually or twice per year, subject to the availability of the underlying data stream, resource and infrastructure constraint.

📧 yuni.susanti@fiz-karlsruhe.de
