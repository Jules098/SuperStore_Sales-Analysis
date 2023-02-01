# SuperStore_Sales-Analysis

I am performing a number of analyses on this superstore's sales data. The data can be found on https://public.tableau.com/app/resources/sample-data.

## Time Series Analysis

The sales data in this superstore can be viewed through the lens of a timeseries analysis. The first plot is the sales data with it's 3 day moving average.

<img width="572" alt="3_dat_MVA_Sales" src="https://user-images.githubusercontent.com/110205528/215886411-5a16ce31-4030-4f10-9440-8d4879d5302f.png">

The sales data was then tested for stationarity using the Adfuller() function available in python. Using an alpha value of 0.05, it was determined that the sales data was stationary.

The following is a decomposition of the sales data:

<img width="623" alt="Sales Data Decomposition" src="https://user-images.githubusercontent.com/110205528/216084832-b12cd7ad-c8db-4058-8b66-99ae5795435b.png">

As we can see, there is no indication of seasonal trends in the sales data as organized as daily sales.

The picture becomes slightly clearer when grouping the sales data by month, as opposed to by day.

<img width="579" alt="Monthly Sales, Cost and Profit Data" src="https://user-images.githubusercontent.com/110205528/216085711-72b480e9-e71d-4e6d-ba85-cbbb3b436931.png">

<img width="576" alt="3 Day Monthly MVA Sales" src="https://user-images.githubusercontent.com/110205528/216086441-126a777e-3223-4235-9daa-26c52cec884b.png">

The 3 day moving average becomes clearer to visualize when the data is organized monthly. While there seems to be upticks in the sales data later in the year, still the seasonal_decompose() function shows no sign of seasonality. I evaluated both the model both under the additive and multiplicative framework, though for the sales organized by month multiplicative was the better option as we can see a clear upward trend in the data as we move a long the time axis.

## Forecasting with Timeseries Data

Using an autocorrelation plot, we can see the linear relationship between the sales data and it's lagged values.

<img width="587" alt="Autocorrelation Plot" src="https://user-images.githubusercontent.com/110205528/216088291-bb48a8cd-2d0e-4dc5-a590-36e59c22776c.png">

We can see a spike in the relationship around the 11th lag, with a descending spike every 11 to 12 lags after that, which would indicate an end of year spike in sales, that we can see in the monthly sales data.

Using an ARIMA model, I fit the model using the best parameters given the data after performing a gridsearch on a baseline ARIMA model.  After fitting the model and using the best parameters, the model achieved a RMSE of 23,456.86 on the last 16 months of projected sales data.

<img width="577" alt="ARIMA Projected Sales Data" src="https://user-images.githubusercontent.com/110205528/216090490-1ff79efa-6f9b-4c4e-b97a-87bf81a3e0a6.png">

## Using Keras and Deeplearning to improve the projected output

To create the Keras model, the original sales data needed to be organized by month and the optimal number of lags needed to be included in the dataframe. After fitting the model with 11 lags, the model had an adjusted R squared of 0.704. The data was scaled using the MinMax scaler available in sci-kit learn. Once the data was scaled and the training data was split from the testing data, I built the Keras Sequential Model and trained it on 100 epochs.

The model achieved a MSE of 0.0062.

I inversed the scaled training data and the scaled predicted output to be able to plot the last 6 months of predicted sales output for visualization purposes.

<img width="579" alt="Keras Deeplearning Predicted Output" src="https://user-images.githubusercontent.com/110205528/216094233-3c3b3a25-74a4-49f6-8c73-2402e41949f6.png">

We can see that the Keras deeplearning model was substantially better at predicting sales output than the originally tested ARIMA model.
