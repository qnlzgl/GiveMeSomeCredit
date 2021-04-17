Name: Guanlun Zhao
Email: guanlun.zhao@u.nus.edu

**Tell us how you validate your model, which, and why you chose such evaluation technique(s).**

- I validated my model with k-Fold Cross-Validation to minimize sampling bias. The data was split into k folds, then trains the data on k-1 folds and test on the one fold that was left out. I used k=5 in this case. 

**What is AUC? Why do you think AUC was used as the evaluation metric for such a problem? What are other metrics that you think would also be suitable for this competition?**
- AUC stands for "Area under the ROC Curve" and it measures the entire two-dimensional area underneath the entire ROC curve (think integral calculus) from (0,0) to (1,1). AUC can be used as a summary of the model skill and can be used to compare two models. The ROC curves of different models can be compared directly in general or for different thresholds. The greater the AUC the better the classifier/model.
- AUC was used because the dataset is imbalanced (Around 6% of samples defaulted)
- Other metrics we can use to evaluate imbalanced dataset includes precision, recall and f1 score.

**What insight(s) do you have from your model? What is your preliminary analysis of the given dataset?**

Some of the insights & Preliminary analysis is stated below
1. Null value analysis
-> 19.8% of MonthlyIncome column and 2.6% of NumberOfDependents column are NULL. I am using mode/mean imputation to impute missing data.

2. Outlier analysis
-> max value in DebtRatio is 329664 which means that someone owes 330,000 times what they own (practically impossible). Distribution of values is skewed The issue is dealt with by remove rows containing 3% biggest value in DebtRatio.
-> When NumberOfTimes90DaysLate has values above 17, there are 267 instances where NumberOfTimes90DaysLate, NumberOfTime60-89DaysPastDueNotWorse, NumberOfTime30-59DaysPastDueNotWorse share the same values(96 and 98). The issue is dealt with by removing rows with NumberOfTimes90DaysLate>17

3. Other findings
-> RevolvingUtilizationOfUnsecuredLines is the ratio of the total amount of money owed to total credit limit. When it increases, chance of defaulting increases too.
-> From NumberOfTimes90DaysLate column, we can find that no one is between 17 and 96 times late, but hundreds of people are 98 times late.
-> When we analyse based on age column, we can find that more younger people are defaulting.
-> After training, the top 4 most important features are: DebtRatio, RevolvingUtilizationOfUnsecuredLines, MonthlyIncome

**Can you get into the top 100 of the private leaderboard, or even higher**
- Yes. I used personal GPU (Nvidia GTX1060) to perfor training. It took 366.9min to finish training. Best private score is 0.86774, which ranks 67th in private leader board.
Best performing parameters are given below:
- XGBClassifier(base_score=0.5, booster='gbtree', colsample_bylevel=1,
              colsample_bynode=1, colsample_bytree=0.6, gamma=0, gpu_id=0,
              importance_type='gain', interaction_constraints='',
              learning_rate=0.006, max_delta_step=0, max_depth=7,
              min_child_weight=9, missing=nan,
              monotone_constraints='(0,0,0,0,0,0,0,0,0,0)', n_estimators=750,
              n_jobs=0, num_parallel_tree=1, predictor='gpu_predictor',
              random_state=0, reg_alpha=0, reg_lambda=1, scale_pos_weight=1,
              subsample=0.6, tree_method='gpu_hist', validate_parameters=1,
              verbosity=None)
