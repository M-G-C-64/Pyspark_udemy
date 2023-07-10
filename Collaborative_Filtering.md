## Collaborative Filtering

    It's a **recommendation technique** that predicts user preference based on preferences of similar users


#### Utility Matrix
    - A Matrix with Users as rows and items as columns used to store things like user ratings
    - Mostly used to predict missing ratings and helps in recommendation

#### Joining 2 DataFrames

    # DF1.join(DF2, 'joining_column_name', 'type_of_join')
    ratingsDF.join(moviesDF, 'movieID', "left").show()
    
