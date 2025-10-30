# Monthly-precipitation-LSTM-forecasting
An LSTM implementation for monthly precipitation forecasting using Kaggle dataset: https://www.kaggle.com/datasets/rtatman/did-it-rain-in-seattle-19482017

## 🔍 Overview

![System Flowchart](flowchart.png)

The system operates by:

A. **Data pre-processing** 
- check for invalid elements (None, NaN, Null, and empty strings) in the raw daily precipitation dataset
- fill/replace those invalids with 0s
- check for any skipped dates/rows
- convert the boolean operator elements (True and False) into binary integers (0s and 1s) in the "RAIN" dataset column

B. **Feature engineering** 
- convert the original daily precipitation into monthly dataset: ("Prec" = average daily precipitation in mm each month, "T_max" = maximum temperature recorded in celsius each month, "T_min" = minimum temperature recorded in celsius each month, "Rain" = percentage of rainy days recorded in decimal each month)
- split the datset, by taking the last 2 years (24 rows) of data as the test data and the rest as the train data
- extract historical temporal fatures in the train data, by associating each target ("Prec") with the array of historical features from -12 months to -1 month, leaving X_train (train features) as a 3 dimensional array (n_samples, time_steps, n_features).

C. **LSTM model development**
- create the model architecture using tensorflow.keras, comprising: 2 LSTM layers [64, 32], one hidden dense layer [16], and one dense output layer [1]

D. **Train data forecasting**
- fit the model using the extracted X_train and the associated y_train (monthly "Prec" target), epochs = 2000
- predict y_train
- print the performance metrics (RMSE, MAE, and R^2)

E. **Test data forecasting**
- apply sliding window and walk forward validation by appending the X_train and y_train with the corresponding historical temporal features and actual "Prec" target of the test data as each month's prediction has passed
- refit the model using the updated X-_train and y_train for next month's prediction, epochs = 100
- plot the results
- print the performance metrics (RMSE, MAE, and R^2)


## 📦 Files in This Repo

| File                                             | Description                                                   |
|--------------------------------------------------|---------------------------------------------------------------|
| `LSTM monthly precipitation forecasting.ipynb`   | Main script to run implement the LSTM forecasting             |
| `Seattle Prec_1948-2017.csv`                     | Raw daily preciopitation dataset                              |
| `requirements.txt`                               | Python package dependencies                                   |
| `README.md`                                      | This documentation                                            |
| `flowchart.png`                                  | Flowchart image of this project                               |
