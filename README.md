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

### Mubi_rating

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

# Queries

1. Question: Which type of user creates the most ratings?

Time: 1.515s

```json
db.ratings.aggregate([
  {
    "$group": {
      "_id": {
        "user_trialist": "$user_trialist",
        "user_subscriber": "$user_subscriber",
        "user_eligible_for_trial": "$user_eligible_for_trial",
        "user_has_payment_method": "$user_has_payment_method"
      },
      "totalRatings": { "$sum": 1 },
       "totalCritics": {"$sum": {"$cond": [{ "$ne": [{ "$type": "$critic" }, "missing"] }, 1, 0] } }
    }
  },
  {
    "$project": {
      "_id": 0,
      "user_types": {
        "$objectToArray": "$_id"
      },
      "totalRatings": 1,
      "totalCritics": 1
    }
  },
  {
    "$sort": { "totalRatings": -1 }
```

1. Question: How related are the number of created lists and the number of ratings (Does more lists created equal more activity)?

Time: /

```json
db.mubi_list_user.aggregate([
  {
    $lookup: {
      from: "mubi_rating",
      localField: "user_id",
      foreignField: "user_id",
      as: "ratings"
    }
  },
  {
    $group: {
      _id: "$user_id",
      numberOfLists: { $sum: 1 }, // Count how many times user_id appears in mubi_list_user
      numberOfRatings: { $sum: { $size: "$ratings" } } // Count how many ratings this user has given
    }
  },
  {
    $sort: { numberOfLists: -1 } // Optionally sort by numberOfLists descending
  }
])

```

1. Question: Are movies watched by premium users more popular?

Time: /

```json
db.rating_user.aggregate([
  {
    $lookup: {
      from: "mubi_movie",
      localField: "movie_id",
      foreignField: "movie_id",
      as: "movie_info"
    }
  },
  {
    $unwind: "$movie_info"
  },
  {
    $group: {
      _id: {
        user_subscriber: "$user_subscriber",
        user_eligible_for_trial: "$user_eligible_for_trial",
        user_has_payment_method: "$user_has_payment_method"
      },
      average_movie_popularity: { $avg: "$movie_info.movie_popularity" }
    }
  }
])
```

1. Question: Which generation of movies do the premium users like to watch?

Time: /

```json
db.rating_user.aggregate([
  {
    $match: {
      $or: [
        { user_subscriber: true },
        { user_eligible_for_trial: true },
        { user_has_payment_method: true }
      ]
    }
  },
  {
    $lookup: {
      from: "mubi_movie",
      localField: "movie_id",
      foreignField: "movie_id",
      as: "movie_info"
    }
  },
  {
    $unwind: "$movie_info"
  },
  {
    $group: {
      _id: "$movie_info.movie_release_year",
      movies_watched: { $sum: 1 }
    }
  },
  {
    $sort: { _id: 1 }
  }
])

```

1. Question: What year is home to the most popular movies?

Time: 0.941s

After Index: 0.483s

```json
db.mubi_movie.aggregate([
  {
    $group: {
      _id: "$movie_release_year",
      average_popularity: { $avg: "$movie_popularity" }
    }
  },
  {
    $sort: { _id: 1 }
  }
])
```

1. Question: Does the way a users profile is setup translate into list popularity?
Time: /

```json

db.lists_user.aggregate([
  {
    "$lookup": {
      "from": "lists",
      "localField": "list_id",
      "foreignField": "list_id",
      "as": "list_details"
    }
  },
  {
    "$unwind": "$list_details"
  },
  {
    "$group": {
      "_id": {
        "avatar": { "$cond": { "if": { "$eq": ["$user_avatar_image_url", ""] }, "then": false, "else": true } },
        "cover": { "$cond": { "if": { "$eq": ["$user_cover_image_url", ""] }, "then": false, "else": true } }
      },
      "total_followers": { "$sum": "$list_details.list_followers" },
    }
  },
  {
    "$project": {
      "_id": 0,
      "user_avatar_image_url": "$_id.avatar",
      "user_cover_image_url": "$_id.cover",
      "total_followers": 1,
    }
  },
  {
    "$sort": { "user_avatar_image_url": 1, "user_cover_image_url": 1 }
  }
])
```

1. Question: Which movie has the most liked negative critique?

Time: 48s

```json
db.ratings.aggregate([
  {
    "$match": {
      "rating_score": { "$in": [1, 2] }  
    }
  },
  {
    "$group": {
      "_id": "$movie_id",
      "totalLikes": { "$sum": "$critic_likes" }
    }
  },
  {
    "$lookup": {
      "from": "movie",
      "localField": "_id",
      "foreignField": "movie_id",
      "as": "movie_info"
    }
  },
  {
    "$unwind": "$movie_info" 
  },
  {
    "$sort": { "totalLikes": -1 }  
  },
  {
    "$limit": 1 
  },
  {
    "$project": {
      "movie_id": "$_id",
      "movie_title": "$movie_info.movie_title",
      "totalLikes": 1
    }
  }
])
```

1. Question: Which movie has the most liked positive critique?

Time: 58s

