# Reporting: A Wragle Report on WeRateDogs Tweet Archive
### by Mary Etokwudo

## Gathering Data

The data was gathered from 3 different sources:

1. A file “twitter-archive-enhanced.csv” downloaded from the link in the classroom.
2. A file “image-predictions.tsv” downloaded using the requests library
3. Data gotten from Twitter API (with tweet id from the twitter-archive-enhanced.csv file) using Tweepy library.

Below is a report of the issues found in the data and how they were fixed. To further understand how these steps were taken. See the notebook named [wrangle_act.ipynb](https://github.com/Uniqcoda/dog-rates/blob/main/wrangle_act.ipynb)


## Tidiness issues

1. The tweets data needed were in 3 tables.

* The 3 tables were merged based on the tweet_ids, using the Pandas’ merge method.

2. Dog stages were in 3 different columns

* This was fixed using the Pandas’ melt function to move the dog stage data into one column.


## Quality issues

1. 181 tweets that are retweets. Retweets were not needed for this analysis.

* This was fixed by fishing out those tweets and dropping them. The tweets can be identified by checking for tweets that have retweeted_status information e.g retweeted_status_id

2. The source column contains the device the tweet was sent from but it is contained in a HTML element.

* This was fixed using regular expressions to match the closing and opening tags of the HTML elements in this column and removing them, thereafter using the split function to separate the “Twitter for” from the device name “iPhone”, for instance.

3. Some columns are not necessary.

* This was fixed this by dropping the columns.

4. The timestamp contains date and time. They need to be separated in case we need to compare tweets based on the times and days of the week.

* First, the timestamp was converted from object type to datetime type using Pandas’ to_datetime method. Then separated the date part from the time part, into two columns.

5. Wrong ratings.

* The text column was worked with to get the right ratings. First we used regular expressions to pick out the fraction representing the ratings. Then we split them by the / symbol and converted them to integers. Then we used these values to replace the ratings numerator and denominator.

6. Rating numerator higher than 20 with a denominator in multiples of 10.

* This was fixed while fixing the wrong ratings problem. We reduced the ratings to a fraction with a denominator of 10.

7. rating_numerator with a high value of 1776 and rating_denominator value of 10

* This was fixed by checking these tweets on Twitter where we saw that they weren’t dogs, so we dropped them.

8. Some of the tweets are not for dogs (from the predictions)

* First, all the tweets that were false for all 3 predictions were  dropped. Then, for the remaining tweets, we picked the prediction that is both true and of the highest confidence (if there are other true predictions).
