### Spark DataFrames (DF)

- Import Spark Session

	```
	from pyspark.sql import SparkSession
	spark = SparkSession.builder.appName("Some name").getOrCreate()


	df = spark.read.option("header",True).csv('s3://adderss')
	df.show()

	```