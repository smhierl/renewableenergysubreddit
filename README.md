# Project 3 - Web APIs & NLP
# Comparing Language Between Renewable Energy and Energy Subreddits
Faced with a rapidly changing climate and declining fossil fuel reserves, the shift from traditional fossil fuels to renewable sources of energy (including wind, solar, hydro, geothermal, and bioenergy) becomes increasingly important year to year. The language surrounding this shift, as observed in subreddits r/RenewableEnergy and r/Energy, is reflective of its complex nature. The following analysis of the each subreddit and the comparison between the two subreddits aims to potentially provide insights on the following:
- Document the current language and public sentiment surrounding each topic.
- Identify linguistic overlap and differences, potentially identifying language or strategies to be used towards bridging the two subjects and ultimately strengthening support for Renewable Energy.


## I. Introduction
The United States has increased renewable energy production 42% from 2010 to 2020, accounting for 21% of all energy use in 2020 ([*source*](https://www.c2es.org/content/renewable-energy/)).

![alt text](https://www.eia.gov/todayinenergy/images/2021.07.28/main.svg)
([*source*](https://www.eia.gov/todayinenergy/detail.php?id=48896))

Driven by "concern for climate change and support for environmental, social, and governance considerations," this trend is forecasted to continue at accelerated rates in the years to come ([*source*](https://www2.deloitte.com/us/en/pages/energy-and-resources/articles/renewable-energy-outlook.html)). 

The Renewable Energy subreddit (r/RenewableEnergy/) reflects the evolving conversation surrounding this topic, including studies and new developments or milestones achieved around the globe. In comparison, the broader Energy subreddit (r/Energy/) covers "all things Energy related, how we use energy now, and how we will use it in the future," including conversations surrounding natural gas, coal, and fossil fuels, among others ([*source*](https://www.reddit.com/r/energy/)).


## II. Method
### 01: Data Collection
Data was gathered using Pushshift's Reddit API, drawing 1000 of the most recent (at time of gathering) posts from https://www.reddit.com/r/RenewableEnergy/ and https://www.reddit.com/r/Energy/.

The raw text files were saved, to be processed in the next stage.


### 02: Data Processing
While initially title text and self text from each reddit post were drawn from the raw data, the self text column was then dropped due to lack of values (83.3% of posts in our selection did not have self text). Therefore only the title text was saved to the final data set.

The data set was then split between subreddits and saved as seperate files for Renewable Energy and Energy.

### 03: Exploratory Data Analysis
In this section, additional columns were generated for each data set to reflect length of title as measured by character count and word count. The distribution of these lengths was then compared between the subreddits, as was average character count per word.

An additional column was also generated to reflect sentiment score of each title, on a scale of -1 to 1. The means and distribution of sentiment scores were then compared against each other.

Finally, the top 15 most frequently occuring words were identified for each subreddit, and cross compared to label their frequency in the counterpoint subreddit. These distributions were plotted against each other, and the most frequent words that overlapped between both subreddits were identified in a seperate list.


### 04: Production Model
For this analysis, the two seperate data sets were combined into one, with a column of dummy values to indicate whether the post came from Renewable Energy (value of 1) or Energy (value of 0). X (title) and y (subreddit dummy) of our model were defined, then divided into train test values.

The baseline model was determined to have a 50.025% accuracy rate, by which to compare the performance of both production models.

Lemmatizer and Stem functions were defined to be used in GridSearch, in comparison to the standard tokenizer for the Count Vectorizer function.

Two production models were then developed, with parameters determined via grid searching.
- Logistic Regression: The cross val score of the best performing model was 0.6897, and the best parameters as follows: max iteration of 1000 (LR), the 'lbfgs' solver (LR), binary as true (CV), max df of 1 (CV), min df of 0 (CV), ngram range of (1,2) (CV), and utilizing the stemming tokenizer (CV).
- Random Forest: The cross val score of the best performing model was 0.7011 and the best parameters as follows: no max depth (RF), automatically determined (sqrt(n_features)) max features (RF), binary as false (CV), max df of 1 (CV), min df of 2 (CV), ngram range of (1,2) (CV), and utilizing the stemming tokenizer (CV).

*LR for logistic regression, RF for random forest, and CV for count vectorizer.

We can determine that Random Forest seems to be the slightly better performing model (higher cross val score and higher rates in the classification report), but visualizations (including confusion matrices, probability distributions, and ROC curves) were developed to further compare both models.


## III. Data Dictionary

The following variables were included in the final model.

|Feature|Type|Description|
|---|---|---|
|**title**|*string*|all|Title text of each reddit post.| 
|**subreddit**|*int*|Dummy value indicating which subreddit a post was drawn from. Takes a value of 1 for Renewable Energy and 0 for Energy.|
    
    
## IV. Conclusions
- Overall, there appeared to be significant overlap in the language used between subreddits. This limited the accuracy rate of our model's predictions, but may be a positive indicator of shared interests and common language between the two.
- The slightly higher sentiment score for Renewable Energy (0.1451 vs 0.0934) may be indicative of a positive perception of Renerwable Energy generally. This positive association could be leveraged for further funding or support.
- A more successful model could be developed through using a greater volume or more robust sampling of text (e.g. self text) from both subreddits, as well as testing additional models and parameters.
- If this analysis was conducted at future dates, as a time series analysis, it could be used to document and potentially forecast upcoming trends or shifts in either sector.
