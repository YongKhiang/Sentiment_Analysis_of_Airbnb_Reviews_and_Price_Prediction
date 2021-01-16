# Capstone Project: Sentiment Analysis on Airbnb Reviews and Price Prediction

## Executive Summary

### Problem Statement

Airbnb is an online marketplace which allows people to rent out their properties or rooms to guests. Since Airbnb launched over 10 years ago, it has disrupted the travel and hospitality industry, with increasing trend of bookings for travellers' stay over the years, in preference to conventional hotels. The platform has thus become an income generation tool for house owners.

Some challenges that Airbnb hosts face include understanding the determinants for customer satisfaction and determining the optimal nightly price to list on the website. Although Airbnb does provide some general guidance, there are currently no free services which help hosts to determine the best price to rent out their space. One method could be to find a few listings that are similar to their place, and set the listing price based on the calculated average price. This method could be tedious given that the market is dynamic and hosts could also miss out on the competitive advantages that their listings has over similar listings such as how near it is to the MRT station, and other extra amenity facilities.  

Through this project, I intend to bridge these gaps by identifying the factors that contribute to a good review score rating and high room price to provide insights to hosts on the Airbnb Singapore platform to improve their earnings. Specifically, sentiment analysis and topic modelling using Natural Languange Processing techniques on the traveller reviews will be conducted to gain information from customer feedback, and a price prediction model using supervised machine learning regression algorithms will be developed to help home owners to set a price based on the listing characteristics. The performance of the regression models will be assessed using R^2 and Root Mean Square Error (RMSE) metrics.

### Data Set

The Airbnb reviews and listing datasets were sourced from the 'Inside Airbnb" website (http://insideairbnb.com/get-the-data.html), an investigatory/watchdog website which scrapes and reports scraped data on Airbnb website for multiple cities around the world, focusing on highlighting illegal renting on the site. 

The Airbnb Singapore dataset was scraped on 22 June 2020 and contains information on all Singapore listings that were live on the site on that date. A GeoJSON file of Singapore neighbourhoods was also downloaded from the same site.

The dataset comprises of two main tables:

- `Reviews`: Detailed reviews given by the guests with 6 attributes (columns).
- `Listings`: Detailed listings data showing 106 attributes for each of the listings. The data dictionary is shown below after dropping a bunch of columns that are not relevant for the problem. 

