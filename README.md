Analysis focuses on analyzing a Wikimedia clicks dataset that tells you where users are navigating from and to, and the number of people in a given month that are going from one link to another.The technical implementation of the project is that we:

Prerequisites
- Download data from --> to the data/ folder
(https://dumps.wikimedia.org/other/clickstream/2023-12/clickstream-enwiki-2023-12.tsv.gz)
- Set up virtual environment / venv folder

Exploratory questions

- [Scope] the exploration for December 2023’s english dataset for wikipedia clickstream
- [Exploratory Data Analysis] We have wiki clickstream data for articles that users are navigating to and from, we want to understand our data and the basic facts from Exploratory Data Analysis
- [Article exploration] Interesting to profile the top 10 links that users are navigating from, to and the transitions users are making
- [Category exploration] Then, we try to map these links to categories and find the top category transitions for Wikipedia categories
- [Predict Category] We now Predict the next category from previous category with reasonable accuracy


Blueprint for analysis
__

- Data Engineering 
  - Set up a SQLite database for the english dump for a specific month (in this case December 2023’s english dataset)
  - Most easiest set up for a database within Python for us to work through
  - Random Sampling We sample the dataset to 10% to be able to easily process through pandas since we want to focus on the analysis. Our assumption is that 10% is representative of the database.
  - If we find distribution similar, let’s work with the sample, since it will cover larger trends in any case.


Analysis
- Understand Summary Statistics
- Article level analysis (too granular, but we can eyeball the top articles users are navigating to)
  - Understand top 10 articles previous articles users navigate from
  - Understand top 10 articles current articles users navigate to
  - Understand top 10 transitions from previous → current 
- Map articles into predefined categories (using a mapper)
  - Understand the top 10  trends within each bucket
  - Understand concentration of transitions between categories
- Quantile analysis
  - Understand deciles for 'n' in sample and overall data to generalize information
  - Understand composition of Outliers (for example n >=500 transitions from prev to curr) might reveal prevalence of user behavior (what users most likely transition from and to)

- Column Description
  - `prev` The title of the Wikipedia article the user visited before the current one, or a special value indicating the source type (example 'other-search' may mean other search engines like Google, external of wikipedia).
  - `curr` The title of the current Wikipedia article the user visited
  - `type` link : The user followed an internal link from another Wikipedia article. External: The user came from an external site (e.g., search engine). Other: Other types of referrer
  - `n` The number of occurrences of this (prev, curr, type) combination during the month.
  

- Prediction Task: Logistic Regression to predict the category of the next “Article” based on the previous article
  - In the ‘prev’ and ‘curr’ columns, find the top 50 occurring tokens and map them to category. Create a dictionary with category and mapping these tokens to specific categories
  - Used this category dictionary to map each article (in ‘prev’ to ‘curr’)  to a category to make the prediction task more bounded and useful
    - [Manual] Initially, had used a static list and was only able to map 4% of articles in ‘curr’, with tokenization, then sorting top 500 tokens and manually classifying it into categories, we’re able to improve it 6.5% (+2.5%) (possible to automate this step) 
  - Filtered for only mapped categories, removing “other” as its less useful
  - One hot encoded all “Wikipedia Categories” as inputs (e.g. Film, Search, Music, etc) to predict landing categories “Film, Music, etc”. Encoded categories to numerical values to use logistic regression (which works on numerical inputs and assumes linearity between response and inputs)
  - Added weights to the prediction to account for the number of links
  - The weight was normalized since we saw large values of “N” to not allow numerical anomalies
  - Used logistic regression to predict the output in terms of a confusion matrix
  - The confusion matrix had various performance parameters such as accuracy, precision and f1 score among others and we were able to get high precision/recall for predicting “Film articles” and were able to build out an equation to predict “Films” where the highest coefficient was that the previous article was “Search” 
  - We faced insufficiency of prediction due to df being sampled at 10% and then included the entire data in df to train the model with more data
  
  
  
  
  

	
