---
title: "Coronary Heart Disease - Logit Model"
theme: default
layout: default
parent: Projects
nav_order: 5
---

This project uses the Framingham Heart Study data found [here](https://www.kaggle.com/amanajmera1/framingham-heart-study-dataset). I model the probability of an individual becoming at risk for coronary heart disease over a ten year period. R was used.


### Summary Statistics and Setup...

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
Here we have the output summary statistics. We can see the means for each variable as well as the standard deviation. There is a pretty significant variation in age, education, and whether someone is a smoker.
![image](https://user-images.githubusercontent.com/76073032/104112777-521ca080-52b8-11eb-9690-9f9562ba86c1.png)


### Logit Model

```{r logit}
mydata$education<-factor(mydata$education) #make education categorical var 

#run logistic regression
logit_model<-glm(TenYearCHD~male+age+education+currentSmoker+cigsPerDay+BPMeds+ prevalentStroke+prevalentHyp+diabetes+totChol+sysBP+diaBP+BMI+heartRate+glucose, data=mydata,family = binomial(link = "logit"))


```

```{r view}
summary(logit_model)

```

Here we have the output of the logit model. 582 values were deleted due to missingness. Variables that have a p-value below 0.05 are significant. They are as follows: male, age, cigsPerDay, totChol, sysBP, and glucose.

The coefficients show the log odds for a one unit increase in the x (predictor) variable. For example, for a one unit increase in cigsPerDay, being categorized as at risk for coronary heart disease over a ten year period increases by 0.018387.

![image](https://user-images.githubusercontent.com/76073032/104112839-4e3d4e00-52b9-11eb-8135-ba3cda75f467.png)

### Calculating Confidence Interval
```{r CI}
#calc CIs
confint(logit_model)

```

![image](https://user-images.githubusercontent.com/76073032/104112963-9741d200-52ba-11eb-908e-64f72505880e.png)

```{r CI default}

```
![image](https://user-images.githubusercontent.com/76073032/104112987-dc660400-52ba-11eb-9510-eac86bbff4f5.png)

### Odds Ratios
```{r odds ratio }
exp(coef(logit_model))

```

![image](https://user-images.githubusercontent.com/76073032/104113017-1a632800-52bb-11eb-9f3a-ef810f18bebc.png)

```{r odds ratio table with CI}
exp(cbind(Odds_Ratio=coef(logit_model),confint(logit_model))) # 95 % CI

```
