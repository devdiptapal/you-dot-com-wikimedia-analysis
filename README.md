# you-dot-com-wikimedia-analysis
Analysis for wikimedia data


Our analysis focuses on analyzing a Wikimedia clicks dataset that tells you where users are navigating from and to, and the number of people in a given month that are going from one link to another.

The technical implementation of the project is that we:

  1. Data Engineering Set up a SQLite database for the english dump for a specific month (in this case December 2023’s english dataset). We'll make it scalable to accept other month's data and work on them as well.

  2. Sampling We sample the dataset to 10% to be able to easily process through pandas since we want to focus on the analysis. Our assumption is that 1% is representative of the database.

  3. Analysis: We analyze the link changes on wikipedia for December 2023 in English

    1. prev: The title of the Wikipedia article the user visited before the current one, or a special value indicating the source type (example 'other-search' may mean other search engines like Google, external of wikipedia).
    2. curr: The title of the current Wikipedia article the user visited.
    3. type: The type of referrer source. Possible values include:
    4. link: The user followed an internal link from another Wikipedia article.
    5. external: The user came from an external site (e.g., search engine).
    6. other: Other types of referrers.n: The number of occurrences of this (prev, curr, type) combination during the month.

  4. Prediction task
     
    1. We used Logreg to predict the article category based on previous article
    2. Used a predefined category list to map each link (previous or current) to a category to make the prediction task “useful”
    3. Any unmapped categories were defined as “other”
    4. Filtered for only mapped categories, removing “other” as its less useful
    5. Added weights to the prediction to account for the number of links
    6. The weight was normalized since we saw large values of “N” to not allow numerical anomalies
    7. Encoded categories to numerical values to use logistic regression
    8. Used logistic regression to predict the output in terms of a confusion matrix
    9. The confusion matrix had various performance parameters such as accuracy, precision and f1 score among others
    10. We faced insufficiency of prediction due to df being sampled at 10% and then further excluding categories where it was “other” as category or the previous and current links were pointing to each other

6. Scope: The dataset includes for December 2023, the total link transitions for the language english
  
  
       
