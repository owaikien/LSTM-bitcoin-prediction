# LSTM-bitcoin-prediction
The aim of this project is to create a LSTM neural network model that predicts $BTC close price using multiple relevant features. In other words, this is also known as multivariate time series forecasting.

<h2>Data Collection and Preparation</h2>

I used live data pulled from CryptoCompare API. Here's a summarise and the intuition behind the data that I've used:


1. Historial $BTC Price
2. Historical $BTC Trade Volume
3. Holder Distribution: Distribution of coin value among the players in the network, an increase in addresses with disproportionately large number of coins is interpreted as bullish sign, as this is a sign that they are becoming more optimistic. I focus mainly number of addresses who holds 1000 to 100000 $BTC
4. Difficulty and Hashrate: Refers to mining difficulty. High price -> attract more miners to the network (as they are attracted by the high reward/return) -> increase in hash rate -> increase in difficulty
5. Number of Active Addresses: a unique address that has conducted transaction actively on the network over a given period of time, an increase in number of active addresses on a blockchain can indicate increase usage and adoption of the network, hence signalling strength of that coin/network
6. Transaction Counts: To analyze usage and adoption of a blockchain, refers to number of transactions processed on that network over a given period of time
7. Large Transaction Counts: Transactions involving a significant number of cryptocurrencies, or known as ‘whale transactions’, increase in large transactions can impact the market as it involves large amount of cryptocurrency being bought or sold, hence affecting the supply and demand of that cryptocurrency.

The steps after include merging all the data, performing feature engineering and preparing sequential data to be fed into my model. To summarize, I did a 85%-15% train-test split for my data and scaled the data using `MinMaxScaler()`. I then processed the data into the following format:

````
[n_samples, time_steps, n_features]
`````

where `n_samples` is your number of samples, `time_steps` is the time interval (which is normally the number of LSTM cells), and `n_features` is the number of features that you’ve got. Ultimately what I am going to do is to use the data of 99 previous days to predict the close price of the 100th day, and repeat this until the last 99 data in my dataset.

<h2>Model Building and Training</h2>

The model I've used here is a simple 2-layer LSTM Recurrent Neural Network model implemented using `Keras`. Here's a diagram displaying the architecture of the model:

![lstm_architecture](https://github.com/owaikien/LSTM-bitcoin-prediction/assets/95358608/9bae97a9-2354-4a1d-b916-ce1ee35bd0a1)

To put simply, the input shape is (99, 17), which is fed into the LSTM layer. The LSTM layer has 100 units, where the output is passed to a Dense layer with 1 unit. The output of the Dense layer is the final output.

![value_loss](https://github.com/owaikien/LSTM-bitcoin-prediction/assets/95358608/234d3ac4-827c-4777-a68f-aeb4ce3e1081)

I've used [mean absolute error](https://en.wikipedia.org/wiki/Mean_absolute_error) to measure my loss function and from the plot above we can see a gradual decrease in loss value to ~0.02.

<h2>Predict</h2>

![model_prediction](https://github.com/owaikien/LSTM-bitcoin-prediction/assets/95358608/7f549192-710a-45b8-9e70-6c237e89841b)

It looked like the model predicted the price action quite well for the test data (610 days). The model seemed to predict the price action quite well for the first ~300 days, while the predicted price were mostly slightly higher than actual price for the last ~300 days

<h2>Backtest</h2>
Finally, I implemented a simple stimulation to backtest this model. How this works is that I will start with $10,000 and for each day in my test data (610 days), I would use my model’s prediction to determine if I wanted to go long or go short. The percentage of the total profit or loss is then calculated and compared to 2 other benchmark strategies: 

1. “buy and hold” where I buy $BTC and hold it for 610 days
2. “random” where I just randomly go long and short using `random` library. I repeated this strategy for 50 times and get its average return. 

Here's what I got:

| LSTM | BUY & HOLD | RANDOM LONG & SHORT |
| ------------ | ----- | ------------------ |
| -16.83% | -52.01% | -24.3% |

The results of this backtest strategy showed that my LSTM model outperformed the other 2 strategies. Although all strategies returned a total loss in portfolio value, I think this is sensible especially in this kind of funky bear market. The LSTM model outperformed the “random” strategy by ~8%, while it outperformed the “buy and hold strategy” by >300% !

To read more I’ve also written some articles explaining the intuition behind some of the steps I’ve did in this model in Medium: <br>
Intuition behind the model and data used: [Part 1](https://medium.com/@kienong2000/using-multiple-features-to-predict-bitcoin-close-price-multivariate-time-series-forecasting-with-e43cb5da63b1) <br>
Data collection & cleaning + Model building and prediction: [Part 2](https://medium.com/@kienong2000/using-multiple-features-to-predict-bitcoin-close-price-multivariate-time-series-forecasting-with-e4ecd2a4e008)
