# Looking for Outliers in 2019 Texas Education Agency Performance Data

This project aims to find schools that are performing better (on state of Texas performance metrics) than schools with similar characteristics.  

I use data from the Texas Education Agency ([Data Source](https://rptsvr1.tea.texas.gov/perfreport/account/2019/download.html)).

In the notebook `campus_data.ipynb` I first explore some trends in the data and then fit a random forest model to a set of features that represent school characteristics.  The model is used to get a predicted score (I use overall scaled score) for each school and then I compare the score predicted by the model to the actual score to find outliers.  

### The model

I use a `RandomForestRegressor` model from the scikit learn python package.  The only non-default hyperparameter I use is `min_samples_leaf=10` to prevent overfitting the model.  As the model trains in a few seconds, I am able to tune this hyperparameter by hand.  

#### Feature selection

I choose the following school characteristic features: school level (elementary, middle, etc.), a flag for charter school, a flag for new school, student count, percentage economically disadvantaged, percentage limited english proficiency, percentage enrolled in an early college high school, percentage special education status, percentage enrolled in a TSTEM academy.  I originally use percentage high mobility as a feature but many schools do not have recorded data and so I ended up leaving it out of the final model.  It is easy to add this feature in the `campus_data.ipynb` notebook.

#### Target selection

I decided to use the Overall Scaled Score as the target predictor because it is the score that determines the overall rating.  It is easy to choose a different metric in the `campus_data.ipynb` notebook.

#### Model evaluation

In the initial model design phase I use a random 20% of the data as a test set and tune the model using the root mean squared error on the overall scaled score.  I also check the distribution of the residuals for bias.

#### Feature Importance

I view the feature importance attribute of the `RandomForestRegressor` and see that unsurprisingly the feature that determines the prediction most heavily is percent economically disadvantaged.  Other features with importance are LEP percentage, SPED percentage, student count, and school level. Percentage of high mobility showed a high importance score when it was included in the model.  Features that show an importance score of less than 0.01 are percent enrolled in ECHS, the flag for charter school, the flag for new school, and percentage enrolled in a TSTEM academy.  

## District Performance Visualized on a Map

It might be interesting to compute an average Overperformance score for each district.  Because school/district properties like urban/rural or geographic region are not directly in the data, it might be beneficial to visualize over/underperformance on a map.  
#### Average Overperformance for a district

To obtain the overperformance score for a district, I compute a simple average over all schools in the district weighted by student count.

#### Visualization

TEA provides district boundaries in GeoJSON format ([Data Source](https://opendata.arcgis.com/datasets/e115fed14c0f4ca5b942dc3323626b1c_0.geojson)) which can be easily merged with campus data aggregated by district.

I use a chloropleth map to visualize overperformance for districts first for the state and then by region.  

#### Observations

Overall, districts whose schools are over or under performing similar schools seem to be spread around the state.  
![Texas Districts Chloropleth](https://github.com/akwhaley/TEAdata/master/Texas_District_Chloro.png?raw=true))
