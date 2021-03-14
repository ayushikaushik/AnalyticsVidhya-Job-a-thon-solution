# AnalyticsVidhya-Job-a-thon-solution
(Job-a-thon)[https://datahack.analyticsvidhya.com/contest/job-a-thon/] is a hackathon conducted by Analytics Vidhya on 26-28 Feb,2021. This repository contains my solution which gave me 0.68 score and rank 254 on AV leaderboard.

Problem Statement: Your Client FinMan is a financial services company that provides various financial services like loan, investment funds, insurance etc. to its customers. FinMan wishes to cross-sell health insurance to the existing customers who may or may not hold insurance policies with the company. The company recommend health insurance to it's customers based on their profile once these customers land on the website. Customers might browse the recommended health insurance policy and consequently fill up a form to apply. When these customers fill-up the form, their Response towards the policy is considered positive and they are classified as a lead.

Once these leads are acquired, the sales advisors approach them to convert and thus the company can sell proposed health insurance to these leads in a more efficient manner.

Now the company needs your help in building a model to predict whether the person will be interested in their proposed Health plan/policy.

Public LB rank: 258
Private LB rank: 254

My Approach:
This is a binary classification problem with tabular data.
After imputing missing values with mode and converting categorical columns into numerical columns using dummy variables, I applied XGBoost algorithm to predict target variable. I cross-validated it using 10 folds due to which my local score and public LB score matched.

Observations from EDA:
•	There are missing values in only 3 columns namely Health Indicator, Holding_Policy_Duration and Holding_Policy_Type. Since these are categorical columns and no domain knowledge can be gathered about their classes, I imputed missing values using mode. Alternatively, filling them with some constant say 0 also gave same result.
•	There are no duplicate values in train data.
•	Region_Code feature has very large number of distinct classes. There is no information about it and no pattern with respect to the target variable. So, I cannot transform it much. I tried frequency encoding for it but no major improvement was achieved in score. Even dropping this feature didn’t make any difference.
•	Features Accomodation_Type, Reco_Insurance_Type and Is_Spouse have only two distinct classes so I did binary encoding for them.
•	For other features I tried to reduce number of distinct classes using their frequency and response rate but score reached only 0.62. I also tried using FeatureHasher function from sklearn.feature_extraction but score reached only 0.59.
•	So,finally I simply one hot encoded these features using get_dummies function from pandas.

Other models I tried:
•	Using Random Forest, I reached 0.65 score.
•	By reducing distinct classes using their response and frequency rate I reached 0.62.
•	Reducing distinct classes using FeatureHasher helped me to reach 0.61.
•	I made an ensemble model from LightGBM, XGBoost and CatBoost. It also gave me my highest score of 0.68 which I also got from XGBoost.
•	I divided given datasets into two parts depending on Reco_Insurance_Type feature and applied various models separately. Its score was 0.61.
