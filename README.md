# PRODIGY_DS_04

## Sentiment Analysis of Social Media Data

### Description

This repository contains code and analysis for examining sentiment patterns in social media data to understand public opinion and attitudes towards specific topics or brands. The analysis uses Twitter data to perform sentiment analysis, visualize sentiment trends, and derive insights about public perception.

### Project Overview

The project includes:

- **Data Collection:** Fetching tweets related to a specific topic or brand using the Twitter API.
- **Sentiment Analysis:** Analyzing the sentiment of tweets to classify them as positive, negative, or neutral.
- **Data Visualization:** Visualizing sentiment trends over time and exploring sentiment distribution.
- **Insights:** Summarizing findings about public opinion and attitudes.

### Getting Started

To perform sentiment analysis and visualize sentiment patterns, follow these steps:

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/yourusername/social-media-sentiment-analysis.git
   ```

2. **Navigate to the Project Directory:**
   ```bash
   cd social-media-sentiment-analysis
   ```

3. **Install Dependencies:**
   ```bash
   pip install tweepy pandas numpy matplotlib seaborn textblob
   ```

4. **Set Up Twitter API Credentials:**
   Create a `.env` file in the root directory with your Twitter API credentials:
   ```
   TWITTER_API_KEY=your_api_key
   TWITTER_API_SECRET_KEY=your_api_secret_key
   TWITTER_ACCESS_TOKEN=your_access_token
   TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret
   ```

5. **Run the Sentiment Analysis Script:**
   ```bash
   python analyze_sentiment.py
   ```

6. **View Results:** The script generates visualizations and saves sentiment analysis results in the `results/` directory.

### Example Output

Here are some visualizations produced by the sentiment analysis script:

- **Sentiment Distribution Pie Chart**
- **Sentiment Trends Over Time**
- **Most Frequent Positive and Negative Words**

![Sentiment Distribution](results/sentiment_distribution.png)
![Sentiment Trends Over Time](results/sentiment_trends.png)
![Frequent Words](results/word_cloud.png)

### Key Findings

- **Sentiment Trends:** Identified how public sentiment towards a topic or brand has changed over time.
- **Public Opinion:** Analyzed the overall sentiment to understand public opinion and attitudes.
- **Insights:** Highlighted key themes in positive and negative sentiments.



### `analyze_sentiment.py` Script

Here is a basic example of the `analyze_sentiment.py` script:

```python
import tweepy
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from textblob import TextBlob
from wordcloud import WordCloud
import os
from dotenv import load_dotenv

# Load Twitter API credentials from .env file
load_dotenv()
api_key = os.getenv('TWITTER_API_KEY')
api_secret_key = os.getenv('TWITTER_API_SECRET_KEY')
access_token = os.getenv('TWITTER_ACCESS_TOKEN')
access_token_secret = os.getenv('TWITTER_ACCESS_TOKEN_SECRET')

# Authenticate to Twitter API
auth = tweepy.OAuth1UserHandler(api_key, api_secret_key, access_token, access_token_secret)
api = tweepy.API(auth)

# Fetch tweets
query = 'YourTopicOrBrand'  # Replace with the topic or brand you are interested in
tweets = tweepy.Cursor(api.search_tweets, q=query, lang='en', tweet_mode='extended').items(1000)

# Create a DataFrame
data = pd.DataFrame([tweet.full_text for tweet in tweets], columns=['Tweet'])

# Perform sentiment analysis
data['Polarity'] = data['Tweet'].apply(lambda tweet: TextBlob(tweet).sentiment.polarity)
data['Sentiment'] = data['Polarity'].apply(lambda polarity: 'Positive' if polarity > 0 else ('Negative' if polarity < 0 else 'Neutral'))

# Plot Sentiment Distribution
plt.figure(figsize=(8, 6))
sns.countplot(x='Sentiment', data=data, palette='viridis')
plt.title('Sentiment Distribution')
plt.savefig('results/sentiment_distribution.png')
plt.show()

# Plot Sentiment Trends Over Time
data['Date'] = pd.to_datetime(data['Date'])
data.set_index('Date', inplace=True)
sentiment_trends = data.resample('D').Sentiment.value_counts().unstack().fillna(0)

