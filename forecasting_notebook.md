## IBM stock price forecast 


Libraries
```{r  Libraries  }
library(readr)
library(forecast)
```
In this exercise, I analyzed the IBM stock adjusted close price in the period of [2017-01-01/2021-12-10] to create a time series forecasting models (naïve  , Seasonal naïve, ETS,ARIMA, Neural Network), then I checked which model returned the best forecasting results the test period of the last 7 month of the data period. Data from is **<a href="https://finance.yahoo.com/quote/IBM/history/" target="_blank">Yahoo Finance</a>** .


## Loading the Data
 
```{r loaud the data}
IBM <- read_csv("C:/Users/baaba/Desktop/MS. Applied Economic and Data analysis/ADEC7460.02 Fall 2021 Predictive AnalyticsForecasting [Fulton]/Module 6 VAR, Panel, HTS, Machine Learning 25 Nov -5 Dec/Assignment Homework Time Series/IBM.csv")

head(IBM)
summary(IBM)
```
plotting the data 
```{r  }
IBM.ts <- ts(IBM$`Adj Close`
             , start = c(2016,01)
             , end = c(2021,12)
             , frequency = 12)

autoplot(IBM.ts)
```

## Traingin Testing split

After loading data from Yahoo finance I split the data set into a training set from 2016,1 to 2021 and test set from 2021,6 to the end of data period.
```{r Split Data into Train and Test Sets}
# training sets   
train = window(IBM.ts, end = c(2021,5))
# testing sets
test = window(IBM.ts, start = c(2021,6)) # 7 months test set
```

## Naïve Model

I generated a naive model and I checked the performance for the model forecasting on the test set. The naive model generated a MASE = 1.2720367 and a RMSE = 17.736403. Next we need to compare it with the other models.
```{r    naïve    }
naive.fit = naive(train)
naive.forecast = forecast(naive.fit, h = 7)
summary(naive.fit)
```

```{r        }
checkresiduals(naive.forecast)
accuracy(naive.forecast, test)
```
```{r}
autoplot(IBM.ts) +
  autolayer(naive.forecast, series = "naive Forecast") +
  autolayer(test , series = "Actual price")
```

## Seasonal naïve Model

Now I generated a Seasonal naive model and I checked the performance for the model forecasting on the test set. The Seasonal naive model generated a MASE = 1.009509 and a RMSE = 14.93724.  the Seasonal naive model has worst RMSE and MASE than the naive model in this data,  we will compare these models  to check how good the next models are.  
```{r           }
snaive.fit = snaive(train)
snaive.forecast = forecast(snaive.fit, h = 7)
summary(snaive.fit)
```
```{r           }
checkresiduals(snaive.forecast)
accuracy(snaive.forecast, test)
```

```{r}
autoplot(IBM.ts) +
  autolayer(snaive.forecast, series = "snaive Forecast") +
  autolayer(test , series = "Actual price" )
```

## ETS Model

In the subsequent model iteration, I generated a Error, Trend, Seasonal model and I checked the performance for the model forecasting on the test set. The ETS Model generated a MASE = 0.7658349 and a RMSE = 10.320349. Although the Seasonal naive model looks much better in the graph but the ETS model has better MASE and  RMSE than the Seasonal naive model.
```{r    ETS    }
ETS.fit = ets(train)
ETS.forecast = forecast(ETS.fit, h = 7)
summary(ETS.fit)
```
```{r           }
checkresiduals(ETS.forecast)
accuracy(ETS.forecast, test)
```

```{r           }
autoplot(IBM.ts) +
  autolayer(ETS.forecast, series = "ETS Forecast") +
  autolayer(test , series = "Actual price")
```

## ARIMA Model

Next I implemented, an ARIMA Model and I checked the performance for the model, and the model   returned a MASE = 0.3730515  and a RMSE = 5.452839 , the ARIMA Model is better than the ETS model for this data.
```{r    ARIMA      }
ARIMA.fit = auto.arima(train)
ARIMA.forecast = forecast(ARIMA.fit, h = 7)
summary(ARIMA.fit)
```
```{r            }
checkresiduals(ARIMA.forecast)
accuracy(ARIMA.forecast, test)
```
```{r            }
autoplot(IBM.ts) +
  autolayer(ARIMA.forecast, series = "ARIMA Forecast") +
  autolayer(test , series = "Actual price")
```

##  Neural Network Model
Next I tried to use the Neural Network Model to see if it can generate better performance that the other models. For the Neural Network Model I got a MASE = 0.4623200 and RMSE = 7.424988 . the Neural Network Model performance for this data is lower than the ARIMA models.
```{r    nn      }
library(forecast)
nn.fit = nnetar(train, lambda=0)


sim <- ts(matrix(0, nrow=20, ncol=5), start=end(train)[1]+1)
for(i in seq(5))
  sim[,i] <- simulate(nn.fit, nsim=20)
library(ggplot2)
ggplot2::autoplot(train) + forecast::autolayer(sim)


nn.forecast <- forecast(nn.fit, PI=TRUE, h=7)
```
```{r           }
checkresiduals(nn.forecast)
accuracy(nn.forecast, test)
```

```{r           }
autoplot(IBM.ts) +
  autolayer(nn.forecast, series = "Neural Network Forecast") +
  autolayer(test , series = "Actual price")
```
## Conclusion

```{r  Conclusion   }
data.frame(Model = c("1.naive modlel", "2.snaive model", "3.ETS model", "4.ARIMA model", "5.Neural Network"), "RMSE" = c(8.048907,14.93724 ,10.320349,5.452839,9.559687), "MASE" = c(0.5629862,1.009509,0.7658349,0.3730515,0.6892889))
```
**Ultimately, the ARIMA model returned the best results (MASE = 0.3730515 and RMSE = 5.452839), when we compare it to the actual test data, So we can choose the ARIMA model among these models to forecast the future IBM adjusted stock price.**
