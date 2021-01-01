---
title: "Forecast of Norwegian Car Sales - Holt-Winters Model"
theme: default
layout: default
parent: Projects
nav_order: 4
---

### The model and data that I used...<br>
Here, I have applied a linear Holt-Winters forecasting model to Norwegian car sales data found on [Kaggle](https://www.kaggle.com/dmi3kno/newcarsalesnorway). The data consists of sales from 2007 to the first month of 2017. I have forecasted expected quantity of cars sold for the year of 2016 and compared these values to actual sales.

### Initial model<br>

![initialHolt](https://user-images.githubusercontent.com/76073032/103423520-a3b48500-4b6c-11eb-8366-eee452666d53.png)<br>
I first created an initial model and used solver to find coefficients for each period (month). <br>

### Forecasting<br>

![Holtscreenshot](https://user-images.githubusercontent.com/76073032/103423589-10c81a80-4b6d-11eb-81b5-4e4b1e7a8e44.png)<br>
Then, I used the coefficients to find the optimal alpha, beta, and gamma. I ran solver to find the lowest sum of squared error possible. 

### Comparing the Forecast to Actual Sales<br>

![iforecasttable](https://user-images.githubusercontent.com/76073032/103423696-a5cb1380-4b6d-11eb-849f-4edeb2e07c78.png)<br>
Here is a table comparing my forecasted values and the actual quantity of vehicles sold. 

### Forecast <br>
![cars_graph](https://user-images.githubusercontent.com/76073032/103423349-baa6a780-4b6b-11eb-837b-501690f26501.png) <br>

### Reasons for Discrepencies<br>

The model appears fairly accurate with the exception of 2nd period (February). The forecasted sales for that month appear lower than actual sales. A possible explanation for this is that in Norway, the most popular cars sold are either Hybrid or electric (according to the Kaggle description). In this month, [the Norwegian oil and gas industry plummeted](https://www.bbc.com/news/business-35318236), which could explain the higher sales in cars (people were purchasing more fuel efficient cars, focused on the future). 

Additionally, the most recent previous period (2015) features a dip in February. The most recent period is most heavily weighted and this could be pulling the forecast for February down. 
