## zbMATH Open KG: RDF Triple Store Setup (Apache Jena Fuseki)

### Requirements

- [Docker](https://www.docker.com/get-started) (includes Docker Compose)

This example demonstrates how to set up the zbMATH Open KG using [Apache Jena Fuseki](https://jena.apache.org/documentation/fuseki2/) as the RDF triple store. Fuseki is a lightweight SPARQL server that allows you to host and query your knowledge graph easily.

For convenience, we provide a small sample subset of the zbMATH Open KG data: [`data/subset-200-v2.ttl`](./data/subset-200-v2.ttl); the full dump can be downloaded from the [Zenodo](./). **Please note that the Fuseki configuration below uses an in-memory dataset and has only been tested with the sample data; running it with the full dump requires substantial RAM and may lead to out-of-memory issues.** For running the full zbMATH Open KG dump in a local container, we recommend testing with our [Virtuoso-based setup](../zb-virtuoso), or alternatively using Fuseki with TDB2 for persistent storage. 

Before running the example, ensure the initial data file is located in the same folder as the `docker-compose.yml` file. 
If not, update the volume mapping in `docker-compose.yml` accordingly:

```yaml
- ./subset-200-v2.ttl:/data.ttl
```

Then, start the service by running:
```bash
docker compose up -d
```

This will launch Fuseki on port 3030 and automatically load the initial data via [`fuseki-entrypoint.sh`](front/fuseki-entrypoint.sh).
As loading the data takes time, you can check the container logs:

```bash
docker compose up
```
You should see something like: 

```bash
Waiting for Fuseki to start...
Fuseki is up. Uploading Turtle files...
Uploading /data/out-1-v2.ttl...
Finished uploading /data/out-1-v2.ttl
...
All data uploads complete.

```

Your service will be available at: `http://localhost:3030/`, with SPARQL endpoint URL at: `http://localhost:3030/#/dataset/dataset/query`
