### Consumer Sentiment levers - Is inflation the true culprit of United States fear for the economy?


🎓 Practicum I:
   This practicum is to determine if inflation in the United States is a driving factor of the consumer sentiment drop in recent months. By reviewing monthly data going back as far as 2001 I was able to gain insights into what is a driving force of consumer sentiment in the United States using both multiple regresssion, two-way anova, and predictions in random forest regression. 
   
   It begins by taking the assumption we hear and see in media sources today. The Energy Information Administration has an API that allows the pulling of historical data relating to energy prices of many differnt types. In the case of this project I chose natural gas and residential electricty prices.
  
  ![alt text](https://github.com/stiznan/stiznan/blob/main/Energy%20Cost%2C%20Inflation%2C%20Consumer%20Sentiment/Images/EIA%20API%20Code.JPG?raw=false)
 

   
   
   I then pulled information from the Bureau of Labor Statistics for consumer prices, University of Michigan & Florida for their consumer sentiment survey to represent sentiment, and lastly historical inflation rates. Each of these data sources required wrangling in their own way. For example, for the EIA API this was pulling information using the Requests library and converting the information into a json and then transforming it into a CSV. For the others it was online information that was either downloaded as an Excel file or copied and placed in a CSV file in Excel. I then reworked the layouts of these Excel files and put them in such a way they could be loaded into python as a csv file. 
   
   Once these tables were created I needed to have a common index. I converted the dates, which were monthly, into date_time formats from pandas and joined them all into one complete table to begin the EDA, Multiple Regression, and ML process. 
  
   

