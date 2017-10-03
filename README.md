## NYC Taxi Ride Duration Prediction
# Purpose
The purpose of this project is to predict the total taxi ride duration based on NYC Taxi and Limousine Commission dataset from [this](https://www.kaggle.com/c/nyc-taxi-trip-duration) kaggle competition.

Click [HTML markdown version](http://htmlpreview.github.io/?https://github.com/shailajakgit/NYC_Taxi/blob/master/Taxi_Ride.html) to look at the analysis.

# Data Description

* id - a unique identifier for each trip
* vendor_id - a code indicating the provider associated with the trip record
* pickup_datetime - date and time when the meter was engaged
* dropoff_datetime - date and time when the meter was disengaged
* passenger_count - the number of passengers in the vehicle (driver entered value)
* pickup_longitude - the longitude where the meter was engaged
* pickup_latitude - the latitude where the meter was engaged
* dropoff_longitude - the longitude where the meter was disengaged
* dropoff_latitude - the latitude where the meter was disengaged
* store_and_fwd_flag - This flag indicates whether the trip record was held in vehicle memory before sending to the vendor because the * * vehicle did not have a connection to the server - Y=store and forward; N=not a store and forward trip
* trip_duration - duration of the trip in seconds

# Steps

1. Feature Engineering:
   - a. Calculated [Haversine distance](https://en.wikipedia.org/wiki/Haversine_formula) from lat-long. But it results in a distance analogous to euclidean distance between two points, which is unrealistic for this use case. An attempt has been made to calculate Manhattan distance from lat-long data.

   - b. Pickup datetime has been used to extract pickup hour and pickup weekday. The new variables account for changing ride durations during rush/lull hours and weekdays/weekends.   

2. Data Cleaning: Data have the following interesting anomalies:

   - a. Non-zero ride time for the same origin and destination location. While this kind of rides may be valid (rider may have rode around the block to get to a destination, or destination may not have clear signs of identification), but such rows may mislead the model and reduce prediction accuracy. Therefore, such rows have been considered outliers.
  
   - b. Unrealistic ride durations (in days) have been considered outliers. 
   
   - c. Linear regression between distance and trip duration has been used to identify outliers further.
  
3. Data subset: The dataset contains 1.4 million rows. Since it is difficult to process such a large volume on a typical laptop computer, data analysis has been performed on a randomly chosen subset of 10000 rows. 

4. Model building: Distribution of the response variable (trip duration) has been estimated by comparing the empirical distribution to various theoritical distributions using ks.test. But trip duration did not fit any long tail distributions well. Therefore, non-paramteric regression techniques - random forest and gradient boosting have been attempted. Gradient boosting has performed the best so far. 10-fold cross validation has been used to estimate out-of-sample error.

# To Do
Get insights from the best model.
