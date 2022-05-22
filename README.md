# Song Popularity

### Overview
Music has been an integral part of our culture all through-out human history. Being able to identify the factors which makes a song popular has major impact to businesses that thrive on popular music, namely radio stations, record labels, and digital and physical music marketplaces. Predicting popular songs can be applied to the problem of predicting preferred songs for a given population.

### Dataset
The data was part of a competition arrange by ML Space hosted on Kaggle. Based on features like acousticness, danceability, energy, instrumentalness, key, liveness, loudness, etc we had train a model which would assign a popularity score to it, highest score of popularity being 1 and lowest was 0. There were 15 numerical fearures present in this dataset. The model performance metrics used was ROC curve(AUC). 

### EDA
Below are some of the key observations dervied from analyzing the data -
1. The data suffered from high number of Null values both in train and test sets. Below is a distribution of Null values over each fearure for both train and test sets.

<img src='https://github.com/Ruparna25/song_popularity_detection/blob/main/Images/null_values.JPG' width=500 height=400></img>

Hence we see out of all the 15 features, ther are 7 feature contains Null values for almost 10% of the data. There are overall approximately 22000 records which contained in atleast one of the columns.

2. The data suffered highly from Multi-Collinearity issue. Multicollinearity is the occurrence of high intercorrelations among two or more independent variables in a multiple regression model. Multicollinearity can lead to skewed or misleading results. Below is a heat-map to show multicollinearity of the data I am working with.

<img src='https://github.com/Ruparna25/song_popularity_detection/blob/main/Images/heat_map.JPG' width=900 height=500></img>

Note: Strong positive correlation exist between loudness and energy. Strong negative correlation exist between loudness and acousticness, and acousticness and energy.

3. Class Imbalance and Class overlapping problem -
Class Imbalance is very common in classification problems. In this data, the number of non-popular songs out number the number of popular songs, hence there are roughly 2 non-poular song for 1 popular song.

<img src='https://github.com/Ruparna25/song_popularity_detection/blob/main/Images/class_imbalance.JPG' width=600 height=300></img>

In addition, we found that there was class overlapping issue. Class overlap is caused due to ambiguous regions in the data where the prior probability of two or more classes are approximately equal. Due to this the distribution of non-popular song across a feature overlaps the distribution of popular songs over the same feature. Below I have shown a distrubtion of the poular and non-poular songs over a few features.

<img src='https://github.com/Ruparna25/song_popularity_detection/blob/main/Images/overlap_data.JPG' width=800 height=400></img>

### Feature Handling
1. It was observed from instrumentalness that is closed clustered around the mean. The distribution of Instrumentalness is shown below. So to have a smoother distibution, the log of instrumentalness was used as a Feature.

<img src='https://github.com/Ruparna25/song_popularity_detection/blob/main/Images/Instrumentalness.JPG' width=800 height=400></img>

2. Null Imputation - Different imputation techniques were used and based on model performance the .
- Simple Imputation technique - Imputing the missing data with mean value of each column.  
- KNN Imputer technique - Imputes missing values using the weighted or unweighted mean of the desired number of nearest neighbors
- Iterative Imputer - This is part of an experimental library and this technique uses round-robin linear regression, modeling each feature with missing values as a function of other features, in turn. 

All the error related to each imputation technique is documented under the **Model Accuracy Data.xlsx** file.

3. Sampling issue - Used SMOTE (Synthetic minority Oversampling Technique) pipeline to catter with the class imbalanced problem. The pipeline combines SMOTE with random undersampling technique of the majority class. First oversample the minority class percent the number of examples of the majority class (e.g. about 1,000), then use random undersampling to reduce the number of examples in the majority class to have 50 percent more than the minority class (e.g. about 2,000).

### Model 
Trained models like SVC, KNN and RandomForest with both Iterative and KNN Imputer. Also, I used GridSearchCV for hypertuning of parameters. 
Addition to these models, I tried to train XGBoost and LGBM which accepts missing values and internally impute the missing value based on the available data.

### Performance
Performance metrics used is ROC curve (AUC). The accuracy scores are documented under **Model Accuracy Data.xlsx** and the highlighted rows are the ones yielding the best performance for that model.
