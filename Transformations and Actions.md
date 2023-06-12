### Transformations and Actions

##### Transformations:

- Operations that create new RDD's
- Lazyily evaluated
- Allow data manipulation, filtering, aggregations, schema and modifications
- map(), filter(), join(), groupBy()

------------------

##### Actions:

- Operations that trigger the transformations and return results to the driver program/ write data to external storage
- Cause the execution of previously defined transformations and execute the computational plans
- actions can return values or perfrom side effects such as writing data or file
- count(), collect(), save(), show()