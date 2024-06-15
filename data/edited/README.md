# Queries

## Queries used before optimization:

![Untitled](Queries%20b6194e343c554fa1879a17a6f0dec324/Untitled.png)

1. Question: Which type of user creates the most ratings?

Time: 1.515s

```jsx
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
  }
])
```

1. Question: Are movies watched by premium users more popular?

Time: /

```jsx
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

```jsx
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

```jsx
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

```jsx

db.lists_user.aggregate([
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

```jsx
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

```jsx
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

```jsx
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

```jsx
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

```jsx
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

### Indexes tried before optimization

```jsx
db.rating_user.createIndex({ movie_id: 1 });

db.mubi_movie.createIndex({ movie_id: 1 });

db.rating_user.createIndex({
user_subscriber: 1,
user_eligible_for_trial: 1,
user_has_payment_method: 1
});

db.mubi_movie.createIndex({ movie_release_year: 1 });
```

The indexes didn’t give a satisfactory result.

## Queries used after optimization

1. Question: Which type of user creates the most ratings?

Time: 1.482s

```jsx
db.rating_user.aggregate([
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
  }
])
```

1. Question: Are movies watched by premium users more popular?

Time: 4.279s

```jsx
db.rating_user.aggregate([
  {
    $group: {
      _id: {
        user_subscriber: "$user_subscriber",
        user_eligible_for_trial: "$user_eligible_for_trial",
        user_has_payment_method: "$user_has_payment_method"
      },
      average_movie_popularity: { $avg: "$movie_popularity" }
    }
  }
])
```

1. Question: Which generation of movies do the premium users like to watch?

Time: 4.655s

```jsx
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
    $group: {
      _id: "$movie_release_year",
      movies_watched: { $sum: 1 }
    }
  },
  {
    $sort: { _id: 1 }
  }
])

```

1. Question: What year is home to the most popular movies?

Time: 0.451s

```jsx
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
Time: 

```jsx

db.list_user.aggregate([
  {
    "$lookup": {
      "from": "mubi_list",
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
      "_id": "has_img",
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

```jsx
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
    "$sort": { "totalLikes": -1 }  
  },
  {
    "$limit": 1 
  },
  {
    "$project": {
      "movie_id": "$_id",
      "movie_title": "movie_title",
      "totalLikes": 1
    }
  }
])
```

1. Question: Which movie has the most liked positive critique?

Time: 58s

```jsx
db.ratings_user.aggregate([
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
    "$sort": { "totalLikes": -1 }  
  },
  {
    "$limit": 1 
  },
  {
    "$project": {
      "movie_title": "$movie_title",
      "totalLikes": 1
    }
  }
])
```

1. Question: What time of day do most people write reviews?

Time: 1.684s

```jsx
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

```jsx
var averageLength = db.mubi_list.aggregate([
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

db.lists_user.aggregate([
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
      "from": "rating_user",
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

```jsx
db.rating_user.aggregate([
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

### Indexes used after collection changes

```jsx
db.mubi_list.createIndex({ movie_id: 1 });

db.rating_user.createIndex({ movie_release_year: 1 });

db.rating_user.createIndex({ movie_popularity: 1 });

db.mubi_list.createIndex({
user_subscriber: 1,
user_eligible_for_trial: 1,
user_has_payment_method: 1
});
```