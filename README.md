# Google Analytics Customer Revenue Prediction
 This script explains the approach used to solve kaggle challenge [Google Analytics Customer Revenue Prediction](https://www.kaggle.com/c/ga-customer-revenue-prediction).
Here, the challenge is to predict how much GStore customers will spend (i.e revenue generated by each GStore customers).

As given in problem description, the 80/20 rule - 80% of revenue comes from only 20% of the customers.So our task is to predict the Gstore's revenue from those customers (predicted values are natural log of sum of all transactions per user).

A jupyter notebook named [customer_revenue_prediction.ipynb](./customer_revenue_prediction.ipynb) is uploaded that has python code and detailed explanation.

### Datafields:

* fullVisitorId- A unique identifier for each user of the Google Merchandise Store.
* channelGrouping - The channel via which the user came to the Store.
* date - The date on which the user visited the Store.
* device - The specifications for the device used to access the Store.
* geoNetwork - This section contains information about the geography of the user.
* sessionId - A unique identifier for this visit to the store.
* socialEngagementType - Engagement type, either "Socially Engaged" or "Not Socially Engaged".
* totals - This section contains aggregate values across the session.
* trafficSource - This section contains information about the Traffic Source from which the session originated.
* visitId - An identifier for this session. This is part of the value usually stored as the _utmb cookie. This is only unique to the user. For a completely unique ID, you should use a combination of fullVisitorId and visitId.
* visitNumber - The session number for this user. If this is the first session, then this is set to 1.
* visitStartTime - The timestamp (expressed as POSIX time).

### Data Pre-processing:

* Some of the fields contain nested json strings, so they are converted to flat and seperate tables which are convenient to do EDA and train the model.
* Datetime conversions are made on date type fields.
* Each row in the dataset is considered as one visit to the store. Then the ratio of revenue generating customers to total number of customers is found out.
* Each columns are analysed and the columns which contain constant values are removed from the dataset.
* Missing values are filled with 0. Categorical variables are identified and they are label encoded. Numeric variabled are convert to float type.(Except IDs and dates)
* Train data is divided into dev set and cross validation set in the ratio roughly 8:2 to know how well our model performs on new data.


### Model Selection:

As we have large dataset (with more columns >30 and rows > 10000)its better to use ensemble learning (gradient boosting models).

Ensemble is a collection of predictors which come together (e.g. mean of all predictions) to give a final prediction.Boosting is an ensemble technique in which the predictors are not made independently, but sequentially (can overfit if stopping criteria isnt chosen well).

The gist on how Gradient Boosting Models work is,

A base model is created and is used to make predictions on the whole dataset.
Then residual is calculated and observations which are incorrectly predicted are given higher weights.
The next sequential model tries to correct the errors from the previous model.
Similarly, multiple models are created, correcting the errors of the previous model.
The final model (strong learner) is the weighted mean of all the models (weak learners).

e1= y - y_pred1<br>
y_pred2 = y_pred1 + e1_pred<br>
e2 = y - y_pred2<br>
and so on.

XGBoost and LightGBM are effiecient gradient boosting models.
XGBoost is a regularised boosting model and hence reduces overfitting. It also implements parallel processing and is faster compared to other boosting algorithms.

Light GBM is fast, distributed gradient boosting framework. Unlike other boosting algorithms it splits the trees leafwise and not level wise.Leaf wise splitting may lead to overfitting. This can be avoided by specifying tree-specific hyper parameters like max depth.It trains faster(on larger datasets) compared to other boosting algorithms like XGBoost.Light GBM grows tree vertically,It will choose the leaf with max delta loss to grow. 
Hence we'll use LightGBM to train our model and predict revenue from test set.

Note : Light GBM is sensitive to overfitting and can easily overfit small data. So the challenge in using LGBM is hyper-parameter tuning.


### Training:

* Model is run several times and hyper-parameters are fine tuned.
* Error is calculated to analyse performance of the model on given data.
* A final dataframe consisting of 'fullVisitorId' and predicted revenue values of test data (i.e natural log of sum of revenue per customer) is created.
* Finally submission.csv file is created. 

### Note:
Complete code along with explanation is uploaded here- [customer_revenue_prediction.ipynb](./customer_revenue_prediction.ipynb) as jupyter notebook.
