# you-dot-com-wikimedia-analysis
Analysis for wikimedia data


Our analysis focuses on analyzing a Wikimedia clicks dataset that tells you where users are navigating from and to, and the number of people in a given month that are going from one link to another.

The technical implementation of the project is that we:

  1. Data Engineering Set up a SQLite database for the english dump for a specific month (in this case December 2023’s english dataset). We'll make it scalable to accept other month's data and work on them as well.

  2. Sampling We sample the dataset to 10% to be able to easily process through pandas since we want to focus on the analysis. Our assumption is that 1% is representative of the database.

  3. Analysis: We analyze the link changes on wikipedia for December 2023 in English

    Scope: The dataset includes for December 2023, the total link transitions for the language english

    a. prev: The title of the Wikipedia article the user visited before the current one, or a special value indicating the source type (example 'other-search' may mean other search engines like Google, external of wikipedia).
    b. curr: The title of the current Wikipedia article the user visited.
    c. type: The type of referrer source. Possible values include:
    d. link: The user followed an internal link from another Wikipedia article.
    e. external: The user came from an external site (e.g., search engine).
    f. other: Other types of referrers.n: The number of occurrences of this (prev, curr, type) combination during the month.

  4. Explorations

     
