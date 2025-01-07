
# Data Preparation

### 1. Remove existing ids in the collections
#### The action is performed using the following console command
``` javascript
db.artists.updateMany({},{$unset:{"id":""}});

db.tracks.updateMany({},{$unset:{"id":""}});

db.tracks.updateMany({},{$unset:{"id_artists":""}});
```

### 2. Create indexes
#### - Delays were detected in the execution of queries due to the large size of the database. Considering the [MongoDB documentation](https://www.mongodb.com/blog/post/performance-best-practices-indexing-es), it would be advisable to create indexes on the keys of both collections.

# Queries and Data Analysis
### - Define the number of documents in each collection. 
#### The action is performed using the following console command
``` json
//Count in tracks:  
[{"$count": "tracks"}] 

//Count in artists:
[{"$count": "artists"}]
```

### - Shortest and longest track recorded. First track without ordering (Working from tracks). 
```json
[ 
  { 
    $group: { 
    _id: null, 
    firstTrack: { 
    $first: "$name" 
    }, 
    minDurationRecorded: { 
            $min: "$duration_ms" 
        }, 
        maxRecordedDuration: { 
            $max: "$duration_ms" 
      } 
    } 
  } 
]
```
### - Convert release dates from 'string' format to ISODate (Working from tracks collection).
#### 1. Check if all elements contain a date
```json
[ 
  { 
    $match: { 
      release_date: { 
        $exists: false 
      } 
    } 
  } 
] 
```
#### 2. Transform data
```json
[ 
  { 
    $set: { 
      release_date_ISO: { 
        $cond: { 
          if: { 
            $eq: [ 
              { 
                $type: "$release_date" 
              }, 
              "string" 
            ] 
          }, 
          then: { 
            $dateFromString: { 
              dateString: "$release_date" 
            } 
          }, 
          else: { 
            $dateFromString: { 
              dateString: { 
                $concat: [ 
                  { 
                    $toString: "$release_date" 
                  }, 
                  "-01-01" 
                ] 
              } 
            } 
          } 
        } 
      } 
    } 
  } 
] 
```
### - Find songs from a specific year, in this case, the year 2000 (Working from tracks collection).
```json
// The previous pipeline was used, adding a filter to obtain a specific year. A collection with the data can also be created and searched.
[ 
  { 
    $set: { 
      release_date_ISO: { 
        $cond: { 
          if: { 
            $eq: [ 
              { 
                $type: "$release_date" 
              }, 
              "string" 
            ] 
          }, 
          then: { 
            $dateFromString: { 
              dateString: "$release_date" 
            } 
          }, 
          else: { 
            $dateFromString: { 
              dateString: { 
                $concat: [ 
                  { 
                    $toString: "$release_date" 
                  }, 
                  "-01-01" 
                ] 
              } 
            } 
          } 
        } 
      } 
    } 
  }, 
  { 
    $match: { 
      release_date_ISO: { 
        $gte: ISODate( 
          "2000-01-01T00:00:00.000+00:00" 
        ), 
        $lt: ISODate( 
          "2001-01-01T00:00:00.000+00:00" 
        ) 
      } 
    } 
  }, 
  { 
    $project: { 
      release_date_ISO: 1, 
      name: 1, 
      _id: 0 
    } 
  } 
]
```

### - Identify which artists have the most songs, whether in singles or collaborations (Working from tracks collection).
```json
[ 
  { 
    $unwind: { 
      path: "$artists" 
    } 
  }, 
  { 
    $group: { 
      _id: "$artists", 
      songCount: { 
        $sum: 1 
      } 
    } 
  }, 
  { 
    $sort: { 
      songCount: -1 
    } 
  }, 
  { 
    $limit: 5 
  }, 
  { 
    $replaceWith: { 
      artistName: "$_id", 
      songCount: "$songCount" 
    } 
  } 
] 
```
### - Define the top 5 artists with the most followers and highest popularity (Worked from artists collection).
```json
[ 
  { 
    $sort: { 
      popularity: -1, 
      followers: -1 
    } 
  }, 
  { 
    $limit: 5 
  }, 
  { 
    $project: { 
      _id: 0, 
      Name: "$name", 
      Popularity: "$popularity", 
      Followers: "$followers", 
      MainGenre: { 
        $arrayElemAt: ["$genres", 0] 
      } 
    } 
  } 
]
```

