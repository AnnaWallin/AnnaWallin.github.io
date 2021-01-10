---
title: "Coronary Heart Disease - Logit Model"
theme: default
layout: default
parent: Projects
nav_order: 5
---

This project uses the Framingham Heart Study data found [here](https://www.kaggle.com/amanajmera1/framingham-heart-study-dataset). I model the probability of an individual becoming at risk for coronary heart disease over a ten year period. R was used.




```
{r install, message = FALSE}

library(readr)
```

```{r}
mydata<-read_csv("HeartDiseaseOG.csv") #load in data
#binary response variable is TenYearCHD
#Predictor variables are: male, age, education, currentSmoker,cigsPerDay,BPMeds, prevalentStroke,prevalentHyp,diabetes, totChol, sysBP, diaBP, BMI, heartRate, glucose
#education has a value of 1 to 4 depending on if the respondent did some high school, high school or GED, some college or vocational school, or college

summary(mydata) #summary statistics of the data set
sapply(mydata,sd) #view standard deviations
```

