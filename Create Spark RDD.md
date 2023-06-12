### Create Spark RDD


```from pyspark import SparkConf, SparkContext```
**SparkConf** used for: 
- Spark properties like appName 
- Spark Master URL
- Manage configurations for SparkContext

**SparkContext** used for:
- Interacting with Apache Spark
- Creating RDD
- Managing Cluster Resources
- Executing Actions



```conf = SparkConf.setAppName("myapp")```
**setAppName** used to set a name for spark App



```sc = SparkContext.getOrCreate(conf=conf)```
**used to avoid duplicating Contexts, gets if already exists or creates a new one**



```text = sc.textFile("file_path)```
- as these spark transformations are lazy, it won't read the data yet
for other file types **csv, json, parquet, binaryFile**



```text.collect()```
- It reads the data now
- **.collect()** triggers all of the previous transformations and retreievs the RDD/DF as list/array




