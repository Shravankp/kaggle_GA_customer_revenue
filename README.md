#Google Analytics Customer Revenue Prediction
EDA Predict how much GStore customers will spend
XGBOOST:
large datasets
no standard scalar
Accuracy matters over speed

ensemble is just a collection of predictors which come together (e.g. mean of all predictions) to give a final prediction.
bagging - build many independent predictors/models/learners and combine them using some model averaging techniques. 
	handles overfitting , independent classifiers
	parallel learning
	eg: Random Forest
Boosting is an ensemble technique in which the predictors are not made independently, but sequentially.
	can overfit if stopping criteria isnt chosen well, sequential classifiers
	sequential learning
	eg: Gradient Boosting

both use decision trees

e1= y - y_predicted1 
y_predicted2 = y_predicted1 + e1_predicted
e2 = y - y_predicted2



to predict: natural log of sum of all transactions per user.

json.loads() 			#takes in a string and converts to dict or list object.
json_normalize() 		#converts semi-structures json to flat table

So the ratio of revenue generating customers to total number of customers is in the ratio os 1.3%




