# Predicting-Stock-Movement-with-Random-Forest
This is one of the project I did as a part of my Internship at Neuraltechsoft Pvt. Ltd. Here, I was asked to create a stock direction prediction model which predicts whether next day stock market will be Bearish or Bullish.

## Introduction

Predicting trends in stock market prices has been an area of interest for researchers for many years due to its complex and dynamic nature. It is a very challenging task due to the many uncertainties involved and many variables that influence the market value in a particular day such as economic conditions, investors’ sentiments towards a particular company, political events etc. Because of this, stock markets are susceptible to quick changes, causing random fluctuations in the stock price. Stock market series are generally dynamic, non-parametric, chaotic and noisy in nature and hence, stock market price movement is considered to be a random process with fluctuations which are more pronounced for short time windows. Market risk, strongly correlated with forecasting errors needs to be minimized to ensure minimal risk in investment. Thus, I tried to minimize forecasting error by treating the forecasting problem as a classification problem, a popular suite of algorithms in Machine learning.

## Data and Pre-processing
I was provided with the daily stock data of 2 years for companies like JP Morgan, IBM, Home Depot, ARWR and Costo Wholesale Corporation. There were 2520 rows with  Company symbol, datetime, close, high, low and volume columns.

I did some pre-processing on data. Since there were multiple ticker symbols, I sorted my data first by Symbol column and then by date time column. After sorting the data, I calculated the change in price from one period to next. Once, I calculated the change in price, I checked for the ticker symbols. Technically, each row where the ticker symbol changes are incorrect because it's using the price from a different ticker. That means we need to have the first row of each ticker symbol to be null for the change in price column. for doing this, I first called the symbol column then I did a comparison using shift method and changed those columns to null. Since there were five companies there were 5 null symbols out of total.

## Indicator Calculations
Now that my data is ready I calculated the momentum indicators. This are technical analysis indicators a trader can use to determine the strength or weakness of the price movement. I calculated 6 different types of momentum indicators.

The first indicator was RSI. I grouped the columns by symbols and performed transformation with a lambda function on change in price column.  The RSI ranges from 0 to 100. RSI determines whether the stock is overbought if it is above 70 and oversold if below 30.

![image](https://user-images.githubusercontent.com/70087327/132368817-15b51049-0f51-4c29-ae94-8dd3f59bee69.png)



Next I calculated the stochastic oscillators by applying rolling function on change in price column. Here I compared the most recent closing price to the highest and lowest prices for 14 days respectively.

![image](https://user-images.githubusercontent.com/70087327/132368855-57fa21fc-8e43-43d5-ad08-8080799f5aa5.png)



After that I calculated the moving average convergence divergence by subtracting the 26-period exponential moving average (EMA) from the 12-period EMA. After the calculations, I found that when MACD was below the signal line it indicated sell signal and above indicated the buy signal. 

![image](https://user-images.githubusercontent.com/70087327/132368951-fc7f483c-36d6-4998-9946-320c6d40f9ec.png)



Next, I calculated price rate of change by measuring the most recent change in price with respect to price in 9 days. 

![image](https://user-images.githubusercontent.com/70087327/132368992-761b3f69-c60f-4e4a-8d67-b76b8bd10dc6.png)



Later on I calculated the on balance volume by calculating the difference for the closing price and used a for loop for looping through each row in the volume column. If the change in price was greater than 0 then I added the volume, and if it was less than 0 then I subtracted the volume.

![image](https://user-images.githubusercontent.com/70087327/132369029-daf9be54-8cc0-45d3-bf17-c4dd70c8c47e.png)


Finally, I calculated my last indicator that is Williams percent range same way I calculated the stochastic oscillator. It ranges from -100 to 0. When the value was above -20 it indicated a sell signal and when it was below -80, it indicated a buy signal.

![image](https://user-images.githubusercontent.com/70087327/132369103-58380127-b7a3-4f91-8c66-a765df58c66a.png)


## Model Building

After calculating the indicators, I created the prediction column. The close column included the prices I needed to determine if the stock is closed up or down for any given day. So, I selected the close column and grouped my data by each symbol. I used lambda function and compared the current prices with the previous prices. So, in this way I created the output column which resulted as '1' if today’s closing price is greater than yesterday’s closing price then '-1' for down days and '0' for no change. 

![image](https://user-images.githubusercontent.com/70087327/132369559-4edb7588-b115-4ccc-b08e-817db15f7230.png)


## Train test split

I splitted data into train and test set in ratio of 80:20. For that the indicator columns served as x and prediction column served as y. After splitting the data I created Random forest classifier model and fitted the training data to model using fit method. Finally with the trained model we can make predictions.

## Accuracy and classification report

After building the model, I checked for the accuracy  and it came out as 70.85%. To get a more detailed overview of how the model performed, I built a classification report and computed F1 score, precision, recall and the support.

![image](https://user-images.githubusercontent.com/70087327/132369590-3121ae08-7862-49cc-a280-73d62645b709.png)

## Confusion matrix
To check the performance of model I plotted the confusion matrix. 

![image](https://user-images.githubusercontent.com/70087327/132369733-c2b7ba51-10fc-4c37-8c1e-0a163eed65a2.png)

## Model Evaluation

Further I calculated out of bag error which measures the prediction error of random forest models utilizing bagging. Then To identify the most important features, I calculated feature importance with Gini based importance and also I graph them as it is easier to visualize the results. Here we can clearly see that the most important feature is stochastic oscillator and least important feature is on balance volume.

![image](https://user-images.githubusercontent.com/70087327/132370019-886ee726-0971-4388-927f-62799ae2ebfc.png)


## Model Improvement
Then I did the model improvement. For this, I did Hyperparameter Tuning which is nothing but searching for the right set of hyperparameter to achieve high precision and accuracy. I used Random Search technique where random combinations of hyperparameters were used to find the best solution for built model.

![image](https://user-images.githubusercontent.com/70087327/132370200-88134522-bf7f-49d2-990f-96531a744ae0.png)


Then, I calculated accuracy for the improved model which came out as approx 74.43% and built the classification report for the same.

![image](https://user-images.githubusercontent.com/70087327/132370241-136ab674-3897-4a1f-b79a-26b59f7a67bd.png)

## ROC

Then I calculated the ROC Curve which shows us the performance of our model at all possible thresholds. When the curve comes closer to the left-hand border and the top border of the ROC space, it indicates that the test is accurate. The closer the curve to the top and left-hand border, the more accurate is the test.

![image](https://user-images.githubusercontent.com/70087327/132370310-fc846711-bc7b-4cc8-9044-853452d37d83.png)

## Conclusion

Created a stocks direction prediction model which will predict whether next day stock market  will be bearish or bullish.