|Feature|Type|Dataset|Description|
|:---|:---|:---|:---|
|listing_id            |int      |reviews |listing id
|id           |int      |reviews |review id
|date      |datetime |reviews| date of review
|reviewer_id         |int      |reviews| reviewer id
|reviewer_name       |str      |reviews| reviewer name
|comments        |str      |reviews| review comments left by the guests
|id            |int      |detailed_listings |listing id
|summary           |str      |detailed_listings |summary of listing characteristics
|description      |str |detailed_listings| detailed description of listing characteristics
|transit         |str      |detailed_listings| description of transport accessibility
|host_id        |int      |detailed_listings| host id
|host_since        |datetime      |detailed_listings| date that the host first joined Airbnb
|host_response_time          |str    |detailed_listings| average amount of time the host takes to reply to guest inquiries and booking requests
|host_response_rate          |str    |detailed_listings|  proportion of guest inquiries and booking requests that the host replies to
|host_acceptance_rate|str|detailed_listings| proportion of guest reservations that the host accepts
|host_is_superhost           |str|detailed_listings| whether or not the host is a superhost, which is a mark of quality for the top-rated and most experienced hosts who provide extraordinary experiences for their guests.
|host_identity_verified            |str|detailed_listings| whether or not the host has been verified with their identity
|neighbourhood_cleansed      |str|detailed_listings| the Singapore neighbourhood town the property is located
|neighbourhood_group_cleansed        |str      |detailed_listings| the broad neighbourhood region the property is located
|latitude           |float      |detailed_listings| latitude coordinate
|longitude |float |detailed_listings| location longitude coordinate
|property_type         |str      |detailed_listings| type of property, e.g. apartment or hotel
|room_type|str|detailed_listings| type of room, e.g. private or shared room
|accommodates        |int|detailed_listings| number of people the property can accommodate
|bathrooms  |int|detailed_listings| number of bathrooms
|bedrooms     |int|detailed_listings| number of bedrooms
|beds     |int|detailed_listings| number of beds
|bed_type     |str|detailed_listings| type of bed, e.g. real bed or futon
|price  |str|detailed_listings| nightly advertised price (target variable)
|security_deposit     |str|detailed_listings| amount of security deposit required
|cleaning_fee    |str|detailed_listings| amount of cleaning fee (fixed amount paid per booking)
|guests_included  |int|detailed_listings| number of guests included in the booking fee
|extra_people     |str|detailed_listings| price per additional guest above the guests_included price
|minimum_nights  |int|detailed_listings| minimum length of stay
|maximum_nights     |int|detailed_listings| maximum length of stay
|has_availability     |str|detailed_listings| whether there is availability
|availability_30     |int|detailed_listings| number of nights available to be booked in the next 30 days 
|availability_60     |int|detailed_listings| number of nights available to be booked in the next 60 days 
|availability_90  |int|detailed_listings| number of nights available to be booked in the next 90 days 
|availability_365     |int|detailed_listings| number of nights available to be booked in the next 360 days 
|number_of_reviews     |int|detailed_listings| number of reviews left by guests
|number_of_reviews_ltm     |int|detailed_listings| number of reviews left by guests in the last 12 months
|first_review    |datetime|detailed_listings| date of first review
|last_review  |datetime|detailed_listings| date of last review
|review_scores_rating     |float|detailed_listings| average overall review rating scores 
|review_scores_accuracy    |float|detailed_listings| average rating scores for listing description accuracy 
|review_scores_cleanliness     |float|detailed_listings| average rating scores for property cleanliness  
|review_scores_checkin     |float|detailed_listings| average rating scores for guests' check-in process  
|review_scores_communication    |float|detailed_listings| average rating scores for host's communication 
|review_scores_location  |float|detailed_listings| average rating scores for listing location 
|review_scores_value    |float|detailed_listings| average rating scores for value-for-money consideration
|instant_bookable     |str|detailed_listings| whether or not the property can be instant booked, without having to message the host first and wait to be accepted
|is_business_travel_ready    |str|detailed_listings| whether or not the property has facilities to cater to business travellers e.g. Wi-fi, designated workspace, iron
|cancellation_policy  |str|detailed_listings| type of cancellation policy, e.g. strict or flexible
|calculated_host_listings_count     |int|detailed_listings| number of listings the host has on Airbnb
|reviews_per_month    |float|detailed_listings| average number of reviews left by guests each month

### Findings

A majority (about 97%) of the guests had positive reviews on the Airbnb experience based on the sentiment analysis scores.

The topics extracted from the Latent Dirichlet Allocation (LDA) algorithm provided an insight into the topics of discussion in the reviews of Airbnb guests. It is observed that topics can be broadly classified into the categories of (i) overall experience of stay, (ii) the amenities provided by the accommodation, (iii) noise level within the property/ neighbourhood, (iv) location of the unit and proximity to points of interest and (v) communication and friendliness of the hosts.

Hence, Airbnb hosts and potential home owners who plan to use the Airbnb platform to rent out their homes could utilise the insight from this analysis to better target their listings to potential customers or to strategise how to best serve their customers by providing better service quality e.g. personable interaction, homely feeling etc, which are important to guest satisfaction so as to boost the host's online reputation and get ahead of competitors. 

