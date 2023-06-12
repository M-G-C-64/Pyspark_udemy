# RDD Transformations 

**tips:**
- all transformations creates a new rdd file, **doesn't modify original file**
- transformations won't start running, until a rdd action is performed



### .textFile()

```rdd1 = sc.textFile("file_path)```



### map()

```rdd2 = rdd1.map(lambda x: x)```

- reads all items in the rdd1 file
- creates a new rdd file, **doesn't modify original file**



### flatMap()

```rdd2 = rdd1.flatMap(lambda x: x)```

- (unifies/ flats/ makes into single dimension) the list before printing
- [[1,2,3],[4,5,6]] => [1,2,3,4,5,6]
- creates a new rdd file, **doesn't modify original file**



### .filter()

```rdd2 = rdd1.filter(lambda x: (expression))```

- only returns items whose expression is true
- creates a new rdd file, **doesn't modify original file**



### .distinct()

```rdd2 = rdd1.distinct()```

- returns only distinct elements
- creates a new rdd file, **doesn't modify original file**



### .groupByKey()

```rdd2 = rdd1.groupByKey().mapValues(list).collect()```

- Only works when data is in the format of key, value pairs [eg: ((1,2),(3,5),(1,9)) => (1, <group(2,9)>),(3,5)]
- groups elements based on common keys and returns an interable value group
- **mapValues(list)** is used to convert this group into a python data structure (list is provided here)
- creates a new rdd file, **doesn't modify original file**



### .reduceByKey(lambda x,y: x+y)

```rdd2 = rdd1.reduceByKey(lambda x,y: x+y)```

- reduces all items with same key by performing the lambda opearation
- [eg: ((1,2),(3,5),(1,9)) => (1, <2+9>),(3,5) => (1, 11),(3,5)] addition is performed according to the lambda function



### .repartition(number_of_partitions)

```rdd1 = rdd.repartition(4)```

- used to change the number of partition, either increase or decrease




### .coalesce(number_of_partition)

```rdd1 = rdd.coalesce(2)```

- used to reduce (only to reduce) the partitions




