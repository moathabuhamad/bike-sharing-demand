# Report: Predict Bike Sharing Demand with AutoGluon Solution
#### Moath Abu Hamad


## Initial Training
### What did you realize when you tried to submit your predictions? What changes were needed to the output of the predictor to submit your results?
**Three runs were performed as follows:**
1. Initial Raw Submission
2. Added Features Submission
3. Hyperparameter Optimization (HPO)

- While submitting predictions obtained from all these five experiments, some of the experiments delivered negative predictions values.<br>
- Kaggle refuses the submissions containing negative predictions values obtained from the predictor. Hence, all such negative outputs from respective predictors were replaced with 0.<br>

### What was the top ranked model that performed?
The `top-ranked model` was the `(add features) model`, with the best **Kaggle score** of **0.46708 (on test dataset)**. This model was developed by training on data obtained using exploratory data analysis (EDA) and feature engineering without the use of a hyperparameter optimization routine. Upon hyperparameter-optimization, some models did show improved RMSE scores on validation data; however this model delivered the best performance on unseen test dataset.

## Exploratory data analysis and feature creation
### What did the exploratory analysis find and how did you add additional features?
- The `datetime` feature was parsed to extract hour information from the timestamp and converted into a datetime feature.
- Initially, the independent features `season` and `weather` were treated as integers. Realizing their categorical nature, they were transformed into the category data type.
- Distinct independent features such as `year`, `month`, `day` *(dayofweek)*, and `hour` were extracted from the `datetime` feature through feature extraction. Subsequently, the `datetime` feature was dropped.
- Upon analysis, it was observed that `casual` and `registered` features significantly improved the RMSE scores during cross-validation and exhibited high correlation with the target variable `count`. However, since these features were only present in the train dataset and not in the test data, they were disregarded during model training.
- A new categorical feature, `day_type`, was introduced based on the existing features `holiday` and `workingday`, effectively categorizing days into "weekday", "weekend", and "holiday".
- Data visualization techniques were employed to extract insights from the features.

### How much better did your model perform after adding additional features and why do you think that is?
- The incorporation of additional features **boosted model accuracy from 1.79985 to 0.46708** compared to the initial model, which lacked thorough exploratory data analysis and feature engineering.
- Model performance saw improvement following the conversion of certain categorical variables from `integer` to their appropriate **categorical** data types.
- Alongside excluding **casual** and **registered** features during model training, **atemp** was also eliminated due to its strong correlation with **temp**, aiding in reducing multicollinearity.
- Additionally, splitting the **datetime** feature into individual features such as **year**, **month**, **day**, and **hour**, along with introducing **day_type**, contributed to enhanced model performance by facilitating better assessment of seasonal trends and historical patterns in the data.

## Hyper parameter tuning
### How much better did your model preform after trying different hyper parameters?
Hyperparameter tuning proved advantageous by significantly improving the model's performance compared to its initial iteration. Throughout the hyperparameter optimization experiments, three distinct configurations were tested. While the hyperparameter-tuned models demonstrated competitive performances relative to the model augmented with exploratory data analysis (EDA) and additional features, the latter notably outperformed on the Kaggle (test) dataset.
- Using autogluon for training required careful consideration of prescribed settings. However, the performance of hyperparameter-optimized models suffered due to limited exploration options from fixed user-provided values.
- Key parameters such as 'time_limit' and 'presets' were vital in hyperparameter optimization with autogluon.
- Autogluon may struggle to build models within the specified hyperparameter set if the time limit is insufficient.
- Lighter and faster preset options like 'medium_quality' and 'optimized_for_deployment' were tested, with "optimized_for_deployment" preferred for its quicker performance.
- Balancing exploration versus exploitation is a significant challenge when using AutoGluon with fixed hyperparameter ranges.

### If you were given more time with this dataset, where do you think you would spend more time?
Given extended time with the dataset, I aim to explore further possibilities by running AutoGluon for an extended duration with a high-quality preset and intensified hyperparameter tuning.

### Create a table with the models you ran, the hyperparameters modified, and the kaggle score.
|model|hpo1|hpo2|hpo3|score|
|--|--|--|--|--|
|initial|default|default|"presets: 'high quality' (auto_stack=True)"|1.79985|
|add_features|default|default|"presets: 'high quality' (auto_stack=True)"|0.46708|
|hpo|Tree-Based Models: (GBM, XT, XGB & RF)|KNN|-|0.5389|

### Create a line plot showing the top model score for the three (or more) training runs during the project.

![model_train_score.png](/model_train_score.png)


### Create a line plot showing the top kaggle score for the three (or more) prediction submissions during the project.

![model_test_score.png](/model_test_score.png)


## Summary
- The bike sharing demand prediction project utilized the AutoGluon AutoML framework for Tabular Data extensively.
- AutoGluon facilitated the creation of both stack ensembles and individual regression models on tabular data, aiding in rapid prototyping.
- The top-performing AutoGluon-based model saw significant improvements by incorporating data from thorough exploratory data analysis (EDA) and feature engineering, even without hyperparameter optimization.
- Automatic hyperparameter tuning, model selection, ensembling, and architecture search capabilities of AutoGluon enabled exploration of optimal options.
- While hyperparameter tuning with AutoGluon improved performance over the initial submission, it didn't surpass the model with EDA and feature engineering but without hyperparameter tuning.
- Hyperparameter tuning with AutoGluon, without default hyperparameters or random parameter configurations, proved complex and highly dependent on factors like time limit, presets, model families, and hyperparameter ranges.
