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

![MovieLang](https://user-images.githubusercontent.com/76073032/102957852-2a000580-44a1-11eb-87b8-c177fa84191c.png)

**Fig 0** Shows most common languages across streaming services

The goal of this project was to examine overall prevalence of film genre as well as their breakdown on different streaming services. This data and related visualiztions can help users to optimize their subscription choices. For example, if an individual is interested in watching comedies, they could opt for the platform which has the highest proportion of comedies. The secondary goal was to examine ratings data from two popular sites: IMDb and Rotten Tomatoes.

In order to work with the data, there are several libraries that were imported. They include pandas, numpy, matplotlib, and seaborn. These are commonly found in the data science world and are useful to manipulate data and then to visualize it. It is then necessary to read in the CSV file and create a dataframe to work with. Finally, examining null values is essential. In fig. 1, there is a breakdown of the null values within the dataset. Interestingly, Rotten Tomatoes appears to have many more nulls than IMDb. The value for Rotten Tomatoes (11,586 nulls) appeared quite large and was double checked via sorting, binary coding the empty values, and then counting them. In order to aptly compare IMDb and Rotten Tomatoes scoring sites, they were filtered to exclude any nulls.

Additionally, the genre column features multiple entries for a single movie, which were separated by commas. This was "cleaned up" by splitting by comma. The explode method used accounts for missing values. This created a separate entry for each genre, which was linked according to an ID. Then, graphs were created that visualized the overall breakdown of films by genres (fig. 2-fig. 6). Countplot was used in each case and percentages, which denote genre/movie, were added to the top of each bar. Each movie can have multiple genre tags, so the total percentage can add up to above 100%. The bars are listed in descending order, so that the most common genre comes first. Bar coloring is also consistent throughout (E.G. Drama is always magenta). Overall, dramas were the most common on all platforms, except for Disney Plus, which featured the most family films. Necessary columns were renamed (Disney + to Disney, Prime Video to Amazon), so that the code would run under different uses, without special cases like spacing and symbols. Seaborn FacetGrid was not used because the Netflix, Hulu, Disney Plus, and Prime Video (Amazon) data were binary coded with the possibility of films being available on multiple platforms. This made it difficult to employ this technique. Therefore, graphs were done individually without FacetGrid being employed.

Next, scatterplots (fig. 7-fig. 11) were created to show the breakdown of movie ratings overall and according to platform. While, as mentioned earlier, Rotten Tomatoes, has a higher average rating than IMDb, there are roughly positive correlations throughout. So, if a movie is rated highly on IMDb, it could well be likely rated on Rotten Tomatoes as well.

Overall, this project will help movie viewers to better understand their streaming options and rating systems. They can learn which platforms have the most films, the most fims in their favorite genres, and learn more about two popular rating sites.

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
![null_column](https://user-images.githubusercontent.com/76073032/102958029-9e3aa900-44a1-11eb-9da1-6ce3df3088ca.png)
**Fig1** shows a breakdown of null values by column

```
#split Genres column by , 
df2=df.assign(MovieType=df['Genres'].str.split(",")).explode("MovieType")

#group data by their respective ID, year, movie type, and rating on IMDb
ratings2 = df2.groupby(['ID', 'Year', "MovieType"], as_index=False)['IMDb'].aggregate(np.mean) 
#ratings2.head() #view the grouped data

#check that MovieType column was created successfully and is present in the new dataframe
#df2.loc[0,'MovieType']
#df2.loc[0,:]
```

```
#overall genres popularity on all services TRY TO STANDARDIZE COLORS
dimensions_for_graph = (6, 5) #to avoid overlapping percents 
a4_dims = (11.7, 8.27) #set sizing, which will be used on the genre countplots
fig, ax = plt.subplots(figsize=a4_dims) #need to use matplotlib to correct for overlap issue with the percents 
desc_order = df2["MovieType"].value_counts().sort_values(ascending=False).index #order bars
sns.set_palette(["magenta","blue","pink","red","green","purple","black","Cyan","palegreen", "slateblue","orange","brown","lightcoral","darkgoldenrod","teal","wheat","deepskyblue","orchid","springgreen","yellow","cornflowerblue","gold","thistle","deeppink","tan","bisque","lime"])
ax2 = sns.countplot(df2.MovieType, order=desc_order)
plt.title("Distribution of Genres Overall on Streaming Services", size=26)
plt.xticks(rotation=90)
total2 = (len(df["Title"])) # THIS IS count OF all MOVIES IN DENOMINATOR
plt.xlabel("Movie Genre", size=18)
plt.ylabel("Count on Streaming Platform", size=18)
sns.set_style("darkgrid")

for p in ax2.patches:
    percentage2 = '{:.1f}%'.format(100 * p.get_height()/total2)
    x = p.get_x() + p.get_width()
    y = p.get_height()
    ax2.annotate(percentage2, (x, y),ha='center');

```
