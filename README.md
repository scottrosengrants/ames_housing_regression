# Ames Housing Regression Model
#### Authored by: Scott Rosengrants

Home prices predicted using linear regression and the Kaggle Ames Housing dataset

### Problem Statement/ Hypothesis: 
Can a predictive price model be built based on the Ames, Iowa assessor data to predict the value and subsequent sale price of a home. Will the model will be accurate enough to be used in Opendoor’s business model? Will the model will work in new markets?

The stakeholders in this analysis are the business stratigists at Opendoor. A recomendation will be made at the end of the analysis stating whether an accurate home price predicting model can be built from the data provided in the Ames, Iowa dataset and whether this model is applicable to other markets Opendoor may be interested in. 

The data used in this analysis was taken from the Kaggle DSI-US-10 Project 2 Regression Challenge : https://www.kaggle.com/c/dsi-us-10-project-2-regression-challenge/data

The data files can be found in this repository under the 'datasets' folder. 

The folder contains:
- test.csv
- train.csv
- clean.csv

### Data Dictionary: 

The data dictionary for this data set is quite extensive, for brevity, it has been excluded from this read me file. It can be found in this repository 'ames_data_dictionary.txt' and at http://jse.amstat.org/v19n3/decock/DataDocumentation.txt

### Repo Structure: 
Included in this specific repo
- Jupyter notebooks
  - ames_clean.ipynb (data cleaning)
  - ames_eda.ipynb (data analysis and feature engineering)
  - ames_modeling.ipynb (several regression models applied and submission code for Kaggle)
