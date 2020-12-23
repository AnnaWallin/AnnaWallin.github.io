---
title: "Streaming Platform - Python Data Visualization"
theme: default
layout: default
parent: Projects
nav_order: 2
---

For this project, I used Python to visualize data that was found on [Kaggle](https://www.kaggle.com/ruchi798/movies-on-netflix-prime-video-hulu-and-disney). I wanted to look at prevalence of genre across streaming platforms (Prime Video, Netflix, Hulu, Disney+) and explore the data.

### Introduction and Summary
The data set for this project was found on Kaggle (link can be accessed in the works cited). It is a part of the public domain and is of considerable size. There are 16,744 entries (movies). The data set was last updated on 5/22/2020 and was created via web scraping. The columns found in the data set include an ID; film title; year of release; age rating (such as 18+); IMDb score; Rotten Tomatoes score; whether it is available on Netflix, Hulu, Amazon Prime, or Disney Plus; the director, Genre(s), runtime, country(ies) the film is from, and finally, the language(s) of the film. The data types include categorical (binary), numerical, and nominal data.

Preliminary examination of the data shows some interesting findings. The oldest film in the data set is a short 13 minute French film from 1902, titled A Trip to the Moon, which can be found on Prime Video (Amazon). The bottom quartile of films is from 2000 and earlier, meaning that most films on these streaming platforms were made within the last two decades. This could be due to the relatively new nature of streaming TV (predisposed to have newer content); however exact reasoning would need to be explored further. Out of 16,744 films included in the data, only 147 are from 2020. English is the most common film language, followed by Spanish (Fig. 0). Additionally, aside from runtime and release year, the data includes information from two popular movie rating sites, IMDb and Rotten Tomatoes. IMDb employs a rating system on a 1-10 scale, while Rotten Tomatoes uses a percentage (a decimal from 0 to 1). However, Rotten Tomatoes has 11,586 missing values. When this is corrected for by only comparing values where there is a rating for both Rotton Tomatoes and IMDB, the scores are close with Rotten Tomatoes having an average of 0.654285 on a 0 to 1 scale and IMDb having an average of 6.375233 on a 1 to 10 scale (before correcting for this, the average on Rotten Tomatoes is 0.654285 and 5.902751 for IMDb).

```
#top 5 most common movie languages overall
df3=df.assign(Lang=df['Language'].str.split(",")).explode("Lang") #split by lang

df3.sort_values(by=['Lang']).head(5)
Lang=df3['Lang'].value_counts() #view top 5 lang
Dict_Lang2={"English": 13233, "Spanish": 872, "French": 799, "Hindi": 731,"German": 483} #dict of top 5 lang
sns.set_palette(["springgreen"]) #color
plt.bar(Dict_Lang2.keys(), Dict_Lang2.values()) #make vis
plt.title("Five Most Common Movie Languages", size = 18) #add title
plt.xlabel("Language", size=12) #label
plt.ylabel("Count", size=12) #label
```

```
#preliminary background snooping to gain a greater appreciation and understanding of the data.
#import to work with your data
import pandas as pd
import numpy as np
import matplotlib
from matplotlib import pyplot as plt
%matplotlib inline
import seaborn as sns

#load in your data
data_csv = pd.read_csv(r'C:\Users\FILEPATH\Movie_Services.csv')
#convert data to pandas data frame
df = pd.DataFrame(data_csv)
#gain a greater understanding of your data through some preliminary sleuthing
#list(df.columns)  #see column titles to see what data includes
#df.describe()  # see summary statistics
#df_2020=df[(df.Year == 2020)]  #filter data by year 2020
#df_2020.count(0)  #count column values - 147 films made in 2020

#make a data frame that has imdb and rotten tomatoes....account for nulls to compare avgs
df_handle_nulls_Ratings = df.groupby(['ID','Year','Title','Rotten Tomatoes'], as_index=False)['IMDb'].aggregate(np.mean) 
df_handle_nulls_Ratings.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
#df_handle_nulls_Ratings.describe() 

#look for null values
#df.isnull().sum().sum() # count of missing values overall across columns and rows
df.isnull().sum() #breakdown of missing values by column
```
