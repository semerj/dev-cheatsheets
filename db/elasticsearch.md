# Elasticsearch

## Basic

Update an existing alias

```sh
$ curl -XPUT localhost:9200/<INDEX>/_alias/<ALIAS>
```


## [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl.html)

General query structure

```
GET /_search
{
    "query": {
        ...
    },
    "_source": ["FIELD_1", "FIELD_2", "FIELD_3"],
    "size": 1
}
```

### [Basic Match Query](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-match-query.html)

Search in a specific field (`FIELD1`), return 2 records, and specify the 3 fields we want. The `match` query is of type `boolean` and does not go through a "query parsing" process.

```
{
    "query": {
        "match" : {
            "FIELD_1" : "SEARCH TERM"
        }
    }
}
```

### [Match Phrase Query](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-match-query-phrase.html)

The match phrase query requires that all the terms in the query string be present in the document, be in the order specified in the query string and be close to each other. By default, the terms are required to be exactly beside each other but you can specify the `slop` value which indicates how far apart terms are allowed to be while still considering the document a match.

```
{
    "query": {
        "multi_match" : {
            "query": "SEARCH TERM",
            "fields": ["FIELD_1", "FIELD_2"],
            "type": "phrase",
            "slop": 3
        }
    },
}
```

### [Multi Match Query](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-multi-match-query.html)

The `multi_match` query builds on the `match` query to allow multi-field queries (equivalent to `GET /_search?q=SEARCH TERM`)

```
{
    "query": {
        "multi_match" : {
            "query" : "SEARCH TERM",
            "fields" : ["_all"]
        }
    }
}
```

#### [Fuzziness](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/common-options.html#fuzziness)

Fuzzy matching can be enabled on `match` and `multi_match` queries to catch spelling errors (based on the Levenshtein distance). The fuzziness value of `AUTO` is equivalent to specifying a value of 2 when the term length is greater than 5.

```
{
    "query": {
        "multi_match" : {
            "query" : "SEARCH TERM",
            "fields": ["FIELD_1", "FIELD_2"],
            "fuzziness": "AUTO"
        }
    },
}
```


#### `fields` and per-field boosting

Query more than one field with wildcards

```
{
    "query": {
        "multi_match" : {
            "query" : "SEARCH TERM",
            "fields": ["FIELD_2", "FIELD_1*"]
        }
    }
}
```

Boost scores from `FIELD2` by a factor of 3

```
{
    "query": {
        "multi_match" : {
            "query" : "SEARCH TERM",
            "fields": ["FIELD_1", "FIELD_2^_3"]
        }
    }
}
```

#### Types of `multi_match` query

The way the `multi_match` query is executed internally depends on the `type` parameter, which can be set to

* [`best_fields`](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-multi-match-query.html#type-best-fields): (default) Finds documents which match any field, but uses the `_score` from the best field. Most useful when you are searching for multiple words best found in the same field.

    ```
    {
        "query": {
            "multi_match" : {
                "query": "SEARCH TERM",
                "type": "best_fields",
                "fields": [ "FIELD_1", "FIELD_2" ],
                "tie_breaker": 0.3
            }
        }
    }
    ```

* [`most_fields`](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-multi-match-query.html#type-most-fields): Finds documents which match any field and combines the `_score` from each field. Most useful when querying multiple fields that contain the same text analyzed in different ways.

    ```
    {
        "query": {
            "multi_match" : {
                "query": "SEARCH TERM",
                "type": "most_fields",
                "fields": [ "FIELD_1", "FIELD_1.original", "FIELD_1.shingles" ]
            }
        }
    }

    # would be executed as
    {
        "query": {
            "bool": {
                "should": [
                    { "match": { "FIELD_1": "SEARCH TERM" }},
                    { "match": { "FIELD_1.original": "SEARCH TERM" }},
                    { "match": { "FIELD_1.shingles": "SEARCH TERM" }}
                ]
            }
        }
    }
    ```

* [`cross_fields`](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-multi-match-query.html#type-cross-fields): Treats fields with the same `analyzer` as though they were one big field. Looks for each word in any field. Particularly useful with structured documents where multiple fields should match. In other words, all terms must be present in at least one field for a document to match. `fuzziness` parameter cannot be used.

    ```
    {
        "query": {
            "multi_match" : {
                "query": "SEARCH TERM",
                "type": "cross_fields",
                "fields": [ "FIELD_1", "FIELD_2" ],
                "operator": "and"
            }
        }
    }
    ```

    You can force all fields into the same group by specifying the `analyzer` parameter in the query.

    ```
    {
        "query": {
            "multi_match" : {
                "query": "SEARCH TERM",
                "type": "cross_fields",
                "analyzer": "standard",
                "fields": [ "FIELD_1", "FIELD_2", "*.edge" ]
            }
        }
    }
    ```

* [`phrase` & `phrase_prefix`](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-multi-match-query.html#type-phrase): Runs a `match_phrase`/`match_phrase_prefix` query on each field and combines the `_score` from each field. `fuzziness` parameter cannot be used.

    ```
    {
        "query": {
            "multi_match" : {
                "query": "SEARCH TERM",
                "type": "phrase",
                "fields": [ "FIELD_1", "FIELD_2" ]
            }
        }
    }
    ```

### [Compound Queries](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/compound-queries.html)

#### [Bool Query](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-bool-query.html)

Occurence types:

* `must`: The clause (query) must appear in matching documents and will contribute to the score
* `should`: The clause (query) should appear in the matching document
* `filter`: Same as `must` but score of the query will be ignored

```
GET /_search
{
    "query": {
        "bool": {
            "must": {
                "bool" : { "should": [
                    { "match": { "FIELD_1": "SEARCH" }},
                    { "match": { "FIELD_1": "TERM" }}
                ]}
            },
            "must": { "match": { "FIELD_2": "ANOTHER" }},
            "must_not": { "match": { "FIELD_2": "TERM" }}
        }
    }
}
```

#### [Function Score Query](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/query-dsl-function-score-query.html)

To factor in the value of a field into the calculation of the relevance score use `function_score` and `field_value_score`.

```
GET /_search
{
    "query": {
        "function_score": {
            "query": {
                "multi_match" : {
                    "query" : "SEARCH TERM",
                    "fields": ["FIELD_1", "FIELD_2"]
                }
            },
            "field_value_factor": {
                "field" : "COUNT_FIELD",
                "modifier": "log1p",
                "factor" : 2
            }
        }
    }
}
```
