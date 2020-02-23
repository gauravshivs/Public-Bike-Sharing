# Pubic-Bike-Sharing

Developed By : Gaurav Shivhare Date : 21, Feb 2019 

Description : Performing detailed analysis on SF Bay Area Bike Share 
l
Data Source : https://www.kaggle.com/benhamner/sf-bay-area-bike-share 

Programming Language : Python 3

Tools : Jupyter Notebook, Tableau

Note : Plotly graphs are not visible on Github, please use personal notebook for real widget experience.

## Business Requirements

Provide the optimized Placement/Reallocation schedule to *Bike Operator*.

## Understanding the Data Provided

1. Station.csv - The list of the Stations with id and geographical location
2. Status.csv - Available Docks and Bikes by Date and Time
3. Trip.csv - Ride details of the users trip
4. Weather.csv - Information about the Weather by Date

### Tableau Link for Deep Data Diving
https://public.tableau.com/profile/gaurav.shivhare#!/vizhome/SFBayAreaDU/BayAreaBikeSharingAnalysis

Please click on above and use presentation mode for clear view
Sheet 1 : Understanding Stations
Sheet 2 : Understanding Trips
Sheet 3 : Weather Overview

Inferences: 
1. Maximum Docks are available in San Francisco.
2. Cities are quite away from each other, intercity analysis is not useful hence only San Francisco is used.
3. Totals Docks available in San Fracisco are 665 while bikes are 435 hence ratio between these is 435/665 =   0.65, which means 65% of all docks have bike and 35% are free.
4. We have trip data from Aug 2013 to Aug 2015 i.e. 2 years.
5. Minimum trip time is 60 Seconds while Maximum reaches upto more than a year which clearly shows there are outliers which are needed to be eliminated
6. We have multiple constraints for Weather(like humidity, visibility and temp.) daily stats for all time frame.
Note :  Status file is ignored due to big size and it is clearly visible it have availability data which is very important can be used for real time bike shifting.


## Approach

Can be two
1. Technical --- Based on Predicting the number of bikes required on real time basis.
2. Business  --- Based on Compromised Business (Practical)

Note: Reallocation highly depends on the frequency of truck operator can afford and capacity of the vehicle used.


Approach
1. 7 AM - 9 AM are generally peak hours for enrouting toward the business areas while 4 PM - 6PM towards the residential area.
So Technically we can identify the busy stations during these peak times, so it can be said the stations have starting point in morning hours are residential or connected (Station, Intercity Bus Stop) areas, and similarly the starting points in evening hours are business areas.
Hence, these stations can be provided with the efficient number of bikes from the non-busy stations at difference windows by the operators.
Identifying these areas will help in reallocation of the bikes for weekdays.

2. On Weekends identify the busy stations and hour for bike allocation.

3. Identify, how weather condition change the usage pattern, can be used to increase/decrease number of bikes accordingly.

4. Ride distances and travels can be used to identify the scope of a new station. 

5. Create a model to regress the number of bikes needed at a station at a time by using all constraints.

### EDA

### Trips
After this analysis we can say Bikes are quites oftenly used in Weekdays.

In Weekdays, 69 and 70 thats stations near train stations are most busy in morning rush hours hence bikes from near by stations can be placed on these stations, while evening rush is quite distributed but this approach can be used for all stations at all time.

In Weekends, stations 50 and 60 are busy at noon time (14 and 16 hours) as they are close to bridge and bay hence bikes can be allocated at these sites.

It is visible from the above the Temprature have the highest statistical impact on Trips,

Moreover, from the tableau sheet, Exploring SF Trips it was visible that the Feb and Dec are the least favoured day for bike and also have minimal temprature of whole year, similary Oct is most favoured and have maximum temprature of whole year.

Hence it can be said as the operator can be relaxed in minimum temprature months and highly active in maximum temprature months.

## Prediction Model and Evaluation

For the dynamic reallocation, a prediction model can be used to predict number of start trip, end trip, bikes available and Dock available.
Hence which can be used for reallocation by `Bikes_Required = Start_Trip - Bike Available - End Trip)` ,
and `if Bikes_Required > Docks_Available then Allocate_Bikes = Docks_Available - End_Trip else Bike_Required`

Note : `Bikes_Required = Start Trips on Station` 


### Results looks good after model evaluation but their are still scope. 
### Inference
1. As per current model, StationId, week day, Start day, temprature highly important for prediction of trips.
2. Cross Validation gives results where SD is very low hence we can said prediction is stable.


Other models can be used for efficiency like XG Boost or ANN, and can be optimized using different approaches.

Model for other metrices can be created using same approach and finally a way optimized way can be approached to the Operator on real time.
Moreover, status file is being skipped but have good use for prediction for other metrices like dock available and bike available, which is not covered in this work.
