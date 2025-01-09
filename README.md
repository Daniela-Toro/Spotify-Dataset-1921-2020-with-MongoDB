# 🎵 **Spotify Dataset Analysis (1921-2020)**  

## 📖 **Introduction**  
This project analyzes the **Spotify Dataset 1921-2020 (600k+ Tracks)** using **MongoDB**. Through advanced NoSQL queries, relevant insights were extracted from two main collections: **artists** and **tracks**.  

### 🌐 **Data Source:**  
The dataset was obtained from Yamac Eren Ay's public repository on [Kaggle](https://www.kaggle.com/).  
📥 You can access the dataset [here](https://www.kaggle.com/datasets/yamaerenay/spotify-dataset-19212020-600k-tracks?select=dict_artists.json).  

---

## 🗂️ **Dataset Structure**  

### 🎤 **Collection: `artists`**  
Contains information on **1,162,095 artists** and their key metrics:  
- `id`: Artist ID.  
- `followers`: Number of followers.  
- `genres`: Genres associated with the artist.  
- `name`: Artist's name.  
- `popularity`: Popularity (range 0 to 100).  

### 🎵 **Collection: `tracks`**  
Includes data on **586,672 tracks** and their audio features:  
- `id`: Track ID.  
- `name`: Track name.  
- `popularity`: Popularity (range 0 to 100).  
- `duration_ms`: Duration in milliseconds.  
- `explicit`: Explicit content (true/false).  
- `artists`: Artists involved in the track.  
- `release_date`: Release date.  
- Metrics such as `danceability`, `energy`, `valence`, etc. (range 0 to 1).  

---

## ⚙️ **Project Steps**  

### **1️⃣ Data Preparation**  
- Transformation of the original dataset from CSV to JSON.  
- Upload to MongoDB, index creation, and removal of redundant IDs.  

### **2️⃣ Queries and Analysis**  
- Use of advanced operators like `$match`, `$group`, `$unwind`, `$bucket`, and `$arrayElemAt`.  
- Date normalization using `$dateFromString`.  
- Query optimization through indexing.  

---

## 📊 **Key Findings**  

### 🎧 **Musical Trends**  
- **Pop genre dominates** in popularity.  
- Tracks with **average duration** reflect market preferences.  
- **Explicit tracks**, though fewer in number, have higher average popularity.  

### 🌟 **Top Artists**  
- **Justin Bieber** and **Drake** lead in popularity and followers.  

### 🔄 **Data Restructuring**  
- Creation of a dedicated collection for musical genres, showcasing MongoDB's analytical versatility.  

---

## 🛠️ **Tools Used**  
- **[MongoDB](https://www.mongodb.com/)**  
- **[Python](https://www.python.org/)**  

---

## 🎯 **Project Purpose**  
This analysis demonstrates how effective data management can provide valuable insights into trends and patterns in the music industry.  
