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
### Summary Statistics...
Here we have the output summary statistics. We can see the means for each variable as well as the standard deviation. There is a pretty significant variation in age, education, and whether someone is a smoker.
![image](https://user-images.githubusercontent.com/76073032/104112777-521ca080-52b8-11eb-9690-9f9562ba86c1.png)


```{r logit}
mydata$education<-factor(mydata$education) #make education categorical var 

#run logistic regression
logit_model<-glm(TenYearCHD~male+age+education+currentSmoker+cigsPerDay+BPMeds+ prevalentStroke+prevalentHyp+diabetes+totChol+sysBP+diaBP+BMI+heartRate+glucose, data=mydata,family = binomial(link = "logit"))


```

```{r view}
summary(logit_model)

```

### Logit Model
