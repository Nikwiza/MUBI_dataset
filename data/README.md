# Primeri kolekcija

# Inicijalne kolekcije

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

# Optimizovane kolekcije

## List_user

```json
{
  "_id": {
    "$oid": "666d0234862f1f9ef516d086"
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
  "user_eligible_for_trial": false,
  "user_has_payment_method": true,
  "has_img": true
}
```

## Mubi_list

```json
{
  "_id": {
    "$oid": "666d0217862f1f9ef51596cc"
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
  "has_img": true
}
```

## Rating_user

```json
{
  "_id": {
    "$oid": "666cfd64862f1f9ef50df5ab"
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
  "user_has_payment_method": false,
  "movie_title": "Pavee Lackeen: The Traveller Girl",
  "movie_has_img": true,
  "movie_popularity": 1,
  "movie_release_year": 2005
}
```