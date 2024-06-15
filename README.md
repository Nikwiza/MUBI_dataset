# MUBI Dataset Analysis

This project analyzes a dataset from MUBI, a curated streaming service for independent, international, classic, and arthouse films. The analysis is performed using a Jupyter notebook, and includes steps for loading, cleaning, and visualizing the data. Additionally, the project includes code for querying a MongoDB database containing the same dataset.

## Getting Started

### Prerequisites

- Python 3.11+
- Jupyter Notebook
- MongoDB

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Nikwiza/MUBI_dataset.git
   cd MUBI-Dataset
   ```

2. Create a virtual environment and activate it::
   ```bash
    python -m venv venv
    source venv/bin/activate   # On Windows use `venv\Scripts\activate`
   ```

3. Install the required packages:
   ```bash
    pip install -r requirements.txt
   ```

4. Ensure MongoDB is installed and running on your machine. Follow the instructions on the MongoDB official website if needed.



# Dataset

The dataset consists of multiple CSV files:

    movies.csv: Contains information about the movies.
    ratings.csv: Contains user ratings for the movies.
    users.csv: Contains information about the users.

## Logical Schema

The logical schema of the dataset is represented in the image below:

## Jupyter Notebook

The Jupyter notebook MUBI_Analysis.ipynb contains the following sections:

- Data Loading: Load the CSV files into pandas DataFrames.
- Data Cleaning: Perform data cleaning and preprocessing.
- Exploratory Data Analysis (EDA): Generate visualizations and perform basic statistical analysis.
- Advanced Analysis: Implement advanced analysis techniques (e.g., machine learning models).

## MongoDB Queries

The mongo_queries.py script contains code to perform various queries on the MongoDB database containing the MUBI dataset. This includes:

- Inserting data from CSV files into MongoDB collections.
- Querying the database to retrieve specific information.
- Aggregating data to perform complex analyses.


## How to Run

1. Run the Jupyter Notebook:
   ```bash
    jupyter notebook notebooks/MUBI_Analysis.ipynb
   ```

2. Run MongoDB Queries:
   ```bash
    python mongodb/mongo_queries.py 
   ```
# Initial data

### mubi_list

```json
{
  "_id": {
    "$oid": "666c83e9862f1f9ef5da4650"
  },
  "user_id": 85981819,
  "list_id": 1969,
  "list_title": "250 Favourite Films",
  "list_movie_number": 250,
  "list_update_timestamp_utc": "2019-11-26 03:20:17",
  "list_creation_timestamp_utc": "2009-12-18 13:04:48",
  "list_followers": 23,
  "list_url": "http://mubi.com/lists/250-favourite-films",
  "list_comments": 5,
  "list_description": "<p>In a loose order, but an order nonetheless.</p>",
  "list_cover_image_url": "https://assets.mubicdn.net/images/film/115/image-w1280.jpg?1527166442",
  "list_first_image_url": "https://assets.mubicdn.net/images/film/115/image-w320.jpg?1527166442",
  "list_second_image_url": "https://assets.mubicdn.net/images/film/3664/image-w320.jpg?1546228811",
  "list_third_image_url": "https://assets.mubicdn.net/images/film/187/image-w320.jpg?1552467202"
}
```

### Mubi_list_user

```json
{
  "_id": {
    "$oid": "666c8418862f1f9ef5db8008"
  },
  "user_id": 85981819,
  "list_id": 1969,
  "list_update_date_utc": {
    "$date": "2019-11-26T00:00:00.000Z"
  },
  "list_creation_date_utc": {
    "$date": "2009-12-18T00:00:00.000Z"
  },
  "user_trialist": true,
  "user_subscriber": true,
  "user_avatar_image_url": "https://assets.mubicdn.net/images/avatars/74983/images-w150.jpg?1523895214",
  "user_eligible_for_trial": false,
  "user_has_payment_method": true
}
```

### Mubi_movie

```json
{
  "_id": {
    "$oid": "666c8435862f1f9ef5dcb9c0"
  },
  "movie_id": 1,
  "movie_title": "La Antena",
  "movie_release_year": 2007,
  "movie_url": "http://mubi.com/films/la-antena",
  "movie_title_language": "en",
  "movie_popularity": 105,
  "movie_image_url": "https://images.mubicdn.net/images/film/1/cache-7927-1581389497/image-w1280.jpg",
  "director_id": 131,
  "director_name": "Esteban Sapir",
  "director_url": "http://mubi.com/cast/esteban-sapir"
}
```

### Ratings_user

```json
{
  "_id": {
    "$oid": "666c84c2862f1f9ef5e02ed0"
  },
  "movie_id": 1066,
  "rating_id": 15610495,
  "rating_url": "http://mubi.com/films/pavee-lackeen-the-traveller-girl/ratings/15610495",
  "rating_score": 3,
  "rating_timestamp_utc": "2017-06-10 12:38:33",
  "critic_likes": 0,
  "critic_comments": 0,
  "user_id": 41579158,
  "user_trialist": false,
  "user_subscriber": false,
  "user_eligible_for_trial": true,
  "user_has_payment_method": false
}
```

### Mugi_rating

```json
{
  "_id": {
    "$oid": "666c99f1862f1f9ef5feb354"
  },
  "user_id": 41579158,
  "rating_date_utc": {
    "$date": "2017-06-10T00:00:00.000Z"
  },
  "user_trialist": false,
  "user_subscriber": false,
  "user_avatar_image_url": "https://assets.mubicdn.net/images/avatars/74283/images-w150.jpg?1523895155",
  "user_eligible_for_trial": true,
  "user_has_payment_method": false
}
```
