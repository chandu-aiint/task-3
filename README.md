# 📊 Task 3 - College Feedback Sentiment Analysis

This project is part of the **Future Intern - Data Science & Analytics Internship**.  
It performs **sentiment analysis** on college course feedback and generates visual insights such as **average scores** and **sentiment distribution charts**.

---

## 🚀 Features
- 📂 Load and process CSV feedback data  
- 🧠 Perform sentiment analysis using **TextBlob**  
- 📊 Generate bar chart for **average scores per category**  
- 📈 Create sentiment distribution plots for each feedback column  
- 💾 Save processed CSV with sentiment labels  

---

## 📁 Project Structure
 #code

 import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from textblob import TextBlob

df = pd.read_csv(r"C:\Users\ramch\Downloads\sample_course_data_clean.csv")
numeric_columns = df.select_dtypes(include=['int64', 'float64']).columns.tolist()
text_columns = [col for col in df.columns if col not in numeric_columns]

print("Numeric columns:", numeric_columns)
print("Feedback text columns:", text_columns)

def get_sentiment(text):
    if pd.isna(text):
        return "Neutral"
    polarity = TextBlob(str(text)).sentiment.polarity
    if polarity > 0:
        return "Positive"
    elif polarity < 0:
        return "Negative"
    else:
        return "Neutral"

for col in text_columns:
    df[col + "_Sentiment"] = df[col].apply(get_sentiment)

avg_scores = df[numeric_columns].mean().reset_index()
avg_scores.columns = ['Category', 'Average_Score']

plt.figure(figsize=(8,5))
sns.barplot(x='Category', y='Average_Score', data=avg_scores, palette='coolwarm')
plt.title("Average Scores per Category")
plt.xticks(rotation=45)
plt.show()
S
for col in text_columns:
    plt.figure(figsize=(5,4))
    sns.countplot(x=col + "_Sentiment", data=df, palette="Set2")
    plt.title(f"Sentiment Distribution for {col}")
    plt.show()

df.to_csv(r"C:\Users\ramch\Downloads\course_feedback_with_sentiment.csv", index=False)
print("Analysis complete! File saved as course_feedback_with_sentiment.csv")


