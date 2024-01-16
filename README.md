<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/c/c1/Steam_Logo.png" width="100" />
</p>
<p align="center">
    <h1 align="center">STEAM MARKET SENTIMENT PREDICTION</h1>
</p>
<p align="center">
    <em> Using NLP and classification models in order to predict sentiment of steam users towards a new game on the steam marketplace</em>
</p>
<p align="center">
	<img src="https://img.shields.io/github/last-commit/Mattcalcaterra/618_steam?style=default&color=0080ff" alt="last-commit">
	<img src="https://img.shields.io/github/languages/top/Mattcalcaterra/618_steam?style=default&color=0080ff" alt="repo-top-language">
	<img src="https://img.shields.io/github/languages/count/Mattcalcaterra/618_steam?style=default&color=0080ff" alt="repo-language-count">
<p>
<p align="center">
	<!-- default option, no dependency badges. -->
</p>
<hr>

##  Quick Links

> - [ Overview](#-overview)
> - [ Features](#-features)
> - [ Repository Structure](#-repository-structure)
> - [ Modules](#-modules)
> - [ Acknowledgments](#-acknowledgments)

---

##  Overview

*This project was completed for the final project for University of Michigan SI 618 in collaboration with [Mohammad Awad](https://github.com/mohammaw)*

### Motivation
Our project aims to analyze data of games listed in the Steam Marketplace in order to develop a
model which can predict the sentiment of customers, based on specific variables of a game.
The intention is that this model can be used to estimate whether a game looking to come to
market will be received positively by customers. Using Steam reviews as a source of data for
NLP is a common occurrence on websites such as Kaggle. Our project differs from those in
trying to link the sentiment of those analyses with variables which can be determined before the
launch of a game, such as genre, developer, and languages. We hope to create a model which
will allow developers and publishers to estimate the reception of their game before launching on
the Steam Marketplace.

### Data Sources
Our project used data from two primary sources: Steamspy API, and Steam Marketplace Review Scraping. The data for both of these sources was collected in a separate notebook from
our main project, and stored in a .csv file for access during the project. Due to the size of both of our dataset, this allowed us to save a significant amount of time, and reduce demand on the datasource websites.

#### Steamspy API

The primary datasource for our project was the steamspy API, which can be called using the
following url: https://steamspy.com/api.php
Steamspy API allows for multiple requests, however we were interested in gathering data on all
games in the Steam Marketplace so we used the `all` endpoint. This returns data for all games
in the Steam Marketplace which have owners data. 1000 entries are returned per page, so we
used a loop to call 1000 entries, then proceed to the next page until all games had been
returned.

We use the API to collect a dataset of all games in the Steam Marketplace, resulting in a
dataframe with 63,968 rows, or 63,968 games. There were also 20 variables contained in the
dataset. Our prediction model is based on 5 of these variables, however 18 of these variables
were kept for analysis purposes.

The following were the variables in our dataset:
- Appid - The games ID
- Name - The games name
- Developer - Developer of the game
- Publisher - Publisher of the game
- Score_rank -
- Positive - Number of positive reviews indicated by Steam
- Negative - Number of negative reviews indicated by Steam
- Userscore -
- Owners - Range of owners of the game
- Average_forever - Average player playtime
- Average_2weeks - Average player playtime in the last two weeks
- Median_forever - Median player playtime
- Median_2weeks - Median player playtime in the last two weeks
- Price - Current price of game
- Initialprice - Initial price of game
- Discount - Current discount
- Languages - Languages indicated on Steam listing
- Genre - Genres indicated on Steam listing
- Ccu - Current Concurrent Users
- Tags - Tags given to the game by players

Since we want to predict how a new game coming to market will be received by the player, our
model is built off of variables which will be known before a game releases. These variables
include:
- Developer
- Publisher
- Price
- Languages
- Genre

Since the data was significantly large, the dataset was saved as a .csv for easier access during
the project. This also eliminated the need to call the API repeatedly, which takes a lot of time.
Steam Marketplace Scraping

In order to acquire reviews for each game in the steam marketplace we scraped the reviews
from the steam marketplace using the following url format:
`https://store.steampowered.com/appreviews/<appid>?json=1`

We have opted for the data to be returned in json format, which was then converted into a
pandas dataframe. This was called for each appid in the Steamspy API dataframe, and 100
reviews were attempted to be collected for each game. If the game had less than 100 reviews,
all the reviews were returned.

Since only 100 reviews were able to be viewed at a time, we used a moving cursor in case more
than 100 reviews were wanted to be retrieved. The function to collect this data was based off
code by Medium used Andrew Muller:
https://andrew-muller.medium.com/scraping-steam-user-reviews-9a43f9e38c92

The primary variables of interest from this data source consisted mostly of `review` and `appid`.
Other variables were kept in the dataset for future analysis, however our project was only
interested in the individual reviews for the game and the appid which they corresponded to.
Once the review endpoint had been called for 100 reviews for each appid, our resulting
dataframe contained 1,336,828 rows. Due to this size (and time required to gather the data), the
data was saved as a compressed .csv file for easier data access in the rest of the project.

---

##  Features

- **Data Collection**
    - **Steamspy API**
        - Using the `requests` package, we were able to call the steamspy API and collect data on all games in the steam marketplace. This data was then saved as a .csv file for easier access during the project.
    - **Steam Marketplace Review Scraping**
        - Using the `requests` package, we were able to call the steam marketplace review scraping endpoint and collect reviews for each game in the steam marketplace. This data was then saved as a .csv file for easier access during the project.
- **Exploritory Data Analysis**
- **NLP**
    - **Sentiment Analysis**
        - Using the `VADER` package, we were able to generate sentiment scores for each review in our dataset. This allowed us to create a new variable, `sentiment`, which was used as our target variable in our model.
- **Regression Models**
    - **Linear Regression**
        - Using the `statsmodels` package, we were able to create a linear regression model which predicted the sentiment of a game based on the variables `developer`, `publisher`, `price`, `languages`, and `genre`.
    - **Random Forest Regression**
        - Using the `sklearn` package, we were able to create a random forest regression model which predicted the sentiment of a game based on the variables `developer`, `publisher`, `price`, `languages`, and `genre`.
    - **XGBoost Regression**
        - Using the `xgboost` package, we were able to create a XGBoost regression model which predicted the sentiment of a game based on the variables `developer`, `publisher`, `price`, `languages`, and `genre`.

---

##  Repository Structure

```sh
└── 618_steam/
    ├── data
    |   ├── README.txt
    |   ├── steam_review_data1.csv.gz
    |   ├── steam_review_sentiment.csv.gz
    |   └── steamspy_data.csv
    ├── src
    |   ├── analysis.ipynb
    |   └── data_compiling.ipynb
    └── project_summary.pdf
```

---

##  Modules

<details closed><summary>src</summary>

| File                                                                                                     | Summary                                                                                                                                                                                                                                |
| ---                                                                                                      | ---                                                                                                                                                                                                                                    |
| [analysis.ipynb](https://github.com/Mattcalcaterra/618_steam/blob/master/src\analysis.ipynb)             | Jupyter Notebook containing the analysis of the project. Contains exploratory analysis of steam data and steam reviews, generates sentiment analysis using NLP packages, establishes and regressor model in order to predict sentiment of steam games based on multiple variables|
| [data_compiling.ipynb](https://github.com/Mattcalcaterra/618_steam/blob/master/src\data_compiling.ipynb) | Jupyter Notebook which contains preliminary data exploration done for the project, as well as code which collects data from multiple end points into a .csv file for further use in the project|

</details>

---

##  Acknowledgments

- [Andrew Muller](https://andrew-muller.medium.com/scraping-steam-user-reviews-9a43f9e38c92)
- [Steamspy API](https://steamspy.com/api.php)
- [Steam Marketplace Review Scraping](https://store.steampowered.com/appreviews/<appid>?json=1)
- [Github Copilot](https://copilot.github.com/)

[**Return**](#-quick-links)

---
