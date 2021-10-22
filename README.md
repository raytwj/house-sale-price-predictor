![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)
# Project 2: Ames Housing Sale Price Prediction

### DSI-23: Ray Tan

### Executive Summary

In the real estate market, housing prices are determined by the attributes of the property itself and by the characteristics of the neighbourhood within which it resides. This method of pricing is known as hedonic pricing. It is reasonable to expect internal factors (such as property size) and external factors (such as availability of amenities) to have an impact on housing prices. The attributes and characteristics that are believed to have a considerable impact on housing prices were selected. This project aims to help real estate buyers and sellers predict the price of a house in Ames, Iowa based on a set of property- and neighbourhood-specific traits. The prediction will be made with the best linear regression model as evaluated by R-Squared and Mean Squared Error. Three linear regression models (Ordinary Least Squares, Ridge, Lasso) were evaluated. The entire dataset was first split into a training dataset and a testing dataset. After cross validation on the training dataset, the models had an R-Squared of around 0.78 and a Mean Squared Error of around 1.28 billion. When fitted on the training dataset and testing dataset, the models had an R-Squared of around 0.81 on the training dataset and around 0.83 for the testing dataset, and a Mean Squared Error of around 1.11 billion on the training dataset and around 1.08 billion on the testing dataset. Lasso was selected as the model of choice. After training on the entire dataset, the final R-Squared obtained was 0.82 while the final Mean Squared Error obtained was 1.07 billion. The Lasso model outperformed the Null model, which gave a baseline R-Squared of 0.00 and a baseline Mean Squared Error of 6.28 billion. The 5 biggest determinants of housing sale price were found to be Above Ground Living Area, Kitchen Quality, Location Rank, Total Basement Area, and Garage Area. Interested real estate buyers and sellers may utilise this model to predict the price of a house in Ames, Iowa and make more informed buy-sell decisions. Buyers will be able to know if they are under- or over-paying for a house at the current quoted market price, and sellers will be able know if they are under- or over-asking for their house with reference to how much a house like theirs has sold for historically.

### Problem Statement

This project aims to help real estate buyers and sellers predict the price of a house in Ames, Iowa based on a set of property- and neighbourhood-specific traits and decide if the current quoted market price is above or below what the house would reasonably be bought or sold at. The prediction will be made with the best linear regression model amongst the trio (Ordinary Least Squares, Ridge, Lasso) as evaluated by R-Squared and Mean Squared Error.

### Background & Research

