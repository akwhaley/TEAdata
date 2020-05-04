# Looking for Outliers in 2019 Texas Education Agency Performance Data

This project aims to find schools that are performing better (on state of Texas performance metrics) than schools with similar characteristics.  

I use data from the Texas Education Agency ([Data Source](https://rptsvr1.tea.texas.gov/perfreport/account/2019/download.html)).

In the notebook `campus_data.ipynb` I first explore some trends in the data and then fit a random forest model to a set of features that represent school characteristics.  The model is used to get a predicted score (I use overall scaled score) for each school and then I compare the score predicted by the model to the actual score to find outliers.  

### The model

I use a `RandomForestRegressor` model from the scikit learn python package.  The only non-default hyperparameter I use is `min_samples_leaf=30` to prevent overfitting the model.  As the model trains in a few seconds, I am able to tune this hyperparameter by hand.  

#### Feature selection

I choose the following school characteristic features: school level (elementary, middle, etc.), a flag for charter school, a flag for new school, student count, percentage economically disadvantaged, percentage limited english proficiency, percentage enrolled in an early college high school, percentage special education status, percentage enrolled in a TSTEM academy.  I originally use percentage high mobility as a feature but many schools do not have recorded data and so I ended up leaving it out of the final model.  It is easy to add this feature in the `campus_data.ipynb` notebook.

#### Target selection

I decided to use the Overall Scaled Score as the target predictor because it is the score that determines the overall rating.  It is easy to choose a different metric in the `campus_data.ipynb` notebook.

#### Model evaluation

In the initial model design phase I use a random 20% of the data as a test set and tune the model using the root mean squared error on the overall scaled score.  I also check the distribution of the residuals for bias.

#### Feature Importance

I view the feature importance attribute of the `RandomForestRegressor` and see that unsurprisingly the feature that determines the prediction most heavily is percent economically disadvantaged.  Other features with importance are LEP percentage, SPED percentage, student count, and school level. Percentage of high mobility showed a high importance score when it was included in the model.  Features that show an importance score of less than 0.01 are percent enrolled in ECHS, and the flag for charter school.  Features that show an importance score of 0 are the flag for new school, and percentage enrolled in a TSTEM academy.  

### Future Work

Some ideas for expanding on this project are:
1. Encorporating geographic region as a feature
2. Using a similar analysis on districts
