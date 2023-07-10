## Collaborative Filtering

    It's a **recommendation technique** that predicts user preference based on preferences of similar users


#### Utility Matrix
    - A Matrix with Users as rows and items as columns used to store things like user ratings
    - Mostly used to predict missing ratings and helps in recommendation

### Joining 2 DataFrames

    # DF1.join(DF2, 'joining_column_name', 'type_of_join')
    ratings = ratingsDF.join(moviesDF, 'movieID', "left").show()

#### Test and Train split

    (train, test) = ratings.randomSplit([0.8, 0.2])

#### ALS

    - Alternating Least Squares
    - It's a collaborative filtering model used to predict missing user ratings
    
- code:

      from pyspark.ml.recommendation import ALS
  
      # nonnegative -> if the values are all positive ? True : False
      # implicitPref -> if the ratings are implicit ? True : False
      # coldStartStrategy -> Operation on completely null users/items
  
      ALS(userCol="userId",
          itemCol="itemId",
          ratingCol="rating",
          nonnegative=True,
          implicitPref=False,
          coldStartStrategy="drop")
  
    

    
    
