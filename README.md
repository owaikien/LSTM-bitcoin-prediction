# LSTM-bitcoin-prediction
The aim of this project is to create a LSTM neural network model that predicts $BTC close price using multiple relevant features. In other words, this is also known as multivariate time series forecasting.

<h2>Data</h2>

I used live data pulled from CryptoCompare API. Here's a summarise and the intuition behind the data that I've used:

````
1. Historial $BTC Price
2. Historical $BTC Trade Volume
3. Holder Distribution: Distribution of coin value among the players in the network, an increase in addresses with disproportionately large number of coins is interpreted as bullish sign, as this is a sign that they are becoming more optimistic. I focus mainly number of addresses who holds 1000 to 100000 $BTC
4. Difficulty: Refers to mining difficulty. High price -> attract more miners to the network (as they are attracted by the high reward/return) -> increase in hash rate -> increase in difficulty


````

To learn more about what I did, visit my medium page where I explained everything:

Intuition behind the model and data used: [Part 1](https://medium.com/@kienong2000/using-multiple-features-to-predict-bitcoin-close-price-multivariate-time-series-forecasting-with-e43cb5da63b1)

Data collection & cleaning + Model building and prediction: [Part 2](https://medium.com/@kienong2000/using-multiple-features-to-predict-bitcoin-close-price-multivariate-time-series-forecasting-with-e4ecd2a4e008)
