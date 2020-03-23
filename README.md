# Movie Map Reduce
In this dataset, each row contains a movie rating done by a user (e.g., user1 has rated Titanic as 10). Here is the format of the dataset: user_id::movie_id::rating::timestamp

For each pair of movies A and B, we need to find all the users who rated both movie A and B. For example, given the following dataset (for the sake of illustration we have used U and M to represent users and movies respectively in the example):

```
U1::M1::2::11111111
U2::M2::3::11111111
U2::M3::1::11111111
U3::M1::4::11111111
U4::M2::5::11111111
U5::M2::3::11111111
U5::M1::1::11111111
U5::M3::3::11111111
```

The assumption is that User and Movie names are in String format and Rating is an Integer value.

The output of the code is in the form below:

```
(M1,M2) [(U5,1,3)]
(M2,M3) [(U5,3,3),(U2,3,1)]
(M1,M3) [(U5,1,3)]
```

where (M,M) shows pairs of movies, [] indicates the list of users and their ratings. For example, (U5,1,3) shows U5 has rated M1 and M2 with 1 and 3 respectively.

## Project description:
The project involves the [chaining of two mapreduce jobs](https://stackoverflow.com/questions/38111700/chaining-of-mapreduce-jobs#answer-38113499)
* First job: UserMapper to map each user to 1 set of (movie, rating)
* First job: UserReducer to aggregate the set of (movie,rating) for each user key in the form of an ArrayWritable 
* Second job: MovieMapper to map movie pairs (movie_1,movie_2) to 1 set of (user,rating_1,rating_2) for each respective movie reviews
* Second job: MovieReducer to aggregate the set of (user,rating_1,rating_2) for each movie pair in the form of an ArrayWritable

 I/O:
* input.txt: Text file containing dataset in the form of user::movie::rating::timestamp
* output: Directory containing temporary output and final output of reducers
* output/temp: Temporary result from first MapReduce job
* output/out: Final result from second MapReduce job


## To run the project:
> $ javac -cp ".:Hadoop-Core.jar" MovieMapReduce.java <br>
> $ java -cp ".:Hadoop-Core.jar" MovieMapReduce INPUT_PATH OUTPUT_PATH