### - Check if there is a high presence of explicit content songs in the market (Working from tracks collection).
```json
[ 
  { 
    $lookup: { 
      from: "artists", 
      localField: "artists", 
      foreignField: "name", 
      as: "ArtistInfo" 
    } 
  }, 
  { 
    $addFields: { 
      AveragePopularity: { 
        $avg: "$ArtistInfo.popularity" 
      } 
    } 
  }, 
  { 
    $group: { 
      _id: "$explicit", 
      averagePopularity: { 
        $avg: "$AveragePopularity" 
      }, 
      songCount: { 
        $sum: 1 
      } 
    } 
  }, 
  { 
    $project: { 
      _id: 0, 
      averagePopularity: 1, 
      songCount: 1, 
      Type: { 
        $cond: { 
          if: { 
            $eq: ["$_id", true] 
          }, 
          then: "Explicit", 
          else: "Non-explicit" 
        } 
      } 
    } 
  } 
]
```
### - Analyze the top 5 most popular songs (Working from tracks collection)
```json
[ 
  { 
    $sort: { 
      popularity: -1 
    } 
  }, 
  { 
    $limit: 5 
  }, 
  { 
    $project: { 
      popularity: 1, 
      name: 1, 
      artists: 1, 
      explicit: 1 
    } 
  } 
] 
```

### -  Identify a song with high potential to be a hit based on danceability and popularity (Working from tracks collection).
```json
[ 
  { 
    $project: { 
      name: 1, 
      danceability: 1, 
      popularity: 1, 
      posibleHit: { 
        $and: [ 
          { 
            $gt: ["$danceability", 0.7] 
          }, 
          { 
            $gt: ["$popularity", 95] 
          } 
        ] 
      } 
    } 
  }, 
  { 
    $match: { 
      posibleHit: true 
    } 
  } 
]
```
### - Songs can have different durations, so it would be interesting to classify them and see how they are distributed (Working from tracks collection). 

```json
// The $push operator is used since there might be multiple songs with the same name but different artists. 
[ 
  { 
    $bucket: { 
      groupBy: "$duration_ms", 
      boundaries: [0, 100001, 300001, Infinity], 
      output: { 
        count: { 
          $sum: 1 
        }, 
        songList: { 
          $push: "$name" 
        } 
      } 
    } 
  }, 
  { 
    $addFields: { 
      category: { 
        $cond: { 
          if: { 
            $eq: ["$_id", 0] 
          }, 
          then: "Short Duration", 
          else: { 
            $cond: { 
              if: { 
                $eq: ["$_id", 100001] 
              }, 
              then: "Medium Duration", 
              else: "Long Duration" 
            } 
          } 
        } 
      } 
    } 
  } 
] 
```

### - Check if there is any collaboration between two music legends: Queen and David Bowie (Working from the tracks collection).
 
```json
[ 
  { 
    $match: { 
      artists: { 
        $all: ["Queen", "David Bowie"] 
      } 
    } 
  }, 
  { 
    $project: { 
      _id: 0, 
      name: 1, 
      artists: 1 
    } 
  }, 
  { 
    $limit: 1 
  } 
] 
```

### - Create a new collection called "genres" that will store information related to the music genres collected in the database (Worked from the artists collection).

```json
[ 
  { 
    $unwind: { 
      path: "$genres", 
      preserveNullAndEmptyArrays: true 
    } 
  }, 
  { 
    $group: { 
      _id: { 
        $ifNull: ["$genres", "No Genre"] 
      }, 
      artistCount: { 
        $sum: 1 
      }, 
      genreFollowers: { 
        $sum: "$followers" 
      }, 
      averagePopularity: { 
        $avg: "$popularity" 
      } 
    } 
  }, 
  { 
    $out: "genres" 
  } 
]
```

### - Find out which genres have the most followers (Working in the new genres database).

```json
[{$sort: {genreFollowers: -1}}, {$limit: 3}]
```