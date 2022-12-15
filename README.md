# dev_Zeppelin
Zeppelin Spark Notes and Workspace


#### Load External Dependencies/Jars
```
%dep z.load("/<path>/packages.jar")
```

or

```
%spark.dep
z.reset() // clean up previously added artifact and repository

// add maven repository
z.addRepo("RepoName").url("RepoURL")

// add maven snapshot repository
z.addRepo("RepoName").url("RepoURL").snapshot()

// add credentials for private maven repository
z.addRepo("RepoName").url("RepoURL").username("username").password("password")
```

#### Zeppelin Notebook
- Load example data (bank.csv) into hdfs
  ```
  $hdfs dfs -cp /<source_path>/bank.csv /tmp/bank.csv
  ```
- Read csv file into a spark dataframe
  ```
  val df0 = spark.read.option("header",true).option("delimiter",";").csv("/tmp/bank.csv")
  ```
  
- To view the schema, enter the following code into the next cell, then hit Shift + Enter:
  ```
  df0.printSchema
  ```
  
- Using the Dataframe where function to query the data, enter the following code, then hit Shift + Enter:
  ```
  df0.where(df0("age") === "30").show()
  ```

- Read Parquet Files
  ```
  val sqlContext = new org.apache.spark.sql.SQLContext(sc)

  val df = sqlContext.read.parquet("src/main/resources/testfile.parquet")

  df.printSchema

  // after registering as a table you will be able to run sql queries
  df.registerTempTable("test")

  sqlContext.sql("select * from test").collect.foreach(println)
  ```

  ```
  //This Spark 2.x code you can do the same on sqlContext as well
  val spark: SparkSession = SparkSession.builder.master("set_the_master").getOrCreate

  spark.sql("select col_A, col_B from parquet.`hdfs://my_hdfs_path/my_db.db/my_table`")
   .show()
  ```
