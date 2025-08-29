# 🎬 IMDB 2024 Project

## 📌 Project Overview
This project scrapes movie data from IMDB using **Selenium**, cleans the data with **Pandas**, and visualizes insights using **Streamlit**.  
The project also integrates **SQL queries** to analyze the cleaned dataset.

---

## 📂 Dataset Details
The cleaned dataset is saved as:
- `cleaned_master.csv`

### Columns in the dataset:
- `MovieName` → Movie title  
- `Genre` → Movie genre(s)  
- `Rating` → IMDB rating  
- `Votes` → Number of votes  
- `Duration_min` → Duration in minutes  

---

## ⚡ Technologies Used
- **Python**
- **Selenium** (for web scraping)  
- **Pandas** (for data cleaning & processing)  
- **Matplotlib / Seaborn** (for visualization)  
- **Streamlit** (for interactive dashboard)  
- **SQL** (for queries and analysis)

---

## 🛠️ Project Workflow
1. **Scraping** → Collected data from IMDB using Selenium  
2. **Data Cleaning** → Removed duplicates, handled missing values (`None` for missing duration)  
3. **Visualization** → Created charts and insights using Streamlit  
4. **SQL Queries** → Performed analytical queries for insights  

---

## 📊 SQL Queries Used
```sql
-- 1. Top-rated movies (by Rating)
SELECT MovieName, Genre, Rating
FROM cleaned_imdb
ORDER BY Rating DESC
LIMIT 10;

-- 2. Most popular movies (by Votes)
SELECT MovieName, Genre, Votes
FROM cleaned_imdb
ORDER BY Votes DESC
LIMIT 10;

-- 3. Movies with duration above average
SELECT MovieName, Duration_min
FROM cleaned_imdb
WHERE Duration_min > (SELECT AVG(Duration_min) FROM cleaned_imdb);

-- 4. Count of movies per genre
SELECT Genre, COUNT(*) AS TotalMovies
FROM cleaned_imdb
GROUP BY Genre;

-- 5. Average rating per genre
SELECT Genre, AVG(Rating) AS AvgRating
FROM cleaned_imdb
GROUP BY Genre;

-- 6. Movies with Rating above 8 and more than 100,000 votes
SELECT MovieName, Rating, Votes
FROM cleaned_imdb
WHERE Rating > 8 AND Votes > 100000;

-- 7. Shortest and Longest Movies
SELECT MovieName, Duration_min
FROM cleaned_imdb
ORDER BY Duration_min ASC
LIMIT 1;

SELECT MovieName, Duration_min
FROM cleaned_imdb
ORDER BY Duration_min DESC
LIMIT 1;
```

---

## 🚀 How to Run the Project
### 1️⃣ Clone the Repository
```bash
git clone https://github.com/yourusername/IMDB_2024_Project.git
cd IMDB_2024_Project
```

### 2️⃣ Install Dependencies
```bash
pip install -r requirements.txt
```

### 3️⃣ Run Streamlit Dashboard
```bash
streamlit run app.py
```

---

## 📷 Sample Outputs
- **Bar Chart** → Average duration by genre  
- **Top Movies** → Sorted by Rating and Votes  
- **Genre Analysis** → Count and Average Rating  

---

## 📌 Project Structure
```
IMDB_2024_Project/
│── app.py                # Streamlit dashboard file
│── scraper.py            # Selenium scraping script
│── cleaner.py            # Data cleaning script
│── cleaned_master.csv    # Final cleaned dataset
│── requirements.txt      # Python dependencies
│── README.md             # Project documentation
```

---

## 👨‍💻 Author
- **Lokesh Madhavan**
