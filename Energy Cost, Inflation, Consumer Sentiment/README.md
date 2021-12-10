### Consumer Sentiment levers - Is inflation the true culprit of United States fear for the economy?


## Practicum I Overview:
   This practicum is to determine if inflation in the United States is a driving factor of the consumer sentiment drop in recent months. By reviewing monthly data going back as far as 2001 I was able to gain insights into what is a driving force of consumer sentiment in the United States using both multiple regresssion, two-way anova, and predictions in random forest regression. It begins by taking the assumption we hear and see in media sources today. 
   
   ## Process
   
   The Energy Information Administration has an API that allows the pulling of historical data relating to energy prices of many differnt types. In the case of this project I chose natural gas and residential electricty prices.
  

 
<img src="https://github.com/stiznan/stiznan/blob/main/Energy%20Cost%2C%20Inflation%2C%20Consumer%20Sentiment/Images/EIA%20API%20Code.JPG" width=50% height=50%>
   
   
   I then pulled information from the Bureau of Labor Statistics for consumer prices, University of Michigan & Florida for their consumer sentiment survey to represent sentiment, and lastly historical inflation rates. Each of these data sources required wrangling in their own way. For example, for the EIA API this was pulling information using the Requests library and converting the information into a json and then transforming it into a CSV. For the others it was online information that was either downloaded as an Excel file or copied and placed in a CSV file in Excel. I then reworked the layouts of these Excel files and put them in such a way they could be loaded into python as a csv file. 
   
   Once these tables were created I needed to have a common index. I converted the dates, which were monthly, into date_time formats from pandas and joined them all into one complete table to begin the EDA, Multiple Regression, and ML process. 
  
   <img src="https://github.com/stiznan/stiznan/blob/main/Energy%20Cost%2C%20Inflation%2C%20Consumer%20Sentiment/Images/Merge%20Index.JPG">
   
   The EDA process involved first exploring my target feature, which was consumer sentiment. I wanted to understand the best feature and found given how correlated they were it was best to drop the additionally added surveys in the Florida Sentiment and average both Michigan and Florida together as one representation of consumer sentiment.
   
  <img src="https://user-images.githubusercontent.com/70237462/145614678-0cd8c10b-5836-4b13-b54f-08b87cf0035c.png" width=50% height=50%>
  
  I then addresed null values and found given there were many null values between 1990-2021 I should drop all of the 90s decade. The dataset was best used from 2001-2021. 
  
  ![image](https://user-images.githubusercontent.com/70237462/145615290-e734bb71-4c71-4b32-87a4-8bc793d2aca5.png)
  
  Given how cyclical much of the data was, I belived it was best to fill null values with median prices.
  
  ![image](https://user-images.githubusercontent.com/70237462/145615439-450facd8-3df3-4dfe-9fc7-5c02d203a339.png)
  
  I addressed outliers by applying a MinMax Scaler and then started the feature selection process with a Random Forest Regressor to determine importance.
  
 <img src="https://user-images.githubusercontent.com/70237462/145615713-5f306e5e-d866-4351-9bd9-ab0cfffcb2d6.png" width=75% height=75%>
 
 Considering how large a correlation matrix would be for this many features I believed the best way to address these features was through a VIF test.
 ![image](https://user-images.githubusercontent.com/70237462/145616128-c49b409e-18b2-4f7f-80bf-5c4f0542e107.png)
 ![image](https://user-images.githubusercontent.com/70237462/145616270-9566a22c-8c91-423f-97b9-b71870b1819e.png)
 
 Once features were chosen and removed I move forward with the multiple linear regression testing. The result showed promising results:
 ![image](https://user-images.githubusercontent.com/70237462/145616522-ea11fd7e-b514-4ee5-af30-e3ebfcd862b1.png)
 
 ![image](https://user-images.githubusercontent.com/70237462/145616560-191ab0e7-63c1-40e8-9ead-2b821f71ad45.png)

### Findings:

This model is considered to be a better with independet variables included given the f-statistic p-value of 1.51e-77

The following features meet the .05 level:
- Residential Natural Gas Price
- Electrical Price <br>
#### Commodity Prices <br>
- Bacon
- Bananas 
- Bread
- Chicken
- Eggs
- Inflation

Surprising that gas unleaded prices and milk do not meet the .05 level. 

#### Coefficients of p-values meeting the .05 level

- Resi_Gas	__-0.1166__
- Elect_Price	__0.2706__
- Bacon __0.2178__	
- Bananas 	__-0.2587__	
- Bread 	__-0.4809__
- Chicken __0.1626__
- Eggs __0.0871__
- Inflation	__0.0697__

Residential Natural Gas increases cause negative sentiment, as well as Bananas and Bread. However surprisingly Inflation increases cause a positive increase in sentiment. 

And when this model was tested against the test set, it reflects the unexplained 20% variance:
![image](https://user-images.githubusercontent.com/70237462/145617105-059d4009-de0f-4222-8301-db288813231a.png)


## Random Forest Regressor Model
Given that a linear regression model is parametric, I would like to see how a nonparametric model performs. I will introduce a random forest regressor model. 

I first determined the number of trees that would be optimial for this model:

![image](https://user-images.githubusercontent.com/70237462/145618143-88b26327-4de6-4bab-b4fe-1e0828ba1408.png)


![image](https://user-images.githubusercontent.com/70237462/145617934-72efee57-be2e-4dde-9499-24f49742a0a2.png)

### When I follow the effect of n_estimators we see that around 140 trees is best for the future model. So we will now apply this to the model. 


![image](https://user-images.githubusercontent.com/70237462/145617901-2c8b1dc2-5b6c-46f4-9c1d-5d00faeaccd1.png)


#### R-squared: 0.978<br>MSE:  0.0021<br>RMSE:  0.001<br>
Interpretation:

Having a R-Squared of 97.8% shows our explained variance. We could say that this model accounts for 97.8% of the variance in consumer sentiment. Having a mean squared error of 0.0021 shows how close to the slope we truly are and the root mean squared error is our standard deviation of these residual errors. In other words this model is very accurate. There could be cause for concern of overfitting, but given this is a decision tree based model this is a reduced risk.  

## Conclusion:<br>

Overall, I see that consumer sentiment is is predicted by the use of the independent variables chosen through EIA, Inflation index, and Bureau of Labor Statistics. Having a F-statistic being so substantially low confirms this. However, given that the assumptions of a parametric test are a concern I chose to include a Random Forest Regressor model to involve nonparametric methods. This resulted in an R-Squared of 97.9% compared to the Multiple Linear Regression that was 81%. I conclude that a nonparametric model is best given the distribution of the dependent variable and the variances of the independent variables.


### Please see Two-Way ANOVA for brief exploration into interactions between Consumer Sentiment, Gas Price, and Inflation Factors.

[Two-Way ANOVA](https://github.com/stiznan/stiznan/blob/main/Energy%20Cost%2C%20Inflation%2C%20Consumer%20Sentiment/Two-way%20ANOVA.ipynb)<br>
![image](https://user-images.githubusercontent.com/70237462/145618839-e62e2b24-ec1f-446a-987d-a6b184cf216b.png)

### Tools Used:
1 Excel<br>
2. Python<br>
- Libraries Include:
    - Pandas
    - Numpy
    - Seaborn
    - Matplotlib
    - Scikit Learn
    - Scipy
    - Statsmodels
    - Random
    - Requests
    
