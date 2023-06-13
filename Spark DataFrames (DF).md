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
	
