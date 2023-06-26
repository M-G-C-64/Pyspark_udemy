### Spark DataFrames (DF)

- Import Spark Session

	```
	from pyspark.sql import SparkSession
	spark = SparkSession.builder.appName("Some name").getOrCreate()

	df = spark.read.option("header",True).csv('s3://adderss')
	df.show()

	# option - inferschema => infers the data type on it's own, otherwise considers everything as a string
	df = spark.read.option("inferSchema", True).option("header",True).csv('s3://adderss')
	df.printSchema()

	df = spark.read.option(inferSchema='True', header='True').csv('s3://adderss')
	```

--------------

- Creating your own Schema

	```
	from pyspark.sql.types import StructType, StructField, StringType, IntegerType

	own_schema = StructType([
		StructField("age", IntegerType(), True),
		StructField("name", StringType(), True),
		# As we won't perform any arthematic operations on roll no, we keep it as string
		StructField("rollno", StringType(), True),
		StructField("email", StringType(), True),
		StructField("marks", IntegerType(), True)])

	
	df = spark.read.option("header",True).schema(own_schema).csv('s3://adderss')
	```

--------------


- Create DF from RDD

	```
	# creating a sprak session
	from pyspark.sql import SparkSession
	spark = SparkSession.builder.appName("new App")
	```
	
	```
	# creating and reading sample RDD (refer RDD.md)
	from pyspark import SparkConf, SparkContext
	conf = SparkConf.setAppName("new RDD")
	sc = SparkContext.getOrCreate(conf)
	
	rdd = sc.textFile('s3://file_url')
	rdd.collect()
	```
	
	```
	# removing headers
	headers = rdd.first()
	rdd = rdd.filter(lambda x: x != header).map
	```
	
	```
	# creating DF from RDD providing headers
	columns = headers.split(',')
	dfRdd = rdd.toDF(columns)
	dfRdd.show()
	```
	
	```
	# creating Df from RDD using own scheme
	from pyspark.sql.types import StructType, StructField, StringType, IntegerType

	own_schema = StructType([
	StructField("age", IntegerType(), True),
	StructField("name", StringType(), True),
	# As we won't perform any arthematic operations on roll no, we keep it as string
	StructField("rollno", StringType(), True),
	StructField("email", StringType(), True),
	StructField("marks", IntegerType(), True)])
	
	dfRdd = spark.createDataFrame(rdd, schema = own_schema)
	dfRdd.show()
	```
	
---------------

- Select DF columns

	```
	# creating and reading df
	from pyspark.sql import SparkSession
	spark = SparkSession.builder.appName("Some name").getOrCreate()


	df = spark.read.option("header",True).csv('s3://adderss')
	df.show()
	```
	
	```
	# select based on column names
	df.select("name", "roll").show()
	
	#another method
	df.select(df.name, df.gender).show()
	```
	
	```
	# using col name
	from pyspark.sql.functions import col
	df.select(col("name"), col("gender")).show()
	```
	
	```
	# select all
	df.select(*).show()
	
	# column indexing
	df.select(df.column(3)).show()
	
	# column slicing
	df.select(df.column[2:4]).show()
	```

------

- Manipulating required Columns using ```withColumn()```
	
	```
	# changing the data type of a column
	from pyspark.sql.functions import col
	df = df.withColumn("roll", col("roll").cast("string"))
	```
	
	```
	# manipulate data
	df = df.withColumn("marks", col("marks") + 10)
	```
	
	```
	# create a new column
	# using a non-existent column name in withColumn() will create a new column
	df = df.withColumn("updated_marks", col("marks") + 11)
	```
	
	```
	# hardcoding a string value in a column
	from pyspark.sql.functions import col, lit
	df = df.withColumn("Country", lit("Usa"))
	```
	
	```
	# renaming a column using withColumn (not preferred)
	df = df.withColumn("new_column", df["old_column_name"]).drop("old_column_name")
	```
	
------

- Renaming a column using ```.withColumnRenamed()```

	```
	df = df.withColumnRenamed("old_name", "new_name")
	```

------

- Filtering Rows using ```.filter()``` / ```.where()```

	```
	df.filter(df.name == "ganesh").show()
	df.filter(col("name") == "ganesh").show()
	
	df.filter((df.gender == "man") & (df.marks > 50))
	```
	
	```
	# isin()
	courses_list = ["dsa", "aws", "gcp"]
	df.filter(df.courses.isin(courses_list)).show()
	
	# .startswith("A")
	# .endswith("A")
	```
	
	```
	# contains
	df.filter(df.courses.contains("se")).show()
	
	# like
	# sql notation
	df.filter(df.courses.like('%se%')).show()
	```
	
-------

- Count

	```
	# counting
	df.filter(df.courses == "dsa").count()
	```
-------

- Distinct

	```
	# distinct
	df.select("gender").distinct().show()
	```
	
-------

- DropDuplicates

	```
	# drop duplicates single column
	df.dropDuplicates(["gender"]).show()
	```
	
	```
	# dropDuplicates multiple colums
	df.dropDuplicates(["gender", "course"]).show()
	```
	
---------

- User Defined Functions (udf)

	```
	from pyspark.sql.functions import udf
	from pyspark.sql.types import IntegerType
	
	# sample function
	def get_total_salary(salary, bonus):
		return salary + bonus
		
	# creating the udf
	# syntax: my_udf = udf(sample_function, <ReturnType>)
	total_salary_udf = udf(get_total_salary, IntegerType())
	
	# executing
	df.withColumn("total_salary", total_salary_udf(df.salary, df.bonus)).show()
	```

-------

- Cache

	```
	# Commonly used when more than one transformation / action is performed

	df = spark.read.csv("header",True).csv('s3://adderss')
	
	# without cache, the transformations will read the data twice
	df.cache()
	
	df1 = df.filter(df["age"] > 30)
	df2 = df.groupBy("city").count()
	```
	
---------

- DF to RDD

	```
 	# Convert DF to RDD
 	rdd = df.rdd
 	```

--------

- DF SQL

  	```
   	# convert df to view
    	df.createOrReplaceTempView("student")

   	# Run SQL queries
   	spark.sql("select * from student where age > 20").show()
   	```

-------

- Write DF

  	```
   	# read DF
   	df = spark.read.option("header",True).csv('s3://adderss')

   	# write DF
   	df.write.options("header",true).csv("s3://studentData")
   	```
   	```
    	# mode() -> overwrite, append, ignore, error
    	df.write.mode("overwrite")options("header",true).csv("s3://studentData")
    	```
 