- images folder (contains all images used for the presentation
- datasets
- Submissions (contains csv submission versions for Kaggle competition)
- ames_data_dictionary 
- presentation slides (pdf format) for Opendoor stakeholders
    
### Executive Summary: 

For this analysis the ames housing data set was cleaned, analyzed, underwent feature creation, and was fit to several linear regression models. Each step is summarized below with the Jupyter Notebook it occurs in specified.

**ames_clean.ipynb**
  - read in data
  - check data types
  - check for null values
  - correct, delete, or impute rows and values based on specific case <br>
    **Changes made to:** 
    - 'Lot Frontage'
    - 'Alley'
    - 'Mas Vnr Type'
    - Basement columns : 'Bsmt Qual', 'Bsmt Cond', 'Bsmt Exposure', 'BsmtFin Type 1', 'BsmtFin SF 1', 'BsmtFin Type 2', 'BsmtFin SF 2', 'Bsmt Unf SF', 'Total Bsmt SF', 'Bsmt Full Bath', 'Bsmt Half Bath'
    - 'Fireplaces','Fireplace Qu'
    -  Gareage columns:'Garage Type', 'Garage Yr Blt','Garage Finish', 'Garage Cars', 'Garage Area', 'Garage Qual',
      'Garage Cond', 'Paved Drive'
    - 'Pool QC'
    - 'Fence'
    - 'Misc Feature'
    - 'Id'
    - 'Street'
  -check for outliers
    - remove homes greater than 4000 square feet 

Data is exported as clean.csv to the datasets folder


Continue to next notebook, ames_eda

**ames_eda.ipynb**
  - read in data
  - Explore non-numeric features
  - Dummify:
    - 'MS Zoning' , 'Neighborhood' , 'Condition 1' , 'Condition 2' , 'Bldg Type' , 'House Style' , 'Kitchen Qual'
    - check for high correlations to price for each
  - Explore numeric features
    -run several heatmaps pulling out features that are highly correlated to price
  - From the above exploration assign a 'predictors' list of features selected that have a strong correlation to price
    - Check for linearity of 'predictors'
    - Check for equal distribution of features
    - check for high correllation between features
  - Convert '1st Flr SF', log_Gr Liv Area', and 'SalePrice' to logarithmic scale
  - Save 'predictors' list
<br>

Continue to next notebook, ames_modeling

**ames_modeling.ipynb**
  - read in data
  - create X and y based on the 'predictors' list and 'SalePrice
  - Train/Test Split data
  - Scale the data (these values will be held for models that required scaled data)
  - Test OLS Linear Regression
  - Test LASSO Regression (using scaled data)
  - Test Elastic Net Regression 
  - Select best model (OLS Linear Regression)
  - Load in Kaggle data
  - Clean Kaggle data
  - Run OLS Regression and save predictions
  - Convert results from logarithmic form back into standard form
  - Create a submission DataFrame
  - Export predictions to 'Submissions' folder as attemptX.csv
  
Outside Research
  - Opendoor (housing company): Explored their business model and fees associated with their service. <br>
    https://www.opendoor.com/
  - Realist Tax (housing data agrregator): Explored the data they provide and the locations they collect data on. <br>
    https://www.corelogic.com/products/realist.aspx
    
    
### Findings/Conclusions:
Based on my findings, the hypothesis can be deemed correct and true. <br>
A predictive price model can be built based on the Ames, Iowa assessor data to predict the value and subsequent sale price of a home. The model will be accurate enough to be used in Opendoor’s business model and the model will work in new markets (provided that similar housing data is provided). <br>

Through analysis of the Sci-Kit Learn linear regression models, OLS Linear Regression, LASSO Regression, and Elastic Regression it was found that OLS Linear regression had the highest r^2 scores and the lowest RMSE scores on this given data set. This is largely due to the fact that the model was never overfit to a detrimential amount therefore by applying LASSO and Elastic Net models, the bais of the model was atrificially increased unnecissarily and the prediction result accuracy suffered because of it. <br>

Proper feature selection in this data set was more impactful on the RMSE score than changing the model type. Features were selected based on their respective correlations to the target, 'Saleprice'. During the first iteration of the model correlations above .5 and below -.25 were included as feature. In the second iteration this range was increased to include features with a correlation of .2 or greater or less than -.1, the RMSE continued to increase and there was no drop in r^2 train vs test data to indicate over fitting. On the third feature itteration all correlation features were included, the theory here would be that the model would be overfit. Upon scoring the results, again there was an increase in RMSE and no significant drop in r^2 score from the train data to the test data, this again indicated the model was still not overfit. <br>

The theoretical limit to features (based on the sqrt(rows) theory) was 45 features. At the end of the analysis the model had exactly 45 features. Increased feature count testing was not conducted but the assumption is that the model would become overfit and the accuracy of the model when introduced to new data would decline. <br>

Beyond simple feature selection based on numerical column correlation to price, feature engineering was also included. The columns 'SalePrice' , '1st Flr SF', and 'Gr Liv Area' were all found to have a parabolic curve when tested for linearity. These features were transformed to the logarithmic scale by taking their natural log. This helped increase the accuracy of the model. <br> **note:** 'SalePrice' did need to be converted back to the standard scale before model scores could be determined.* <br>

Dummy variables were also used on 'Kitchen Qual' and 'Bsmt Qual' features to change them from objects to numerical features. Creating dummy columns for each feature quickly grows the feature list and exceeds the theoritical 45 allowable features for this data set. Careful selection of which columns to create into dummies and therefore a feature is needed. These selection was conducted by dummifying the feature in a new DataFrame, checking its correlation to price, and then comparing how strong those correlations were compared to other dummified features. Through this process of elimination 'Kitchen Qual' and 'Bsmt Qual' were found to be the best predictive features and complied with the 45 feature limit. <br>

In conclusion, the optimal model was found to be OLS linear regression. The features included in the model all had a correlation to the target 'SalePrice'. There did not seem to be a dilution of accuracy as features were added. The final feature count was 45 and the model produced a RMSE score of 22894.78 and R^2 score of 0.9.

### Recommendations/Further Steps:

My recomendation is that Opendoor can in fact create an accurate home price prediction model based on assessor data. There needs to be more research conducted on the optimal feature count, it may be greater than 45. The feature count will largely be determined on the data available for the target city, not all county assessors collect the same information. This issue can be somewhat resolved by using a data aggregator such as Realist Tax. The benifit of using a service such as this is that they collect data on the entire U.S. in a consistent and structured manner. This is vital to the models success as the input features to the model will need to be the same from city to city. It is advisable to take a sample data set from Realist Tax and reconduct this study, ensuring a similar model can be built.
