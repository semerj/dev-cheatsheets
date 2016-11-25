# PySpark

### RDD basics - [API](https://spark.apache.org/docs/latest/api/python/pyspark.html#pyspark.RDD)
```python
xrangeRDD = sc.parallelize(data, 8)

# return RDD unique id
xrangeRDD.id()

# name newly created RDD
xrangeRDD.setName("My first RDD")

# view the lineage (the set of transformations)
xrangeRDD.toDebugString()

# view how many partitions the RDD will be split into
xrangeRDD.getNumPartitions()
```

### actions
* `count()`, `collect()`

    ```python
    subRDD = xrangeRDD.map(lambda x: x - 1)
    subRDD.collect()

    xrangeRDD.count()
    ```

* `filter()` is a _transformation_, so no tasks are run

    ```python
    filteredRDD = subRDD.filter(lambda x: x < 10)
    filteredRDD.collect()
    ```

* `first()`, `takeOrdered()`, `top()`

    ```python
    filteredRDD.first()                       # same as filtered.take(1)
    filteredRDD.takeOrdered(3)                # retrieve 2 smallest elements
    filteredRDD.takeOrdered(4, lambda x: -x)  # revierse the order
    filteredRDD.top(5)                        # retrieve 5 largest elements
    ```

* `reduce()`: function passed to reduce should be communicative and associative as reduce is applied at the partition level and then again to aggregate results from partitions

    ```python
    filteredRDD.reduce(lambda x, y: x + y)
    # subtraction is not both associative and communicative
    filteredRDD.reduce(lambda x, y: x - y)
    # not the same as above
    filteredRDD.repartition(4).reduce(lambda x, y: x - y)
    ```

* `takeSample()`

    ```python
    filteredRDD.takeSample(withReplacement=True, num=6)
    filteredRDD.takeSample(withReplacement=False, num=6, seed=500)
    ```

* `countByValue()`

    ```python
    repetitiveRDD = sc.parallelize([1, 2, 3, 1, 2, 3, 1, 2, 1, 2, 3, 3, 3, 4, 5, 4, 6])

    repetitiveRDD.countByValue()
    >>> defaultdict(<type 'int'>, {1: 4, 2: 4, 3: 5, 4: 2, 5: 1, 6: 1})
    ```

* `flatMap()`

    ```python
    numRDD = sc.parallelize([2, 3, 4])

    sorted(numRDD.flatMap(lambda x: range(1, x)).collect())
    >>> [1, 1, 1, 2, 2, 3]

    sorted(numRDD.flatMap(lambda x: [(x, x), (x, x)]).collect())
    >>> [(2, 2), (2, 2), (3, 3), (3, 3), (4, 4), (4, 4)]
    ```

* `mapValues()`: apply a function to each value of pair RDD without changing key
   ```python
   pairRDD = sc.parallelize([('a', 1), ('a', 2), ('b', 1)])

   pairRDD.mapValues(lambda x: x + 1)
   >>> [('a', 2), ('a', 3), ('b', 2)]
   ```

* `flatMapValues()`: pass each value in the key-value pair RDD through a `flatMap` function without changing the keys

    ```python
    x = sc.parallelize([("a", ["x", "y", "z"]), ("b", ["p", "r"])])

    x.flatMapValues(lambda x: x).collect()
    >>> [('a', 'x'), ('a', 'y'), ('a', 'z'), ('b', 'p'), ('b', 'r')]
    ```

* `reduceByKey()`: works better than `groupByKey()` for distributed datasets

    ```python
    pairRDD.reduceByKey(lambda x, y: x + y).collect()
    >>> [('a', 3), ('b', 1)]
    ```

* `groupByKey()`: all key-value pairs are shuffled

    ```python
    pairRDD.groupByKey().mapValues(lambda x: list(x)).collect()
    >>> [('a', [1, 2]), ('b', [1])]
    ```

* `combineByKey(createCombiner, mergeValue, mergeCombiner)`: used when combining elements but return type differs from input value type. requires three functions.

    ```python
    # Per-key average
    dataRDD = sc.parallelize([("a", 2.), ("a", 4.), ("a", 0.), ("b", 10.), ("b", 20.)])

    sumCount = dataRDD.combineByKey((lambda x: (x,1)),
                                    (lambda x, y: (x[0] + y, x[1] + 1)),
                                    (lambda x, y: (x[0] + y[0], x[1] + y[1])))

    sumCount.map(lambda key, xy: (key, xy[0]/xy[1])).collectAsMap()
    >>> [('a', 2.0), ('b', 15.0)]
    ```

* `fold()`:

    ```python
    numsRDD = sc.parallelize([1, 2, 3, 4, 5, 6, 7, 8, 20])
    sumAndCount = numsRDD
                    .map(lambda x: (x, 1))
                    .fold((0, 0), (lambda x, y: (x[0] + y[0], x[1] + y[1])))
    sumAndCount
    >>> (56, 9)

    avg = float(sumAndCount[0])/float(sumAndCount[1])
    ```

* `foldByKey()`: merges values for each key using associative f(x) and a neutral "zero value"

* `sortByKey()`: sorts RDD which consists of (key, value) pairs.

    ```python
    tmpRDD = sc.parallelize([('a', 1), ('b', 2), ('1', 3), ('d', 4), ('2', 5)])

    tmpRDD.sortByKey().first()
    >>> ('1', 3)
    ```

* `join(rdd)`: return an RDD containing all pairs of elements with matching keys

    ```python
    x = sc.parallelize([("a", 1), ("b", 4)])
    y = sc.parallelize([("a", 2), ("a", 3)])

    sorted(x.join(y).collect())
    >>> [('a', (1, 2)), ('a', (1, 3))]
    ```

### cache
If you plan to use an RDD more than once, then you should tell Spark to cache that RDD. However, if you cache too many RDDs and Spark runs out of memory, it will delete the least recently used (LRU) RDD first. Again, the RDD will be automatically recreated when accessed.

```python
filteredRDD.cache()
filteredRDD.is_cached
```

### unpersist
Spark automatically manages the RDDs cached in memory and will save them to disk if it runs out of memory. For efficiency, once you are finished using an RDD, you can optionally tell Spark to stop caching it in memory by using the RDD's `unpersist()` method to inform Spark that you no longer need the RDD in memory.

```python
# If we are done with the RDD we can unpersist it so that its memory can be reclaimed
filteredRDD.unpersist()

# Storage level for a non cached RDD
filteredRDD.getStorageLevel()
filteredRDD.cache()
>>> Serialized 1x Replicated

# Storage level for a cached RDD
filteredRDD.getStorageLevel()
>>> Memory Serialized 1x Replicated
```
