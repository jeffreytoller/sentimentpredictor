# Overview

This project attempts to reproduce the results from a 1991 paper:

[Pessimistic rumination in popular songs and newsmagazines predict economic recession via decreased consumer optimism and spending](https://www.sciencedirect.com/science/article/abs/pii/016748709190029S)

The paper claims to have found a significant relationship between pessimism measured in song lyrics and magazine headlines to changes in economic activity. 

I attempt to recreate this by looking at popular song lyrics for the past 60 years and comparing that to changes in GDP per capita.

# Data

The data used in this project was sourced from:

    API calls to the Genius API through a python library.
    Scraping songlyrics.com
    The Bureau of Economic Analysis

# Preprocessing

Before I begin modeling, I use TextBlob to analyse the lyrics for a sentiment value. These sentiment values are then shifted to create multiple lag values.

# Premodelling/Feature Engineering

The sentiment values and their multiple lags are fed into a LASSO regression model. Alpha (the regularization term) is iteratively increased until only one sentiment lag is remaining. This is the lag that we will use as an exogenous variable in the next modelling step.

# Visualizing

Visualizations that are helpful are the time series values of GDP compared against lyric sentiment. Differenced lyrics sentiment is too noisy to visually inspect for patterns.

Also, the PACF shows very little autocorrelation in the differenced (stationary) GDP values which suggests that the predictive power of a time-series model will be quite limited, unless the exogenous variable is highly correlated.

# Modelling

Using a SARIMAX model, I test the 19-month lagged sentiment of the lyrics as an exogenous variable. A visual inspection of the predictions resulting from this SARIMAX model shows no significant difference from a linear model, as the shape of those predictions does not reflect the shape of the lagged sentiment.

# Conclusions

The results of the paper could not be recreated. In fact, the correlations found do not seem to be powerful enough that any such prediction could be reliably made. Perhaps the addition of more features or a more powerful model could help the predictive power of the model.

# Future Improvements

When time permits, I would like to create a LSTM model capable of making similar predictions. Also, including things that economists expect to affect changes in GDP would also be interesting like monetary/financial policy. This could be especially interesting as our model (which attempts to quantify a perviously exogenous consumer pessimism) could endogenize consumer pessimism thereby helping to separate the effects of government policy from cultural or time-dependent changes.
