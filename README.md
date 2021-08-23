# Predicting-house-prices-using-Deep-Learning

![0_XMbwmj-4r80bBuIg (1)](https://user-images.githubusercontent.com/65482013/120058965-ffc3fc80-c06b-11eb-87fe-34ebb175f039.jpg)

In this project, we start with EDA of the pricing data of houses in King County, WA to train and implement Deep Learning with an Artificial Neural Network, in order to predict price of a new property on the market.

## The Data

We will be using data from a Kaggle data set:

https://www.kaggle.com/harlfoxem/housesalesprediction

## Independent Variables (Raw)
    
* id - Unique ID for each home sold
* date - Date of the home sale
* price - Price of each home sold
* bedrooms - Number of bedrooms
* bathrooms - Number of bathrooms, where .5 accounts for a room with a toilet but no shower
* sqft_living - Square footage of the apartments interior living space
* sqft_lot - Square footage of the land space
* floors - Number of floors
* waterfront - A dummy variable for whether the apartment was overlooking the waterfront or not
* view - An index from 0 to 4 of how good the view of the property was
* condition - An index from 1 to 5 on the condition of the apartment,
* grade - An index from 1 to 13, where 1-3 falls short of building construction and design, 7 has an average level of construction and design, and 11-13 have a high quality level of construction and design.
* sqft_above - The square footage of the interior housing space that is above ground level
* sqft_basement - The square footage of the interior housing space that is below ground level
* yr_built - The year the house was initially built
* yr_renovated - The year of the house’s last renovation
* zipcode - What zipcode area the house is in
* lat - Lattitude
* long - Longitude
* sqft_living15 - The square footage of interior housing living space for the nearest 15 neighbors
* sqft_lot15 - The square footage of the land lots of the nearest 15 neighbors

## EDA Insights & Takeouts

1. Most houses cost $1-2M, which makes sense. The problem with the data is that there are a small number of high value outliers which could skew the ML model
2. There is a slight increase in the mean price of the houses with the no. of bedrooms but this is in not a strong correlation
3. The house prices increase almost linearly with the area of the house
4. Houses in certain latitude and longitude are costlier. Places around Long -122.2 and Lat 47.65 seem like a place for costly houses, ranging from $4-8M
5. Waterfront properties are costlier by around $0.8M
6. Properties lying in northern King County are significantly costlier than those in south and east. Houses in the North and West King County are costlier by at least 35% that those in East and South.
7. Almost all houses in the South are low priced.
8. Properties around Lake Washington, viz. Mercer Island, Bellevue, Medina, Kirkland, are the costliest.
9. Properties around urban areas, like Madison Park, University District and Eastgate are on the higher side.
10. People on the mainland prefer/have higher number of bedrooms in their houses.
11. House prices don't change much wrt the month but inflation has caused the prices to get linearly hiked year on year.

## Geographical mapping

To obtain insights about the change in house prices with respect to the zone, direction, latitude, longitude, affinity to the coastline and zip codes, I decided to plot the pricing and other data variation on a live map via python.

I did this using the folium library and the Map function. We first try to fit the lat/long graph to s Google photo of King County but this map does not look good enough to make deep guesses.

To look at the zip code or sector wise ditribution of prices, we plot a choropleth map for King County using a ![GeoJSON file](https://github.com/pranavtumkur/Predicting-house-prices-using-Deep-Learning/blob/main/us-zip-code-latitude-and-longitude.geojson).

INFERENCE - Zip codes by themselves do not matter, but the cluster of zip codes shows a definite trend, i.e. the zone in which these zip codes lie, viz. North, East, West, South, Central, plays a huge role upon the prices of the houses. We create a new parameter called 'Zone' and populate it with N,E,W,S using the latitude and longitude of the zip code.

## Builidng a ML model

#### Database preparation-

1. Decide record's zone (N,E,W,S) as per lat and long
2. Convert the categorical variable of zone, using get_dummies
3. Drop the zip code column to not mislead the ML model
4. Make 2 columns for month and year and drop the date column
5. Do the train, test split
 
#### Creating a Deep Learning ANN Model

1. Add 4 hidden layers with 19 neurons each, using relu as the activation function
2. Use optimizer='adam' and loss='mse'
3. Train the model with batch_size=128 for 600 epochs
4. Record the MSE losses in a new df
5. Plot the loss and val_loss to check if model is not overfitting the data

## Evaluating the model

1. Calculate the MEA and RMSE (RMSE ~ $129k, which is okay for average house costs of $5.4M)
2. Calculate explained variance score (Explained Variance Score = 0.87 implies a very strong corelation between our predicted and true values)
3. Plot a histogram of the 'predicted values' v/s the 'true values' of house prices (both almost coincide, which is good)
4. Plot a scatter plot of 'predicted values' to compare to the straight line of true values to check bias and variance (Error is high only for extremely high valued properties. Otherwise bias and variance are low)
5. Lastly, let's plot a histogram of the errors (Errors are symmetrically distrubuted across 0. This proves that we have made a good ANN model!)



