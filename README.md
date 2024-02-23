# Overview
The data, notebooks, and graphs included in this repo are used to gather, scrape, and analyze textual data and then build classification models that can accurately identify which subreddit a post comes from. The data has been gathered by accessing Reddit's API and scraping the 1000 most recent posts (as of 1/5/24) from [r/ADHD](https://www.reddit.com/r/ADHD/) and [r/autism](https://www.reddit.com/r/autism/). 
My review consists of 3 notebooks in this repo, all named according to the workflows present in them. I begin by scraping both subreddits and gathering recent posts' text and titles' text. In the 2nd notebook, I dive into EDA to better understand any quantifiable differences between the two subreddits' posts. I then move to using some natural language processing workflows via the `nltk` library to better understand differences in word count and frequency. Finally, in the 3rd notebook I build several classification models using `GridSearchCV` to narrow down the 4 best models from each grid search and then evaluate the performances of them by scoring their accuracy score. The accuracy score is the metric I largely rely on to evaluate the predictive strength of each model. In each notebook, I dive further into the techniques I utilized and provide explanations for my processes.


# Problem Statement
Both ADHD and Autism fall under the umbrella of neurodiversity, a framework that contains several different ways in which brains function and people interact with their surroundings. The fundamental insight from neurodiversity is that there is no single "correct" way for going about how a brain functions. ADHD and Autism are particularly intriguing since people diagnosed with one or both tend to share symptoms. Per the NIH, [scientific studies suggest that roughly 50-70% of individuals on the autism spectrum disorder share symptoms with ADHD](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8918663/#:~:text=According%20to%20the%20scientific%20literature,rate%20of%20comorbidity%20is%20intriguing.). Despite key differences being present, there are several similarities between the two disorders which makes it difficult to decipher between the two. 
A clinical psychology tech startup wants to understand the language people with both disorders use and examine if there are any subtle or major differences that can help determine which disorder they have. I have been employed to look into this issue and provide insight on techniques the startup can use to create models that can guess whether an individual has ADHD or autism.
As mentioned in the overview, I began examining this by looking at subreddits for both ADHD and autism. My logic behind this was that there must be keywords and/or phrases that posters in both groups use more or less often. A **big** assumption we have to make drawing data from Reddit, however, is that individuals commenting in both subreddits actually have either ADHD or autism. Once we make this assumption we can, via NLP, analyze the language content specific to both groups and create classification models that can help us predict which subreddit the post comes from. 


# Data
Data is scraped initially from [r/ADHD](https://www.reddit.com/r/ADHD/) and [r/autism](https://www.reddit.com/r/autism/). Please refer to the below data dictionaries.

#### Data Dictionary 1
|Feature|Type|Description|
|---|---|---|
|post|string|The actual body of text in a post by a redditor|
|title|string|The title of the post by a redditor|
|subreddit|string|The target variable, name of the subreddit the post is from|
|upvotes|integer|Number of upvotes from community members|
|downvotes|integer|Number of downvotes from community members|

#### Data Dictionary 2
|Feature|Type|Description|
|---|---|---|
|post_title|string|Concatenated string of text of the post and title separated by an empty string for count vectorizing purposes|
|subreddit|string|The target variable, name of the subreddit the post is from|


# Summarizing Analysis & Findings
1. After extracting the necessary information, I begin with EDA. I quantify the textual data into several variables including post and title word counts, sentiment scores of the posts, and how many "stopwords" are present. After looking at visuals and summary statistics, I conclude there are no major differences between the two subreddits in those regards.

2. I continue EDA to look at word counts. This proves to be insightful, since both subreddits' posts contain several unique words that have high frequency of occurrence. This supports my hypothesis (mentioned in the review) that posts in r/ADHD discuss more about medications and things synonymous with focus and attention. Meanwhile, posts in r/autism discuss more about social & daily situations living with autism. Because of this, I suggest that `TfidVectorizer` could be more promising for modeling purposes.

3. I create several classification models to determine which model can best predict which subreddit a post comes from. The logistic regression model using `TfidVectorizer` as the transformer proves to be most accurate. I conlude by sharing scores and mentioning that we can strengthen the model by utilizing `TfidVectorizer` since this focuses on both the frequency and ***importance*** of the words, something we noticed is extremely valuable during the EDA process.


# Recommendations to the Start-up
Online forums specific to the disorder of interest could be useful places to study language used and how it can help determine which disorder an individual may have. In this case in my subreddit study, there is a clear difference between words used in r/ADHD and r/autism and the classification model is made more accurate when examining the **relative frequency** of a word's occurrence vs. the absolute frequency. 

