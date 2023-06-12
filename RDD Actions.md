# RDD Actions

### .collect()

```rdd.collect()```



### .count()

```rdd1.count()```

- returns the number of items in the data




### countByValue()

```rdd1.countByValue()```

- returns how many times each elemenet is present in the data



### .saveAsTextFile()

```rdd1.saveAsTextFile()```

- creates a text file of the rdd
- if we want to save the RDD in other formats, best procedure is to convert the rdd into DataFrame (```.toDF()```) then use ```df.write.csv/json/parquet("file_name.csv")```