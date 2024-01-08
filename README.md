<h1> Customer Reviews / Text Analysis Project 

# Background:

In this project, I am tasked to analyze customer reviews for a toy product on an e-commerce platform. The primary objective is to gain insights into the reception of the product among its purchasers. The analysis involves exploring sentiments, identifying common positive and negative aspects, and understanding the overall customer satisfaction. 

# Analysis:

### 1. Data preprocessing 

```php
# Create WordCloud for dataset and remove stopwords like 'https'
import nltk
from nltk.corpus import stopwords
from spacy.lang.en.stop_words import STOP_WORDS
from wordcloud import WordCloud
```

```php
# Create stopword list:
STOP_WORDS.add('otter')
stopwords = set(list(STOP_WORDS) +list(stopwords.words()))
stopwords.update(["br", "href", 'https'])
stopwords.update(stopwords)
textt = " ".join(desc for desc in df.translated)
wordcloud = WordCloud(stopwords=stopwords).generate(textt)
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.savefig('wordcloud11.png')
plt.show()
```
<img width="399" alt="Screenshot 2024-01-08 at 8 06 52 PM" src="https://github.com/phanhoangminh99/Text-Analysis-/assets/115093313/b06dfb56-07ef-4cb3-84ff-5753206f5ebf">


### 2. Data cleaning and preparation  

```php
# Remove rating that is zero
df = df[df['Rating'] != 0]
#Creating Positive & Negative sentiments as +1 and -1 according to rating
df['sentiment'] = df['Rating'].apply(lambda rating : +1 if rating >= 4 else -1)
```

```php
# Create sentiment labels
positive = df[df['sentiment'] == 1]
negative = df[df['sentiment'] == -1]
```

### 3. Create a Wordcloud for positive sentiments

```php
# Create a WordCloud from the 'translated' column in the positive DataFrame 
stopwords.update(["br", "href","good","great", 'https']) 
pos = " ".join(review for review in positive.translated)
wordcloud2 = WordCloud(stopwords=stopwords).generate(pos)
plt.imshow(wordcloud2, interpolation='bilinear')
plt.axis("off")
plt.show()
```

<img width="402" alt="Screenshot 2024-01-08 at 8 07 10 PM" src="https://github.com/phanhoangminh99/Text-Analysis-/assets/115093313/bac93240-6db6-44fe-8a4d-fe443404b9d3">


```php
# Search positive words that contains the product name
for i in positive[positive['translated'].str.contains("otter")]['translated'].iloc[0:7]:
    print(i,'\n')
```

<img width="744" alt="Screenshot 2024-01-08 at 8 07 27 PM" src="https://github.com/phanhoangminh99/Text-Analysis-/assets/115093313/9730911b-de2f-49d6-b7bb-74b12666f2d5">


### 4. Create a Wordcloud for negative sentiments

```php
# WordCloud for negative samples
neg = " ".join(review for review in negative.translated)
wordcloud3 = WordCloud(stopwords=stopwords).generate(neg)
plt.imshow(wordcloud3, interpolation='bilinear')
plt.axis("off")
plt.savefig('wordcloud33.png')
plt.show()
```

<img width="396" alt="Screenshot 2024-01-08 at 8 07 41 PM" src="https://github.com/phanhoangminh99/Text-Analysis-/assets/115093313/68535aba-44b5-4d2e-b851-54b54bce7732">


```php
# Search negative words that contains the product name 
for i in negative[negative['translated'].str.contains("otter")]['translated'].iloc[0:7]:
    print(i,'\n')
```

<img width="747" alt="Screenshot 2024-01-08 at 8 08 03 PM" src="https://github.com/phanhoangminh99/Text-Analysis-/assets/115093313/ced0710c-3587-4333-8a82-03306364cbf2">


### 5. Distribution of Sentiments amongst samples

```php
#Create a histogram of sentiment distribution in the DataFrame

df['sentimentt'] = df['sentiment'].replace({-1 : 'negative'})
df['sentimentt'] = df['sentimentt'].replace({1 : 'positive'})
fig = px.histogram(df, x="sentimentt")
fig.update_traces(marker_color="indianred",marker_line_color='rgb(8,48,107)',
                  marker_line_width=1.5)
fig.update_layout(title_text='Product Sentiment')
fig.show()
```

<img width="694" alt="Screenshot 2024-01-08 at 8 08 38 PM" src="https://github.com/phanhoangminh99/Text-Analysis-/assets/115093313/a525cf86-2473-4488-8487-bb23102940e0">


### 6. Distribution of Score

```php
import plotly.express as px
fig = px.histogram(df, x="Rating")
fig.update_traces(marker_color="turquoise",marker_line_color='rgb(8,48,107)',
                  marker_line_width=1.5)
fig.update_layout(title_text='Stars Rating Score')
fig.show()
```

<img width="689" alt="Screenshot 2024-01-08 at 8 08 29 PM" src="https://github.com/phanhoangminh99/Text-Analysis-/assets/115093313/fb457538-f8b5-45d9-b800-f88893e6ac57">


# Conclusion: 

- Overall, the product sentiment is positive with the majority of customers giving the product a 5-star rating. This is a good sign, as it means that customers are generally satisfied with the product. However, there is still some negative sentiment, with the most common negative words being "price", "noise", and "functionality". This suggests that customers are concerned about the price of the product, the noise it makes, and its functionality, so it is important to identify and address the areas where customers are having problems.

