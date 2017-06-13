# Neo4j

## REST API

Query Neo4j REST API via curl POST request

```sh
curl -H "Content-Type: application/json" \
     -H "Accept: application/json" \
     -d "@query.json" http://localhost:7474/db/data/transaction/commit
```

`query.json`

```json
{ "statements": [{ "statement": "MATCH (n) RETURN n LIMIT 10" }] }
```

## [Exporting Subgraphs](https://neo4j-contrib.github.io/neo4j-apoc-procedures/#_export_to_cypher_script)

First, add to `neo4j.conf`
```
apoc.export.file.enabled=true
```

Exports nodes and relationships from the cypher statement including indexes as cypher statements to the provided file
```
CALL apoc.export.cypher.query(
  "MATCH (p1:Person)-[r:KNOWS]->(p:Person) RETURN *",
  "export.cypher",
  {}
);
```

## Importing Subgraphs

```sh
neo4j-shell -file export.cypher -path /path/to/graph.db
```
