# Movie Recommendation System

## Introduction
Movie Recommendation system is one of the recommendation system I am currently studying.

Currently, I use an Alternating Least Squares (ALS) algorithm with Spark APIs to predict the ratings for the movies in MovieLens small dataset with [DataBrick](https://www.databricks.com/) platform.

My next plan is to deploy the full dataset training on K8S. Or maybe use [Elastic Search](https://www.elastic.co/cn/what-is/elasticsearch) to fully deploy this small dataset as https://github.com/hu-guanwei/Movie-Recommender.

Current result can be seen from this [link](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/8554488083666860/3313189036436457/6195906399611815/latest.html)

## Dataset

 [MovieLens Latest Datasets](https://grouplens.org/datasets/movielens/latest/) Last updated 9/2018.

Small: 100,000 ratings and 3,600 tag applications applied to 9,000 movies by 600 users. 

Full: 27,000,000 ratings and 1,100,000 tag applications applied to 58,000 movies by 280,000 users. Includes tag genome data with 14 million relevance scores across 1,100 tags. Last updated 9/2018.
 
 
### Data Datail (copied from [link](https://files.grouplens.org/datasets/movielens/ml-latest-small-README.html))

#### movies: 

Movie information is contained in the file movies.csv. Each line of this file after the header row represents one movie, and has the following format:

movieId,title,genres

Movie titles are entered manually or imported from https://www.themoviedb.org/, and include the year of release in parentheses. Errors and inconsistencies may exist in these titles.

Genres are a pipe-separated list, and are selected from the following:

1. Action
2. Adventure
3. Animation
4. Children's
5. Comedy
6. Crime
7. Documentary
8. Drama
9. Fantasy
10. Film-Noir
11. Horror
12. Musical
13. Mystery
14. Romance
15. Sci-Fi
16. Thriller
17. War
18. Western
19. (no genres listed)

#### ratings: 

Each line of this file after the header row represents one rating of one movie by one user, and has the following format:

userId,movieId,rating,timestamp

The lines within this file are ordered first by userId, then, within user, by movieId.

Ratings are made on a 5-star scale, with half-star increments (0.5 stars - 5.0 stars).

Timestamps represent seconds since midnight Coordinated Universal Time (UTC) of January 1, 1970.

#### links :

Identifiers that can be used to link to other sources of movie data are contained in the file links.csv. Each line of this file after the header row represents one movie, and has the following format:

movieId,imdbId,tmdbId
movieId is an identifier for movies used by https://movielens.org. E.g., the movie Toy Story has the link https://movielens.org/movies/1.

imdbId is an identifier for movies used by http://www.imdb.com. E.g., the movie Toy Story has the link http://www.imdb.com/title/tt0114709/.

tmdbId is an identifier for movies used by https://www.themoviedb.org. E.g., the movie Toy Story has the link https://www.themoviedb.org/movie/862.

#### tags

All tags are contained in the file tags.csv. Each line of this file after the header row represents one tag applied to one movie by one user, and has the following format:

userId,movieId,tag,timestamp
The lines within this file are ordered first by userId, then, within user, by movieId.

Tags are user-generated metadata about movies. Each tag is typically a single word or short phrase. The meaning, value, and purpose of a particular tag is determined by each user.

Timestamps represent seconds since midnight Coordinated Universal Time (UTC) of January 1, 1970.

## Approach

## motivation
Dig deep into ALS  collaberative recommender engine and make recommendation with user based movie recommendation as well as find similar movies.
**motivation:** As artificial intelligence pervails in internet industry, more and more ecommerce platforms start to characterize their recommendation systems in order to provide better service. Collaborative filtering is one of the most popular recommendation algorithm which can be implemented with Alternating Least Squares (ALS) model in Spark ML. It would be a interesting and significant attempt to create a movie recommender for movie rating sites us

**step1. Data ETL and Data Exploration**
Explore the 4 datasets with movie_df(movie information), rating_information, links data frame and tags data frame. The minimum number of ratings per user is 20, and the minimum number of ratings per movie is 1. Among all 9742 movies and 610 users, there are more than 3000 movies are rated by only one users.

**step2. Online Analytical Processing**
Use spark sql to do OLAP and gain more knowledge on the movie genre. By exploring the genre of each movies, i found that there are 34 movies without genre label, and about half of movies are labeled as drama, and the least labeled genre is Film-Noir, only 87 movies.

**step3. Model Selection and Model Evaluation**
And then used ALS based approach to make our movie recommendation engine. Used grid search to output the best one with least rmse and used 3 fold cross validation to validate our final result, with training rmse = 0.64 and test rmse = 0.87 and overall rmse with 0.69.

**step4. Model Application: Recommend moive to users and Find the similar moives**
Made top 10 recommendations for user 575 and 273, and also output the top 5 similar movies for movie id 471; however as there is no id associate with movie id 463, therefore I didnot make recommendation for movie id 463. 
For user 575, the top recommendations are On the Beach, The Big Bus, Seve, Victory, Bill Hicks, Dylan Moran, Jetee etc..
For user 273, the top recommendations are On the Beach, Dylan Moran, Bill Hacks, Deathgasm, Victory, Seve, The Big Bus etc..
For movieid 471 the similar movies are What’s Eating Gilbert Grape, Term of Endearment, Angela’s Ashes, Cutting Edge, Memphis Belle etc.

**Output and Conclusion**
In this project, I built a ALS model with Spark APIs based on MovieLens dataset, predicted the ratings for the movies and made specific recommendation to users accordingly. The RMSE of the best model is approximately 0.87

## Reference：

1. F. Maxwell Harper and Joseph A. Konstan. 2015. The MovieLens Datasets: History and Context. ACM Transactions on Interactive Intelligent Systems (TiiS) 5, 4: 19:1–19:19. https://doi.org/10.1145/2827872