```json
db.ratings.aggregate([
  {
    "$match": {
      "rating_score": { "$in": [4, 5] } 
    }
  },
  {
    "$group": {
      "_id": "$movie_id",
      "totalLikes": { "$sum": "$critic_likes" }
    }
  },
  {
    "$lookup": {
      "from": "movie",
      "localField": "_id",
      "foreignField": "movie_id",
      "as": "movie_info"
    }
  },
  {
    "$unwind": "$movie_info" 
  },
  {
    "$sort": { "totalLikes": -1 }  
  },
  {
    "$limit": 1 
  },
  {
    "$project": {
      "movie_id": "$_id",
      "movie_title": "$movie_info.movie_title",
      "totalLikes": 1
    }
  }
])
```

1. Question: What time of day do most people write reviews?

Time: 1.684s

```json
db.ratings.aggregate([
  {
    "$match": {
      "rating_timestamp_utc": { "$exists": true }
    }
  },
  {
    "$group": {
      "_id": { "$hour": { "$toDate": "$rating_timestamp_utc" } },
      "count": { "$sum": 1 }
    }
  },
  {
    "$sort": { "count": -1 }
  },
  {
    "$limit": 1
  }
])

```

1. Question: Do people who post lists with longer ratings give reviews more commonly?
Time: 0.017s

```json
var averageLength = db.lists.aggregate([
  {
    "$match": {
      "list_description": { "$exists": true, "$type": "string" }
    }
  },
  {
    "$group": {
      "_id": null,
      "averageLength": { "$avg": { "$strLenCP": "$list_description" } }
    }
  }
]).toArray()[0].averageLength;

db.lists.aggregate([
  {
    "$match": {
      "list_description": { "$exists": true, "$type": "string" }
    }
  },
  {
    "$addFields": {
      "descriptionLength": { "$strLenCP": "$list_description" }
    }
  },
  {
    "$match": {
      "descriptionLength": { "$gt": averageLength }
    }
  },
  {
    "$lookup": {
      "from": "reviews",
      "localField": "user_id",
      "foreignField": "user_id",
      "as": "user_reviews"
    }
  },
  {
    "$project": {
      "user_id": 1,
      "numberOfReviews": { "$size": "$user_reviews" }
    }
  }
])
```

1. Question: What is the percentage of critique likes that we get from different types of users; nije u procentima nego prosecno koliko lajkova koji tip

Time: 2.735s

```json
db.ratings_user.aggregate([
  {
    "$facet": {
      "trialists": [
        {
          "$match": {
            "critic_likes": { "$gt": 0 },
            "user_trialist": true
          }
        },
        {
          "$group": {
            "_id": null,
            "averageLikes": { "$avg": "$critic_likes" }
          }
        }
      ],
      "subscribers": [
        {
          "$match": {
            "critic_likes": { "$gt": 0 },
            "user_subscriber": true
          }
        },
        {
          "$group": {
            "_id": null,
            "averageLikes": { "$avg": "$critic_likes" }
          }
        }
      ],
      "eligible_for_trial": [
        {
          "$match": {
            "critic_likes": { "$gt": 0 },
            "user_eligible_for_trial": true
          }
        },
        {
          "$group": {
            "_id": null,
            "averageLikes": { "$avg": "$critic_likes" }
          }
        }
      ],
      "has_payment_method": [
        {
          "$match": {
            "critic_likes": { "$gt": 0 },
            "user_has_payment_method": true
          }
        },
        {
          "$group": {
            "_id": null,
            "averageLikes": { "$avg": "$critic_likes" }
          }
        }
      ]
    }
  }
])
```

### Indexes

// Index on movie_id in both collections to speed up the $lookup stage
db.rating_user.createIndex({ movie_id: 1 });
db.mubi_movie.createIndex({ movie_id: 1 });

// Compound index on the fields used for grouping in rating_user
db.rating_user.createIndex({
user_subscriber: 1,
user_eligible_for_trial: 1,
user_has_payment_method: 1
});

// Index on the fields used in the match condition
db.rating_user.createIndex({
user_subscriber: 1,
user_eligible_for_trial: 1,
user_has_payment_method: 1
});

// Index on movie_id in both collections for the $lookup stage
db.rating_user.createIndex({ movie_id: 1 });
db.mubi_movie.createIndex({ movie_id: 1 });

// Optional: Index on movie_release_year in the mubi_movie collection to speed up the grouping
db.mubi_movie.createIndex({ movie_release_year: 1 });

Question: Which type of users create the most ratings, and do they attach critiques to those ratings?

Creating index:

```json
db.ratings.createIndex({
  user_trialist: 1,
  user_subscriber: 1,
  user_eligible_for_trial: 1,
  user_has_payment_method: 1
})
```

Time before: 1.515s

Time after: 1.384s

Question1: Which movie has the most liked negative critique?

Question2: Which movie has the most liked positive critique?

Creating index:

```json
db.ratings.createIndex({
  rating_score: 1,
  movie_id: 1,
  critic_likes: 1
})
db.movie.createIndex({
  movie_id: 1
})

```

Time before1: 48s

Time after1: 0.108s

Time before2: 58s

Time after2: 0.592s

![Untitled](Indeksi%20e416f2022ede4a88b4447c7e05dca5e5/Untitled.png)
