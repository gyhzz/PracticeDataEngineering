# Introduction to Spark

### Terms

sparkSession: The entry point to all Spark functionalities such as session configuration, creating DataFrames, and running queries

#### **`SparkSession`** Methods

More information of sparkSession: https://spark.apache.org/docs/3.5.1/sql-getting-started.html#starting-point-sparksession

| Method | Description |
| --- | --- |
| sql | Returns a DataFrame representing the result of the given query |
| table | Returns the specified table as a DataFrame |
| read | Returns a DataFrameReader that can be used to read data in as a DataFrame |
| range | Create a DataFrame with a column containing elements in a range from start to end (exclusive) with step value and number of partitions |
| createDataFrame | Creates a DataFrame from a list of tuples, primarily used for testing |


#### Operations with SQL

`spark.sql("SELECT * FROM products WHERE price < 200 ORDER BY price")`

#### Operations with DataFrames

`display(spark.table('products').select('name', 'price').filter('price < 200').orderBy('price'))`

#### Transformations

| Transformation | Explanation |
|----------------|-------------|
| `select(cols)` | Selects a set of columns. |
| `filter(condition)`<br>`where(condition)` | Filters rows using the specified condition. |
| `groupBy(cols)` | Groups the DataFrame by the specified columns for aggregation. |
| `orderBy(cols)`<br>`sort(cols)` | Sorts the DataFrame by the specified column(s). |
| `join(other, on=None, how=None)` | Joins the current DataFrame with another DataFrame. |
| `withColumn(colName, colExpr)` | Adds a new column or replaces an existing column using the specified expression. |
| `drop(cols)` | Drops the specified column(s). |
| `distinct()` | Returns a new DataFrame with distinct rows. |
| `repartition(numPartitions)` | Changes the number of partitions in the DataFrame. |
| `coalesce(numPartitions)` | Decreases the number of partitions in the DataFrame. |
| `explode(col)` | Converts a column that contains arrays or maps into separate rows. |
| `pivot(col, values)` | Pivots a column of the current DataFrame to perform specified aggregations. |


#### Actions

| Action | Explanation |
|--------|-------------|
| `show(n=20)` | Displays the first `n` rows of the DataFrame. |
| `collect()` | Collects all rows and returns them as an array to the driver program. |
| `count()` | Returns the number of rows in the DataFrame. |
| `first()`<br>`head()` | Returns the first row. |
| `take(n)` | Returns the first `n` rows as an array. |
| `saveAsTable(name)`<br>`write()` | Writes the DataFrame out to external storage. |
| `toPandas()` | Converts the Spark DataFrame to a Pandas DataFrame (Python only). |
| `foreach(func)` | Applies a function to each row. |
| `foreachPartition(func)` | Applies a function to each partition of the DataFrame. |
| `explain()` | Prints the physical plan to the console for debugging. |