plt.figure(figsize=(12, 6))
sentiment_trends.plot()
plt.title('Sentiment Trends Over Time')
plt.xlabel('Date')
plt.ylabel('Number of Tweets')
plt.legend(title='Sentiment')
plt.savefig('results/sentiment_trends.png')
plt.show()

# Generate Word Cloud for Positive and Negative Tweets
positive_words = ' '.join(tweet for tweet in data[data['Sentiment'] == 'Positive']['Tweet'])
negative_words = ' '.join(tweet for tweet in data[data['Sentiment'] == 'Negative']['Tweet'])

positive_wc = WordCloud(width=800, height=400, background_color='white').generate(positive_words)
negative_wc = WordCloud(width=800, height=400, background_color='black').generate(negative_words)

plt.figure(figsize=(12, 6))
plt.subplot(1, 2, 1)
plt.imshow(positive_wc, interpolation='bilinear')
plt.title('Positive Words Cloud')
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(negative_wc, interpolation='bilinear')
plt.title('Negative Words Cloud')
plt.axis('off')

plt.savefig('results/word_cloud.png')
plt.show()
```

Replace `YourTopicOrBrand` with the topic or brand you are analyzing. Ensure you have the necessary `.env` file with your Twitter API credentials.

### Full README Example

Here’s how your full README file might look with the added section:

```markdown
# Sentiment Analysis of Social Media Data

This repository contains code and analysis for examining sentiment patterns in social media data to understand public opinion and attitudes towards specific topics or brands. The analysis uses Twitter data to perform sentiment analysis, visualize sentiment trends, and derive insights about public perception.

## Project Overview

The project includes:

- **Data Collection:** Fetching tweets related to a specific topic or brand using the Twitter API.
- **Sentiment Analysis:** Analyzing the sentiment of tweets to classify them as positive, negative, or neutral.
- **Data Visualization:** Visualizing sentiment trends over time and exploring sentiment distribution.
- **Insights:** Summarizing findings about public opinion and attitudes.

## Getting Started

To perform sentiment analysis and visualize sentiment patterns, follow these steps:

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/yourusername/social-media-sentiment-analysis.git
   ```

2. **Navigate to the Project Directory:**
   ```bash
   cd social-media-sentiment-analysis
   ```

3. **Install Dependencies:**
   ```bash
   pip install tweepy pandas numpy matplotlib seaborn textblob
   ```

4. **Set Up Twitter API Credentials:**
   Create a `.env` file in the root directory with your Twitter API credentials:
   ```
   TWITTER_API_KEY=your_api_key
   TWITTER_API_SECRET_KEY=your_api_secret_key
   TWITTER_ACCESS_TOKEN=your_access_token
   TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret
   ```

5. **Run the Sentiment Analysis Script:**
   ```bash
   python analyze_sentiment.py
   ```

6. **View Results:** The script generates visualizations and saves sentiment analysis results in the `results/` directory.

## Example Output

Here are some visualizations produced by the sentiment analysis script:

- **Sentiment Distribution Pie Chart**
- **Sentiment Trends Over Time**
- **Most Frequent Positive and Negative Words**

![Sentiment Distribution](results/sentiment_distribution.png)
![Sentiment Trends Over Time](results/sentiment_trends.png)
![Frequent Words](results/word_cloud.png)

## Key Findings

- **Sentiment Trends:** Identified how public sentiment towards a topic or brand has changed over time.
- **Public Opinion:** Analyzed the overall sentiment to understand public opinion and attitudes.
- **Insights:** Highlighted key themes in positive and negative sentiments.

## `analyze_sentiment.py` Script

Here is a basic example of the `analyze_sentiment.py` script:

```python
import tweepy
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from textblob import TextBlob
from wordcloud import WordCloud
import os
from dotenv import load_dotenv

# Load Twitter API credentials from .env file
load_dotenv()
api_key = os.getenv('TWITTER_API_KEY')
api_secret_key = os.getenv('TWITTER_API_SECRET_KEY')
access_token = os.getenv('TWITTER_ACCESS_TOKEN')
access_token_secret = os.getenv('TWITTER_ACCESS_TOKEN_SECRET')

# Authenticate to Twitter API
auth = tweepy.OAuth1UserHandler(api_key, api_secret_key, access_token, access_token_secret)
api = tweepy.API(auth)

# Fetch tweets
query = 'YourTopicOrBrand'  # Replace with the topic or brand you are interested in
tweets = tweepy.Cursor(api.search
