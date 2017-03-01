### Large-scale Collaborative Ranking in Near-Linear Time

This repo consists of three folders:
- data folder: sample data “MovieLens1m.csv” can be found there, the data is of the form: “user id, movie id, rating”.
- util folder: python scripts to divide data into training and testing dataset (instructions on how to use these utilities functions are given below).
- code folder: Julia code which implements the primal-CR and primal-CR++ algorithm described in the paper is put into this folder (instructions on how to run the codes are given below).
- experimental folder: all the un-cleaned codes we initially wrote are put in this folder.




#### Instructions on how to run the code:
Our trained model can be tested in terms of NDCG@10, pairwise error and objective function.



1. Prepare a dataset of the form (user, item, ratings) triple in csv file. (Example: data/MovieLens1m.csv)

2. Run util/split1.py to get training data and test data by specifying the number of subsamples N to use as what we did in Section 5.1 in the paper 
    ```
    $ python util/split1.py data/MovieLens1m.csv -o ml1m -n 200

    ```

or run util/split2.py to get multiple training data of C = 100, 200, d2 but the same test data as what we did in Section 5.3:  


	```
	$ python util/split2.py data/MovieLens1m.csv -o ml1m
	```
	

    , where the option -n specify the number of subsampled ratings per user N in Section 5.1 and -o specify the output file name prefix. (The scripts also generate the training ratings which can be used for other methods)

3. Use command line to go to the repo folder and start Julia

    ```
    $ Julia -p 4

    ```
, where the option -p n provides n worker processes on the local machine. Use $ Julia -p 1 for single thread experiments.


4. Type 

	```
    	$ include(“code/primalCR.jl”)
    	```

 in Julia command line to load the functions for primal-CR algorithm. Similarly, to run the primal-CR++ algorithm, type 


    ```
    $ include(“code/primalCRpp.jl”)     

    ```


5. Type 


```
$ main("data/ml1m_train_ratings.csv", "data/ml1m_test_ratings.csv")
```

 in Julia command line. Stop after it starts printing and type again


```
 $ main("data/ml1m_train_ratings.csv", "data/ml1m_test_ratings.csv”)
```


 One can replace the arguments for the main function by changing the training data and test data file paths. The reason to type the same command twice is that the first time it includes the compilation time for Julia codes and the second time the codes will run much faster.

