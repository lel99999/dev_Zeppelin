# dev_Zeppelin
Zeppelin Spark Notes and Workspace

#### Sharing Notebooks


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

#### Hive Connection
- Default Connection 
  # SCALA <br/>
  ```
  import java.sql.Connection;
  import java.sql.Statement;
  import java.sql.DriverManager;

  object HiveJDBCConnect extends App{
  	var con = null;
	try {
		val conStr = "jdbc:hive2://192.168.1.100:10000/default";
		Class.forName("org.apache.hive.jdbc.HiveDriver");
		con = DriverManager.getConnection(conStr, "", "");
		val stmt = con.createStatement();
		stmt.executeQuery("Show databases");
		System.out.println("show database successfully");
	} catch (Exception ex) {
		ex.printStackTrace();
	} finally {
		try {
			if (con != null)
				con.close();
		} catch (Exception ex) {
		}
	}
  }

  ```
  
  # JAVA <br/>
  ```
  import java.sql.Connection;
  import java.sql.Statement;
  import java.sql.DriverManager;

  public class HiveJDBCConnect {
	public static void main(String[] args) {
		Connection con = null;
		  try {
			  String conStr = "jdbc:hive2://192.168.1.100:10000/default";
			  Class.forName("org.apache.hive.jdbc.HiveDriver");
			  con = DriverManager.getConnection(conStr, "", "");
			  Statement stmt = con.createStatement();
			  stmt.executeQuery("show databases");
			  System.out.println("show database successfully.");
		  } catch (Exception ex) {
			  ex.printStackTrace();
		  } finally {
			  try {
				  if (con != null)
				  	con.close();
			  } catch (Exception ex) {
			  }
		  }
	  }
  }

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
