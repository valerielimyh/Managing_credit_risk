Managing credit risk 
==============================

# Introduction
After issuing your customer a loan, how do you determine whether they would default or not? To address this issue, I developed a classification model to identify whether a company would default on their loan by using behavioural features such as the number of employees in their company, the industry they are in, as well as information available on loan applications such as their loan amount, whether they took up other loans etc. 

I tried various models, from logistic regression as my baseline model, to random forest, as well as gradient boosting models such as LGBM, Catboost and XGBoost. 

# Results
Random forest performed better than log regression in terms of lower log loss, higher precision, higher recall, and hence f1 score (which is a balance of precision and recall). However, random forest is a bagging algo that builds very deep trees (the best model had a max depth of 50) and many trees (had 2000 trees) to create strong learners, and thus may be prone to overfitting. Gradient boosting models outperform random forest because
1. gradient boosting models construct successive training sets based on incorrectly classified examples (weak learners). hence, gradient boosting models assigns a higher weight to weak learners such that they have more incentive to solve the more difficult predictions. This enables gradient boosting models to handle overfitting and to deal with class imbalance too. 

2. LGBM and Catboost have categorical encoding support built-in that enables it to handle categorical features better. 

Among the 3 gradient boosting models, LGBM performed the best, in terms of had a higher precision and recall, AUC, and lower log loss. It also had a short training time.

I also modified the threshold to determine default status. Instead of using the default threshold of 0.5, I lowered it to 0.4. Doing so would result in a higher recall, and lower precision. but in determining whether a company would default, I assume we would want a higher recall, i.e minimise false negatives, which is the instances where a company defaults on its own but we failed to identify them. Having actual dollar value would help in making this judgment of what threshold to set. Lowering the threshold for LGBM improved recall score (from 0.82) to close to 0.9, and precision was not significantly reduced (from 0.91 to 0.85). Log loss remained roughly similar too. 

Oversampling defaulters class or undersampling non-defaulters did not improve model's performance, possibly because the class imbalance (70% non defaulters, 30% defaulters) were not too severe. 

I tried building a neural network using Keras, with 3 hidden layers, consisting of 32, 64, 128 neurons, with Relu activation and applying Batch Normalisation. I tried several other architectures but neural networks did not result in a better model performance than LGBM. 

# Conclusion
With the predicted probabilities generated from LGBM, I perform lift analysis to identify who are the high risk customers that credit risk analysts should intervene early or quickly. I identified 47 companies who have the highest likelihood of defaulting. Credit risk analysts can account for this risk by intervening early, e.g. longer repayment period such that each repayment amount  is smaller and thus more manageable for them, or charging a higher interest rate to get them to pay more upfront etc.

