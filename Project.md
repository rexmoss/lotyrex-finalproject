# What Drives Spotify Streams?
## The Question 
According to Spotify as of March 2021, there are 8 million artists on Spotify and 22 million tracks. However, just 57,000 artists account for 90% of streams. So what does it take to find success on Spotify? How can the rest of these 8 million artists be more like the 57,000? 

Our main question we wanted to answer is - what drives streams on Spotify? 

We wanted to figure out what has the largest influence on the amount of streams an artist has on Spotify. In addition to this,
we wanted to create a model that can predict the number of streams an artist will receive, based on a variety of factors. 
There is obviously a lot that goes into creating successful music and we could not consider every possible factor. For our purposes, 
we decided to focus on four predictor variables and see how much they impacted stream volume. We decided to consider the following variables:
1. Number of tracks on Spotify
2. Years of experience as an artist 
3. Artist type (solo or group)
4. Tracks released per year

Of course there are other factors that undoubtedly influence stream volume, but we felt that these four are very interpretable and relevant to Spotify listeners.
## Collecting the Data
Arguably, the most difficult part of our project was simply finding the data that we wanted.

The team started by looking for datasets available on the internet. However, most of the datasets we found only had a few of the variables we wanted and partial information.
So, it became clear to us that we needed to combine at least a few datasets to get what we wanted. We started with a table from ChartMasters.org 
which had information about artists and the number of Spotify streams they have had throughout their careers.
It did not have all the variables we wanted, but it had a few and there were a few more that we knew we could create from the dataset. 

## Preparing the Data 

With this intial dataset, we read in the table to R programming and then combined that table with two more datasets we found on 
Kaggle which added informationon how long a particular artist has been around. 
By this point, all we needed was a variable that had information on the artist type. We decided to manually add this variable by editing an Excel file. 
The column had two factors: solo and group. 

Finally, once we had the data the way we wanted it in a usable format, we read the csv into python and began to prepare it for analysis. 
First off, there was a decent amount of data cleaning we had to do. Unfortunately, many of the rows had missing values in them. 
After some consideration, we decided to just drop all the rows that had missing values. 
We could not discern any similarities between rows with missing values, and since it appeared to be random, 
we decided it would not skew the data too much if we just got rid of them. There were also a few rows that were unnecessary for our purposes, such as the number of streams 
by an artist. Since that was, essentially what we wanted to predict, there so no sense in keeping that as a explanatory variable.
From there, we had to change the datatypes of some columns so that we could perform calculations on them. We converted several columns to a numeric datatype. 
In addition to changing the datatypes, we also needed to change the names of some of the columns to more accurately represent them.

Once we had made all of the changes we needed to, we added a few features. We converted the experience column from days to years, since we felt that was much more interpretable.
We also divided the number of tracks by the number of years of experience to create a new column that had the number of tracks per year. 
This was a simple way to calculate the frequency with which these artists are putting out new music. We were especially interested to see how this affected the stream volume. 
It seems that some artists put out new music constantly and others do it less often, so we wanted to see what kind of effect this had on the success of their music.

## Explanatory Data Analysis
Our dataset had five explanatory variables. Number of tracks on Spotify (Tracks),	years of experience as an artist (years_experience), 
and number of tracks released per year (tracks_per_year) were numeric variables, and the type of artist they were (artist_type).  
We began with a broad overview of the categorical variable.
Of the 547 artists we analyzed, 371 were solo artists and 176 were groups, or bands.
We also thought it would be interesting to use the means of our numeric variables to see what an “average” artist would look like from our data. 
The "average" artist from our dataset would be 18 years old, release roughly 19 tracks per year, be a solo artist, and have 247 tracks thus far. This "average" 
did not make a lot of sense as an 18 year-old with 247 tracks is not "common." However, it was still fairly interesting. 

We then viewed each numeric variable in the context of artist type.
<p align="center">
  <img src="https://i.ibb.co/8zCSdqf/output1.pngg" />
</p>

This boxplot shows that the mean and range of the total number of tracks by an artist does not drastically differ depending on the artist type. 
However, both types of artists had several outliers, with solo artist having many more. This suggests that there is a lot of variance and that
outliers have heavy sway in this aspect of the data. This could mean that making predictions will be hard.

<p align="center">
  <img src="https://i.ibb.co/TqsvZD3/output2.png" />
</p>

This plot shows that the mean and range of the total number of tracks per year by an artist does differ depending on the artist type. 
"Group" artists tended to have more experience, and solo artists had many more outliers. This could mean that the variable "artist_type" could be a useful explantory variable.

<p align="center">
  <img src="https://i.ibb.co/j5QsF7b/output3.png" />
</p>

This plot shows that the mean and range of the total number of tracks per year by an artist does slightly differ depending on the artist type. 
However, much like the first plot, both types of artists had several outliers, with solo artist having many more. This, again, suggests that there is a lot of variance and that
outliers have heavy sway in this aspect of the data. This could mean that making predictions will be hard.

<p align="center">
  <img src="https://i.ibb.co/hfRkp9Y/output4.png" />
</p>

This correlation plot shows that besides a slight positive linear correlation between "years_experience" and "Tracks," there is not much correlation 
between the numeric variables.

## Building the Model 

We ended up building several models to see which ones would perform the best.  First we tried building a simple linear regression model with all available features.  When we looked at the importance of all the coefficients, we found that the only features that were important in predicting the number of total streams were the features that tracked how many songs had over a certain number of plays(i.e. 10 million+).  After discussing this we decided that these features were not helping us accurately predict how to get the maximum number of streams on spotify.

After removing those features, we were left with the total number of tracks, total years of experience as an artist, the number of tracks per year on average, if the artist is a group or band, and if the artist is a solo artist.  We fit these features into a linear regression, K nearest neighbors, decision tree regressor, and lasso regression models.  We scored each of them with a model score and with an r-squared value, and evaluated which model performed the best.  

The decision tree regressor model performed the best with a score of .59, and an r-squared value of -.92.  We decided to check the importance of each of our features and found that years of experience and average number of new tracks per year were most important in predicting how many streams an artist will accrue on spotify.  

The features that proved to be most important were:
Years of Experience : 0.47
Tracks per Year: 0.32
Total Number of Tracks: 0.18
Artist Type Group: 0.02
Artist Type Solo: .002
All features are scored as probabilities meaning that years of experience handles 47% of the decisions in our model.

## Making Predictions/Explaining Significance

That being said, our model did not predict accurately with the features we have.  When we input “the average artist”  according to our data, our model predicted that they would get 9,594,534,528 streams on spotify.  This is almost 7 billion more than the top artist has in total.  

After evaluating all of the coefficients from all of our other models we found that none of our features were very significant and therefore were not useful in making predictions for other bands.  As mentioned above, we spent a lot of time looking for data, but found that the features that we thought would be significant, were not.  We think that there are othe features out there that could prove to have some significance and ability in predicting how many streams an artist is likely to achieve on spotify.  

In conclusion, our results and our model proved to be insignificant.  Tracking the number of songs, years of experience, and what type of artist they are did not prove statistically significant in our analysis.  We hope to be able to find data better suited for what we hope to predict in the future, and find a way to predict an artist’s potential success throughout their career.



