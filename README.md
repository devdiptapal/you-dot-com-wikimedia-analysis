# you-dot-com-wikimedia-analysis
Analysis for wikimedia data


Our analysis focuses on analyzing a Wikimedia clicks dataset that tells you where users are navigating from and to, and the number of people in a given month that are going from one link to another.

The technical implementation of the project is that we:

  1. Data Engineering Set up a SQLite database for the english dump for a specific month (in this case December 2023’s english dataset)

  2. Sampling We sample the dataset to 1% to be able to easily process through pandas since we want to focus on the analysis. Our assumption is that 1% is representative of the database.

  3. Analysis:
      Columns
      prev: The title of the Wikipedia article the user visited before the current one, or a special value indicating the source type (example 'other-search' may mean other search engines like Google, external of wikipedia).
      curr: The title of the current Wikipedia article the user visited.
      type: The type of referrer source. Possible values include:
      link: The user followed an internal link from another Wikipedia article.
      external: The user came from an external site (e.g., search engine).
      other: Other types of referrers.n: The number of occurrences of this (prev, curr, type) combination during the month.
