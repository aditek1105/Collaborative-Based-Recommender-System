1	Collaborative Filtering Recommendation System

We here use collaborative filtering to predict the user’s interests for the YouTube videos and based on that suggest/recommend the user other videos from the YouTube trending videos dataset to watch using data collected by other users like him/her.
Dummy Data Creation:
I.	Creating dummy users

As the existing dataset did not contain the list of users and their preferences or ratings for the YouTube videos, a set of dummy users and their preferences towards the 16 categories of videos that is present in the existing dataset was created.

II.	Creating dummy ratings

YouTube does not have an explicit rating attribute to its videos, a custom metric was created that converted views, likes, dislikes and comment attributes of a video to a rating which introduced randomness and relevance to the video’s ranking from the existing dataset with these four attributes. 

i)	Metric to calculate rankings on original YouTube dataset:

a)	The views, likes, dislikes and comment counts were grouped and aggregated by the category-id that results in 16 different values for the 4 attributes (total categories is 16).
b)	The views, likes, dislikes and comment counts of each video are normalized over its particular category using the values calculated in step ‘a’.
c)	The above results in a value between 0 and 1 for each video’s likes, dislikes, views and comment counts.
d)	A fixed weight is assigned to each of the ratios of views, likes, dislikes and comment counts. After multiplying these weights, the four ratios are summed up to have a value between 0 to 1.
e)	Using min-max normalization technique, ratings are set between 0 and 10. 
f)	Finally, rating column is appended to the dataset.

ii)	Calculating ratings for individual users in dummy dataset:	

a)	Preferences are designated to each user in the following way: The first 25% of all users have preference towards the first four categories (category_id: 1, 2, 10, 15), the next 25% users have a preference to next four categories and so on.
b)	Multiple videos are randomly assigned to each user depending on the set preference. Most of the videos watched by the user are assigned to them from their preferred category but there are few videos assigned from other categories as well to model real world scenario.
c)	Using the above calculated ratings, ratings of individual users are generated randomly.
d)	For instance, if the rating from original dataset, let’s say, Ry:
-	If Ry is under the 1st quartile, a random rating between 0 to 3.0 can be assigned by the user to that video. 
-	If Ry lies in the first quartile to median range, a random rating between 1.0 to 4.0 can be assigned. 
-	If Ry lies in the median to third quartile range, a random rating between 3.0 to 4.0 can be assigned.  
-	If Ry is above 3rd quartile, a random rating between 3.5 to 5.0 can be assigned by the user to that video. 
e)	Finally, a rating column is appended to the dummy dataset which contains rating by each user between 0 to 5 for every video the user has watched.

III.	Performing Collaborative Filtering

The process of collaborative filtering involves getting the dataset where the users have given a rating for the video. The dummy users are assigned videos from a particular category randomly and used the weighted average to assign them the ratings randomly. 
The data frame will contain the users and the ratings , this will be used for the generation of the user-item interaction matrix.

1.	User-Item interaction matrix: 
-    The user-item interaction matrix is created by taking the pivot table of the data frame which would involve in creating a matrix with the columns as the video titles and the rows as the users. 
-	If the users have watched the video, then that index will have the rating value, else it is signified as NaN.
 
2.	Making Recommendations:
- 	For this the user provides in the video title as the input for which the similar videos must be suggested.
-    	The input would then be compared with the user-interaction matrix and a series will be created with the correlation values of all the video titles that can be recommended for the video title that has been given in as the input.
-	The result will be converted into a data frame and after performing basic sorting and grouping of the data present in the series will allow the user to get the result that would be a list of videos and their correlation values.
-    These correlation values will be between 1 and -1 where 1 will be the closest/exact match of the video title for which the input was given and -1 means the video is either poorly rated or is not at all the match for the video given in as input.
-	The result in sorted based on the correlation values as the highly suggested video will have a rating of 1 or closer to that value as compared to a video that has a lower band of correlation value.
 	 

It can be seen from the above results that category of the suggested videos is according to the preference of the users and matches the category subset from which the queried video belongs. For example, the video ‘Half Magic – Official Trailer’ belongs to category-1 (film & animation) and users who watch category-1 also prefer category 2, 10 and 15. As evident from the results, the suggested videos belong to category 1 (film & animation) and 10 (music).
IV.	Outcomes:
The behaviour of Collaborative Filtering using the real world dataset distributions was observed. Some of the observations made were:
-	Effects of low number of user ratings on the recommendation, and a low number of users when compared to the size of the content to be watched. 
- 	Effects of users not having a preference, effects of irrelevant random preferences and ratings. 
The strengths of collaborative filtering were also evident while carrying out this exercise such as for example:
-	Serendipity: The model helps in discovering user interests without the user explicitly specifying them. The model will still correctly recommend video titles on the basis of other users having similar preferences).
- 	A feedback matrix is required to train the model, no need of other contextual features to carry out the recommendation.
The limitations of collaborative filtering that was observed is listed below:
-	Cold-start problem was observed which generally is a limitation of collaborative filtering algorithm.
- 	It is necessary to have a rating feature in the dataset, without this the principle of collaborative filtering would not be possible to implement.