For the price prediction model, seven machine learning regression algorithms were evaluated in their predictive performance on Airbnb listing price in Singapore. The XGBoost regression model had the best predictive performance and was able to explain 81% of the variation in listing prices, with a RMSE of \\$41.38 on the test data, with negligible drop in model performance using a reduced set of 26 features for the production model.

Based on the exploratory analysis, the listing prices are generally higher for renting out serviced apartment/ hotel rooms or entire homes which can accommodate more guests. Likewise, accommodations with amenities such as TV, gym, pool and hot tub, and located close to the city at the central area also tend to fetch a higher listing price.   

Potential directions for future work to further improve the prediction model include the following:

- Use more accurate price data based on actual price paid by the guests. Such data is available for a fee in some websites such as AirDNA (https://www.airdna.co/).
- Incorporate the listing photos' image quality as a feature to the prediction model, using convolutional neural network techniques.
- Include other proximity features such as accessibility to restaurants and supermarkets in the listing neighbourhood.
- Add in more granular features specific to the accommodation unit such as the unit level and and whether there are good views from the unit.

### Folder Organisation

The repository is organised into 4 folders. The `code` folder contains the notebook files for the project. The `data` folder contains the raw data files downloaded from source website, and processed data files from EDA for modelling. The `assets` folder contains images and MRT station shape file resources to support the EDA. Lastly, the `html` folder contains the html output files from EDA and topic modelling for interactive visualisation.   

### Contents:
#### Sentiment Analysis and Topic Modelling on Reviews 
- [Data Cleaning (Reviews)](./code/01_Sentiment_Analysis_and_Topic_Modelling_on_Airbnb_Reviews.ipynb#Data-Cleaning-(Reviews))
- [Sentiment Analysis](./code/01_Sentiment_Analysis_and_Topic_Modelling_on_Airbnb_Reviews.ipynb#Sentiment-Analysis)
- [Topic Modelling](./code/01_Sentiment_Analysis_and_Topic_Modelling_on_Airbnb_Reviews.ipynb#Topic-Modelling)<br>
    - [Positive Reviews](./code/01_Sentiment_Analysis_and_Topic_Modelling_on_Airbnb_Reviews.ipynb#Positive-Reviews)<br>
    - [Negative Reviews](./code/01_Sentiment_Analysis_and_Topic_Modelling_on_Airbnb_Reviews.ipynb#Negative-Reviews)
- [Summary](./code/01_Sentiment_Analysis_and_Topic_Modelling_on_Airbnb_Reviews.ipynb#Summary)

#### Price Prediction Model
- [Data Cleaning (Listings)](./code/02_Airbnb_Listings_Data_Cleaning_and_EDA.ipynb#Data-Cleaning-(Listings))
- [Exploratory Data Analysis](./code/02_Airbnb_Listings_Data_Cleaning_and_EDA.ipynb#Exploratory-Data-Analysis)
- [Feature Engineering](./code/02_Airbnb_Listings_Data_Cleaning_and_EDA.ipynb#Feature-Engineering)
- [Feature Selection](./code/02_Airbnb_Listings_Data_Cleaning_and_EDA.ipynb#Feature-Selection)
- [EDA Summary](./code/02_Airbnb_Listings_Data_Cleaning_and_EDA.ipynb#EDA-Summary)
- [Pre-processing](./code/03_Price_Prediction_Modelling_Insights.ipynb#Pre-processing)
- [Modelling](./code/03_Price_Prediction_Modelling_Insights.ipynb#Modelling)
- [Model Evaluation](./code/03_Price_Prediction_Modelling_Insights.ipynb#Model-Evaluation)
- [Production Model](./code/03_Price_Prediction_Modelling_Insights.ipynb#Production-Model)
- [Conclusion and Recommendations](./code/03_Price_Prediction_Modelling_Insights.ipynb#Conclusion-and-Recommendations)

### External Libraries/ Tools

`nltk`, `spacy`, `gensim`, `mallet`, `pyLDAvis`, `plotly`, `pyshp`, `xgboost`