In the real estate market, housing prices are determined by the attributes of the property itself and by the characteristics of the neighbourhood within which it resides. ([*source*](https://www.investopedia.com/terms/h/hedonicpricing.asp)) This method of pricing a marketed good is known as hedonic pricing, wherein an item is treated as the sum of its individual qualities that cannot be sold separately in the market, with the objective of estimating the extent to which each of these qualities affect the market price of the item. ([*source*](https://link.springer.com/referenceworkentry/10.1007%2F978-94-007-0753-5_1279))

Hedonic pricing is used traditionally to estimate economic values for environmental or ecosystem services that directly affect market prices. ([*source*](https://www.ecosystemvaluation.org/hedonic_pricing.htm)) Essentially, the basic premise of the hedonic pricing method is that the price of a marketed good is related to its traits. ([*source*](https://www.ecosystemvaluation.org/hedonic_pricing.htm))

With respect to housing prices, it is reasonable to expect internal factors (such as property size, age, material quality, physical condition, and features like fireplaces or pools) and external factors (such as availability of amenities, proximity to public transportation, crime rate, socioeconomic status of households, and level of air or water pollution) to have an impact on housing prices. ([*source*](https://www.investopedia.com/terms/h/hedonicpricing.asp)) The attributes and characteristics that are believed to have a considerable impact on housing prices were selected as the starting features for the development of the predictive model.

### Data Sources

The following data sources were used:

* [`train.csv`](./data/train.csv): 2006-2010 Ames Housing Dataset For Model Training

This model training dataset contains information on various features and the sale price of houses sold in Ames, Iowa from 2006 to 2010.

* [`test.csv`](./data/test.csv): 2006-2010 Ames Housing Dataset For Model Testing

This model testing dataset contains information on various features of houses sold in Ames, Iowa from 2006 to 2010.

### Data Dictionary

A description of the variables in the original datasets, `train.csv` and `test.csv`, can be found at their source over [here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt).

A description of the variables in the finalised dataset used for analysis in this notebook is given below.

|Variable|Type|Dataset|Description|
|---|---|---|---|
|LotArea|int64|train-final|Lot size in square feet|
|LotFrontage|int64|train-final|Linear feet of street connected to property|
|LotShape|object|train-final|General shape of property|
|LotConfig|object|train-final|Lot configuration|
|LandContour|object|train-final|Flatness of property|
|LandSlope|object|train-final|Slope of property|
|Neighborhood|object|train-final|Physical locations within Ames city limits|
|OverallQual|int64|train-final|Rates the overall quality of the material and finish of the house|
|OverallCond|int64|train-final|Rates the overall condition of the house|
|ExterQual|int64|train-final|Evaluates the quality of the material on the exterior|
|ExterCond|int64|train-final|Evaluates the condition of the material on the exterior|
|BsmtQual|int64|train-final|Evaluates the height of the basement|
|BsmtCond|int64|train-final|Evaluates the general condition of the basement|
|TotalBsmtSF|int64|train-final|Total basement area in square feet|
|HeatingQC|int64|train-final|Heating quality and condition|
|CentralAir|int64|train-final|Central air conditioning|
|GrLivArea|int64|train-final|Above ground living area in square feet|
|KitchenQual|int64|train-final|Kitchen quality|
|Fireplaces|int64|train-final|Number of fireplaces|
|FireplaceQu|int64|train-final|Fireplace quality|
|GarageArea|int64|train-final|Size of garage in square feet|
|GarageQual|int64|train-final|Garage quality|
|GarageCond|int64|train-final|Garage condition|
|WoodDeckSF|int64|train-final|Wood deck area in square feet|
|PoolArea|int64|train-final|Pool area in square feet|
|YrSold|int64|train-final|Year sold in YYYY|
|SalePrice|int64|train-final|Sale price in $|
|AgeSinceRemod/Add|int64|train-final|Age at date of sale since date of remodelling or addition|
|TotRms|int64|train-final|Total number of rooms (comprises basement bathrooms, above ground bathrooms, above ground bedrooms, and above ground kitchens)|
|PorchArea|int64|train-final|Total porch area in square feet (comprises open porch, enclosed porch, three season porch, and screen porch)|
|LocationRank|int64|train-final|Neighborhood location desirability rank|
|OverallQualCond|int64|train-final|Interaction feature obtained from the product of overall quality and overall condition|
|ExterQualCond|int64|train-final|Interaction feature obtained from the product of exterior quality and exterior condition|
|BsmtQualCond|int64|train-final|Interaction feature obtained from the product of basement quality and basement condition|
|GarageQualCond|int64|train-final|Interaction feature obtained from the product of garage quality and garage condition|

### Results & Analysis

A total of three linear regression models (Ordinary Least Squares, Ridge, Lasso) were evaluated to determine the best one for predicting housing prices. The entire dataset was first split into a training dataset and a testing dataset before proceeding to do modelling.

Cross validation was done on the training dataset for all three models. The results obtained were comparable across all three models. All three models had an R-Squared of around 0.78 and a Mean Squared Error of around 1.28 billion.

Further to that, all three models were fitted on the training dataset and testing dataset to obtain their performance scores. Once again, the results obtained were similar across all three models. All three models had an R-Squared of around 0.81 on the training dataset and around 0.83 for the testing dataset. The Mean Squared Error of all three models was around 1.11 billion on the training dataset and around 1.08 billion on the testing dataset.

The Lasso model was selected as the model of choice as it scored the highest R-Squared and the lowest Mean Squared Error on the testing dataset. When the Lasso model was finally trained on the entire dataset, the final R-Squared obtained was 0.82 while the final Mean Squared Error obtained was 1.07 billion. The Lasso model also outperformed the Null model, which gave a baseline R-Squared of 0.00 and a baseline Mean Squared Error of 6.28 billion.

The 5 biggest determinants of housing sale price were found to be Above Ground Living Area, Kitchen Quality, Location Rank, Total Basement Area, and Garage Area. Variables that measure some form of area have emerged as stronger predictors of housing sale price than any other variables like, for instance, those that measure quality or condition. Location Rank, which was engineered from a set of underlying features to serve as a surrogate measure of a neighbourhood's desirability, was one of the most important determinants of housing sale price; this affirms the fact that the neighbourhood plays a significant role in deciding the property's sale price. Despite there being many variables that measure quality, the Kitchen Quality was the only one that stood out as being a major influencer of housing sale price.

### Recommendations & Conclusions

Interested real estate buyers and sellers may utilise this model to predict the price of a house in Ames, Iowa after entering the required set of property- and neighbourhood-specific traits. This will allow firstly, the buyers to know if they will be under- or over-paying for a house at the current quoted market price, and secondly, the sellers to know if they are under- or over-asking for their house with reference to how much a house like theirs has sold for historically. Hence, both buyers and sellers will be able to make more informed buy-sell decisions off of the model.

With regards to the top predictors of housing sale price, some recommendations can be made to real estate buyers and sellers. For buyers who wish to buy their house at a bargain, they should go for houses that are generally small across all facets, situated in an inferior neighbourhood, and have a poor quality kitchen, as these factors will have the largest effect on bringing down the price. For sellers who wish to sell their house at a premium, they could spend some money sprucing up the quality of their kitchen, seeing that houses with a higher quality kitchen tend to command higher prices.

In conclusion, housing sale price prediction based on the principles of hedonic pricing, where both internal and external influencing factors of the item are considered, has proven to be a reliable method. Moving forward, data on more neighbourhood-specific traits (such as proximity to public transportation, crime rate, socioeconomic status of households, and level of air or water pollution), which is lacking in the provided data sources, can be collected to not only elucidate the extent to which these traits affect housing sale price but also to improve the predictive ability of the model.

### References

https://www.investopedia.com/terms/h/hedonicpricing.asp
https://link.springer.com/referenceworkentry/10.1007%2F978-94-007-0753-5_1279
https://www.ecosystemvaluation.org/hedonic_pricing.htm