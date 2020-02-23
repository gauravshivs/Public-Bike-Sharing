# Public-Bike-Sharing

Developed By: Gaurav Shivhare 

Date: 21, Feb 2019 

Description: Performing detailed analysis of SF Bay Area Bike Share 

Data Source: https://www.kaggle.com/benhamner/sf-bay-area-bike-share 

Programming Language: Python 3

Tools: Jupyter Notebook, Tableau

Note: Plotly graphs are not visible on Github, please use the personal notebook for real widget experience.

## Business Requirements

Provide the optimized Placement/Reallocation schedule to *Bike Operator*.

## Understanding the Data Provided

1. Station.csv - The list of the Stations with id and geographical location
2. Status.csv - Available Docks and Bikes by Date and Time
3. Trip.csv - Ride details of the users trip
4. Weather.csv - Information about the Weather by Date

### Tableau Link for Deep Data Diving
https://public.tableau.com/profile/gaurav.shivhare#!/vizhome/SFBayAreaDU/BayAreaBikeSharingAnalysis

Please click on above and use presentation mode for a clear view
Sheet 1: Understanding Stations
Sheet 2: Understanding Trips
Sheet 3: Weather Overview

Inferences: 
1. Maximum Docks are available in San Francisco.
2. Cities are quite away from each other, intercity analysis is not useful hence only San Francisco is used.
3. Totals Docks available in San Francisco are 665 while bikes are 435 hence ratio between these is 435/665 =   0.65, which means 65% of all docks have the bike and 35% are free.
4. We have trip data from Aug 2013 to Aug 2015 i.e. 2 years.
5. Minimum trip time is 60 Seconds while Maximum reaches up to more than a year which clearly shows there are outliers which are needed to be eliminated
6. We have multiple constraints for Weather(like humidity, visibility, and temp.) daily stats for all time frames.
Note:  Status file is ignored due to big size and it is clearly visible it has availability data which is very important can be used for real-time bike shifting.


## Approach

1. Business  - Based on Compromised Business (Practical) 

2. Technical - Based on Predicting the number of bikes required on real-time basis.

Note: Reallocation highly depends on the frequency of truck operators can afford and the capacity of the vehicle used.


1. 7 AM - 9 AM are generally peak hours for routing toward the business areas while 4 PM - 6 PM towards the residential area.
So Technically we can identify the busy stations during these peak times, so it can be said the stations have a starting point in the morning hours are residential or connected (Station, Intercity Bus Stop) areas, and similarly, the starting points in evening hours are business areas.
Hence, these stations can be provided with an efficient number of bikes from the non-busy stations at different windows by the operators.
Identifying these areas will help in the reallocation of the bikes for weekdays.

2. On Weekends identify the busy stations and hour for bike allocation.

3. Identify, how weather condition changes the usage pattern, can be used to increase/decrease the number of bikes accordingly.

4. Ride distances and travels can be used to identify the scope of a new station. 

5. Create a model to regress the number of bikes needed at a station at a time by using all constraints.

### EDA

### Trips
After this analysis, we can say Bikes are quite often used on Weekdays.

In Weekdays, 69 and 70 that's stations near train stations are most busy in morning rush hours hence bikes from nearby stations can be placed on these stations, while evening rush is quite distributed but this approach can be used for all stations at all time.

In Weekends, stations 50 and 60 are busy at noontime (14 and 16 hours) as they are close to bridge and bay hence bikes can be allocated at these sites.

It is visible from the above the Temperature has the highest statistical impact on Trips,

Moreover, from the tableau sheet, Exploring SF Trips it was visible that Feb and Dec are the least favored day for bike and also have a minimum temperature of the whole year, similarly, Oct is most favored and have a maximum temperature of the whole year.

Hence it can be said as the operator can be relaxed in minimum temperature months and highly active in maximum temperature months.

## Prediction Model and Evaluation

For the dynamic reallocation, a prediction model can be used to predict a number of start trips, end trips, bikes available and Dock available.
Hence which can be used for reallocation by 

`Bikes_Required = Start_Trip - Bike Available - End Trip)`,

and 

`if Bikes_Required > Docks_Available 
  
  then Allocate_Bikes = Docks_Available - End_Trip 
  
else Bike_Required`

Note : `Bikes_Required = Start Trips on Station` 


### Results looks good after model evaluation but there is still scope. 
### Inference
1. As per the current model, StationId, weekday, Start day, temperature highly important for the prediction of trips.
2. Cross-Validation gives results where SD is very low hence we can say prediction is stable.


Other models can be used for efficiencies like XG Boost or ANN and can be optimized using different approaches.

Model for other metrics can be created using the same approach and finally a way optimized way can be approached to the Operator in real-time.
Moreover, the status file is being skipped but have a good use for prediction for other metrics like dock available and bike available, which is not covered in this work.
