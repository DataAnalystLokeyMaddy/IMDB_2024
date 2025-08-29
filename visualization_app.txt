import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

# Load CSV
file_path = r"C:\Users\lokey maddy\OneDrive\Desktop\VS MS Studio\IMDB_Project\master_IMDB_2024_cleaned.csv"
df = pd.read_csv(file_path)

# App title
st.title("🎬 IMDB 2024 Movies Dashboard")

# Sidebar filters
st.sidebar.header("Filter Movies")

# Genre filter
genres = df['Genre'].unique()
selected_genre = st.sidebar.multiselect("Select Genre(s):", genres, default=genres)

# Rating filter
min_rating = float(df['Rating'].min())
max_rating = float(df['Rating'].max())
rating_range = st.sidebar.slider("Rating Range:", min_value=min_rating, max_value=max_rating, value=(min_rating, max_rating))

# Votes filter
min_votes = int(df['Votes'].min())
max_votes = int(df['Votes'].max())
votes_range = st.sidebar.slider("Votes Range:", min_value=min_votes, max_value=max_votes, value=(min_votes, max_votes))

# Apply filters
filtered_df = df[
    (df['Genre'].isin(selected_genre)) &
    (df['Rating'] >= rating_range[0]) &
    (df['Rating'] <= rating_range[1]) &
    (df['Votes'] >= votes_range[0]) &
    (df['Votes'] <= votes_range[1])
]

# Show filtered table
st.subheader(f"Filtered Movies ({len(filtered_df)})")
st.dataframe(filtered_df)

# Top 10 by Rating
st.subheader("🏆 Top 10 Movies by Rating")
top_rated = filtered_df.sort_values(by='Rating', ascending=False).head(10)
st.dataframe(top_rated[['MovieName', 'Genre', 'Rating', 'Votes', 'Duration_min']], use_container_width=True)

# Top 10 by Votes
st.subheader("🔥 Top 10 Movies by Votes")
top_votes = filtered_df.sort_values(by='Votes', ascending=False).head(10)
st.table(top_votes[['MovieName', 'Genre', 'Rating', 'Votes', 'Duration_min']])

# Visualization: Average Rating by Genre
st.subheader("⭐ Average Rating by Genre")

avg_rating_genre = filtered_df.groupby("Genre")['Rating'].mean().sort_values(ascending=False)
fig, ax = plt.subplots(figsize=(10,6))  # Bigger, clearer figure
bars = ax.bar(avg_rating_genre.index, avg_rating_genre, color='dodgerblue', edgecolor='black', width=0.6)

ax.set_title("Average Movie Ratings by Genre", fontsize=16, fontweight='bold')
ax.set_xlabel("Genre", fontsize=12)
ax.set_ylabel("Average Rating", fontsize=12)
ax.set_xticklabels(avg_rating_genre.index, rotation=45, ha='right', fontsize=10)
ax.yaxis.grid(True)

# Add data labels
for bar in bars:
    height = bar.get_height()
    ax.annotate(f'{height:.2f}',
                xy=(bar.get_x() + bar.get_width()/2, height),
                xytext=(0, 5),  # 5 points vertical offset
                textcoords="offset points",
                ha='center', va='bottom', fontsize=9, color='black')

plt.tight_layout()
st.pyplot(fig)

# Visualization: Average Duration by Genre (minutes)
st.subheader("⏱ Average Duration by Genre (minutes)")

avg_duration_genre = filtered_df.groupby("Genre")['Duration_min'].mean().sort_values(ascending=False)
fig2, ax2 = plt.subplots(figsize=(10,6))  # Bigger, clearer figure
bars2 = ax2.bar(avg_duration_genre.index, avg_duration_genre, color='mediumseagreen', edgecolor='black', width=0.6)

ax2.set_title("Average Duration by Genre (minutes)", fontsize=16, fontweight='bold')
ax2.set_xlabel("Genre", fontsize=12)
ax2.set_ylabel("Duration (minutes)", fontsize=12)
ax2.set_xticklabels(avg_duration_genre.index, rotation=45, ha='right', fontsize=10)
ax2.yaxis.grid(True)

# Add data labels
for bar in bars2:
    height = bar.get_height()
    ax2.annotate(f'{height:.1f}',
                 xy=(bar.get_x() + bar.get_width()/2, height),
                 xytext=(0, 5),
                 textcoords="offset points",
                 ha='center', va='bottom', fontsize=9, color='black')

plt.tight_layout()
st.pyplot(fig2)


# Movie Quote
st.markdown(
    """
    ---
    <center>
    <span style='font-size:22px; color:#FFD700; font-style:italic;'>
    🎬 "Cinema is a mirror by which we often see ourselves." <br>
    <span style='font-size:18px; color:#E0E0E0;'>– Alejandro González Iñárritu</span>
    </span>
    </center>
    """,
    unsafe_allow_html=True
)
