# Scala Spark

### Import libraries and create Hive context
```scala
import org.apache.spark.sql.{DataFrame, Row}
import org.apache.spark.sql.hive._
import org.apache.spark.sql.types.{StructType,StructField,StringType,DoubleType}
import org.apache.spark.rdd.RDD

val hc = new org.apache.spark.sql.hive.HiveContext(sc)
import hc.implicits._
```

### DataFrame - [API](http://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.DataFrame)
```scala
val df = hc.sql("select * from db.table")

df.show()
df.printSchema()
df.columns

df.select("name").show()
df.groupBy("age").count().show()

val foo = df.groupBy("col1", "col2")
            .count()
            .filter("col2 = 'value'")
            .sort($"count".desc)
            .drop("col2")
```
