# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
- Description:
    - Investigating relationships between different API data
- Primary Goal
    - Investigate the relationships between the data gathered through the  [CityBikes](https://citybik.es/), [Foursquare](https://developer.foursquare.com/places), and  [Yelp](https://www.yelp.com/developers/documentation/v3/get_started) APIs.
    -   Does the number of bikes at a bike share station, have any effect on nearby points of interest?
- Secondary Goal:
    -   Become more familiar with statistical modelling, and hone my skills with Python and SQL

## Process
### [Part 1: Connecting to CityBikes API](notebooks/city_bikes.ipynb)
-   Chose Chicago as a city, as I have a friend hoping to move there, and I hoped this may offer some insights into the area
    -    Chicago is also a large and dense city, which means more data to work with
-   API was fairly simple to parse through
    -   Pulled latitude, longitude, total bikes, and station id to keep for data
    - Went with total bikes over currently free bikes, as I was interested in looking at the relationship between surrounding businesses, and total availability of bikes
    - Free bikes felt like too inconsistent a variable to use for this specific query
### [Part 2: Connecting to Foursquare and Yelp APIs](notebooks/yelp_foursquare_EDA.ipynb)
-   Chose 4 additional POIs after restaurants
    -   Based these choices off of:
        -   Businesses that are common in cities
        -   Businesses that can be found in various different parts of a city, as opposed to clustered in one area (i.e. theatres and malls tend to be grouped in specific places)
        -   Had to make sure that POI categories overlapped between Foursquare and Yelp
-   Pulled a max of 10 results per location, per category
    -   Both APIs offered an option to add multiple categories to a request, but this felt like it could lead to a skew in data
-   The Yelp data was more intuitive to work with (categories were names instead of id numbers, labels and nesting made sense)   

### [Part 3: Joining Data](notebooks/joining_data.ipynb)
-   After cleaning data separately, I made a join of all three tables, to make it easier to examine the POIs surrounding stations
-   There was some data that didn't overlap between Yelp and Foursquare (such as popularity), or that had to be modified to be consistent (such as ratings)
    -   The join lead to a lot of duplicates
        -   Chose to only delete duplicates that were duplicated on stations
        -   Otherwise, duplicate names were left in
            -   Some POIs were duplicated due to stations being located closely together, so deleting them could erroneously lead to assuming a station had less POIs available than there were
            -   Some POIs were chains, so their names could be repeated in the data multiple times, but at different locations
-   EDA lead to some interesting discoveries:
    -   The majority of POIs that had a price listed, were on the low/cheap end. Despite this, they scored a lower rating on average, compared to their more expensive counterparts
        -   This could be because people tend to be more likely to go out of their way to speak up about bad experiences, than they are about the good
        -   It could also be that more expensive establishments tend to have a higher level of service, so they will tend to rate higher in general
    -   Bike share stations near museums tended to have a higher number of bikes
-   Data cleaning included:
     -  the removal of POIs who had a greater distance than the limit set in our API request
     -  replacing null ratings with the mean
     -  removing remaining null values

### [Part 4: Building a Model](notebooks/model_building.ipynb)
-   It was difficult to find strong correlations in my data. Due to the ordinal nature of price points and ratings, there were few options to choose from
-   Little selection was required, as there were so few variables to choose from
-   Chose to compare price, rating, and distance (POI distance from station), to the number of available bikes at a station

## Results
### API Coverage Quality:
-   Foursquare offered more complete coverage
    -   Yelp data was mostly focused on food establishments, but Foursquare had more results/available data for a greater variety of POIs
### Model Results:
-   The model showed that price, rating, and distance all had a correlation to the total bikes available at a station
-   The number of bikes per station, clearly has a relationship with the price, rating, and distance from, nearby POIs
    -   One hypothesis, is that a station with a large amount of bikes, means that there are more potential customers in the area
        -   With more potential customers, you have more opportunities to build your client base
        -   Since bike stations are a static location, you will frequently have the same people using them
        -   Regular customers tend to spend more money (making it easier for a business owner to raise prices), and be happier with their service
        -   Therefore, if you have a business near a bike share station with a larger amount of total bikes, you could expect to see that reflected in your business

## Challenges 
-   This took a great deal of processing power, and I had to split lots of my process into smaller chunks
-   Modelling is still a relatively new subject to me, so much of this process is still unfamiliar and difficult, just to a lack of practice
-   There was a great deal of data to parse through, and this took the majority of the time
-   There were a lot of null values to either replace or drop

## Future Goals
-   With more time, I would have liked to spend more time cleaning my data, as I felt very rushed and would have liked more time to validate my findings
-   I also would like to have spent more time finding a better model for my data
    -   The one I chose has a decent fit, but I feel like the ordinal nature of so much of my data lead to a lack of normality
