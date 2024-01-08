<h1> Customer Reviews / Text Analysis Project 

![Text-Analysis](https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/047592be-54f2-46ec-bfd9-1443c0090bd8)

# Background:

In this project, I am tasked to analyze customer reviews for a toy product on an e-commerce platform. The primary objective is to gain insights into the reception of the product among its purchasers. The analysis involves exploring sentiments, identifying common positive and negative aspects, and understanding the overall customer satisfaction. 

# Analysis:

### 1. Data preprocessing 

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

```php
# Create WordCloud for dataset and remove stopwords like 'https'
import nltk
from nltk.corpus import stopwords
from spacy.lang.en.stop_words import STOP_WORDS
from wordcloud import WordCloud
```

<img width="402" alt="Screenshot 2024-01-08 at 7 47 50 PM" src="https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/e3c34e75-39a5-4319-aeda-1d96d469549d">

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

<img width="398" alt="Screenshot 2024-01-08 at 7 47 25 PM" src="https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/1221f43b-5bc7-4361-9ef7-cdd2748020c7">


```php
# Search positive words that contains the product name
for i in positive[positive['translated'].str.contains("otter")]['translated'].iloc[0:7]:
    print(i,'\n')
```

<img width="759" alt="Screenshot 2024-01-08 at 7 47 05 PM" src="https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/e4bb7ed6-ba22-4341-8a85-c4a30ff34ac9">


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

<img width="402" alt="Screenshot 2024-01-08 at 7 46 42 PM" src="https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/90fd6dd1-1505-4a30-8a7a-4e2991357e63">


```php
# Search negative words that contains the product name 
for i in negative[negative['translated'].str.contains("otter")]['translated'].iloc[0:7]:
    print(i,'\n')
```

<img width="740" alt="Screenshot 2024-01-08 at 7 46 19 PM" src="https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/48d22efe-0423-44cd-8e51-6d4921c1acaf">


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

### 6. Distribution of Score

```php
import plotly.express as px
fig = px.histogram(df, x="Rating")
fig.update_traces(marker_color="turquoise",marker_line_color='rgb(8,48,107)',
                  marker_line_width=1.5)
fig.update_layout(title_text='Stars Rating Score')
fig.show()
```

<img width="688" alt="Screenshot 2024-01-08 at 7 48 51 PM" src="https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/349d36dc-da18-4593-9751-682310ba7ebc">



<img width="836" alt="Screenshot 2024-01-08 at 7 45 56 PM" src="https://github.com/phanhoangminh99/Text-Analysis/assets/115093313/3f37b725-537e-4a04-8cba-f8ff312cfdef">


# Conclusion: 

- Overall, the product sentiment is positive with the majority of customers giving the product a 5-star rating. This is a good sign, as it means that customers are generally satisfied with the product. However, there is still some negative sentiment, with the most common negative words being "price", "noise", and "functionality". This suggests that customers are concerned about the price of the product, the noise it makes, and its functionality, so it is important to identify and address the areas where customers are having problems.

