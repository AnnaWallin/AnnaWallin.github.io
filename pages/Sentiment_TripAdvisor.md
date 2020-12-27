---
title: "Word Cloud of Trip Advisor Reviews - Sentiment Analysis"
theme: default
layout: default
parent: Projects
nav_order: 3
---
Here we have a quick word cloud generated from Trip Advisor Reviews. The data is from [Kaggle](https://www.kaggle.com/andrewmvd/trip-advisor-hotel-reviews).

```
#Import necessary packages
!pip install pandas
import pandas as pd
!pip install matplotlib
import matplotlib.pyplot as plt
!pip install seaborn
import seaborn as sns
color = sns.color_palette()
%matplotlib inline
!pip install plotly
import plotly.offline as py
py.init_notebook_mode(connected=True)
import plotly.graph_objs as go
import plotly.tools as tls
import plotly.express as px 
```

```
#import data
from google.colab import files
uploaded = files.upload()
```

```
import io 

df = pd.read_csv(io.BytesIO(uploaded['tripadvisor_hotel_reviews.csv'])) 
print(df)
```

```
!pip install nltk
!pip install wordcloud
```

```
import nltk
from nltk.corpus import stopwords
import wordcloud
from wordcloud import WordCloud,STOPWORDS
# Create stopword list:
stopwords = set(STOPWORDS)
stopwords.update(["will","even","used","got","lot","room","hotel","really","n't","went","way","want"])
textt = " ".join(review for review in df.Review)
wordcloud = WordCloud(stopwords=stopwords,max_words=20,background_color="white").generate(textt)
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.savefig('wordcloud11.png')
plt.show()
```
