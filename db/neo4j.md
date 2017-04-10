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
