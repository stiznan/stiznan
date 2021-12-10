### Consumer Sentiment levers - Is inflation the true culprit of United States fear for the economy?


🎓 Practicum I:
   This practicum is to determine if inflation in the United States is a driving factor of the consumer sentiment drop in recent months. By reviewing monthly data going back as far as 2001 I was able to gain insights into what is a driving force of consumer sentiment in the United States using both multiple regresssion, two-way anova, and predictions in random forest regression. 
   
   It begins by taking the assumption we hear and see in media sources today. The Energy Information Administration has an API that allows the pulling of historical data relating to energy prices of many differnt types. In the case of this project I chose natural gas and residential electricty prices.
  

 
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
 ![image](https://user-images.githubusercontent.com/70237462/145616056-479c2d94-9b45-45ab-8aff-0e7422276ca2.png)





