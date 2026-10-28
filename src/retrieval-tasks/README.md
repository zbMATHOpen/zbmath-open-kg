## Historically-Grounded Scholarly Exploration

To demonstrate the utility of the knowledge graph, we present several use case examples we collectively term _historically grounded_ scholarly exploration. These use cases exploit the resource's distinctive combination of extensive temporal coverage and expert-curated semantics for semantic exploration of mathematical research across centuries of scholarship. 

- Precursor Discovery — identifying potentially overlooked works beyond citations  
- Conceptual Ancestry — tracing concept migration across (sub)fields  
- Revival Detection — spotting re-emerging ideas in new contexts  
- Reviewer–Author Lineage — mapping potential intellectual connection via scholarly interactions (reviewing)  

Importantly, these use cases are **not** intended to establish historical influence; rather, they demonstrate how the KG can surface relationships and patterns for
further scholarly and historical investigation. 

## Implementation

(1) Install the prerequisite libraries:

```bash
pip install -r requirements.txt
```

(2) Configure the SPARQL endpoint url in the script to match your KG's SPARQL endpoint.

```
# --- CONFIGURATION ---
endpoint_url = "http://localhost:8890/sparql"  # change into your SPARQL endpoint
```

(3) Run the following scripts to perform the respective retrieval tasks:

- **(1) Precursor Retrieval**  
  ```bash
  python precursor-retrieval.py
  ```
- **(2) Conceptual Ancestry**  
  ```bash
  python ancestry-retrieval.py
  ```
- **(3) Revival Detection**  
  ```bash
  python revival-retrieval.py
  ```
- **(4) Reviewer–Author Lineage**  
  ```bash
  python lineage-retrieval.py
  ```

