# 🎵 **Spotify Dataset Analysis (1921-2020)**  

## 📖 **Introducción**  
Este proyecto analiza el **Spotify Dataset 1921-2020 (600k+ Tracks)** utilizando **MongoDB**. A través de consultas avanzadas en NoSQL, se extrajeron insights relevantes de dos colecciones principales: **artistas** y **canciones**.  

### 🌐 **Fuente de datos:**  
El dataset fue extraído del repositorio público de Yamac Eren Ay en [Kaggle](https://www.kaggle.com/).  
📥 Puedes acceder al dataset [aquí](https://www.kaggle.com/datasets/yamaerenay/spotify-dataset-19212020-600k-tracks?select=dict_artists.json).  

---

## 🗂️ **Estructura del Dataset**  

### 🎤 **Colección: `artists`**  
Contiene información de **1,162,095 artistas** y sus métricas clave:  
- `id`: Identificación del artista.  
- `followers`: Número de seguidores.  
- `genres`: Géneros asociados al artista.  
- `name`: Nombre del artista.  
- `popularity`: Popularidad (rango de 0 a 100).  

### 🎵 **Colección: `tracks`**  
Incluye datos de **586,672 pistas** y sus características de audio:  
- `id`: Identificación de la pista.  
- `name`: Nombre de la pista.  
- `popularity`: Popularidad (rango de 0 a 100).  
- `duration_ms`: Duración en milisegundos.  
- `explicit`: Contenido explícito (true/false).  
- `artists`: Artistas que participaron en la pista.  
- `release_date`: Fecha de lanzamiento.  
- Métricas como `danceability`, `energy`, `valence`, etc. (rango de 0 a 1).  

---

## ⚙️ **Pasos del Proyecto**  

### **1️⃣ Preparación de los Datos**  
- Transformación del dataset original de CSV a JSON.  
- Carga en MongoDB, creación de índices y eliminación de IDs redundantes.  

### **2️⃣ Queries y Análisis**  
- Uso de operadores avanzados como `$match`, `$group`, `$unwind`, `$bucket`, y `$arrayElemAt`.  
- Normalización de fechas con `$dateFromString`.  
- Optimización de consultas mediante índices.  

---

## 📊 **Conclusiones Principales**  

### 🎧 **Tendencias Musicales**  
- **El género pop domina** en popularidad.  
- Canciones de **duración media** reflejan preferencias del mercado.  
- **Canciones explícitas**, aunque minoritarias, tienen una mayor popularidad promedio.  

### 🌟 **Artistas Destacados**  
- **Justin Bieber** y **Drake** lideran en popularidad y seguidores.  

### 🔄 **Reorganización de Datos**  
- Creación de una colección específica para géneros musicales, mostrando la versatilidad analítica de MongoDB.  

---

## 🛠️ **Herramientas Utilizadas**  
- **[MongoDB](https://www.mongodb.com/)**  
- **[Python](https://www.python.org/)**  

---

## 🎯 **Propósito del Proyecto**  
Este análisis muestra cómo un manejo efectivo de datos puede proporcionar información valiosa sobre tendencias y patrones en la industria musical.  
