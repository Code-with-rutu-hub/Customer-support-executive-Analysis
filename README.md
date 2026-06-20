# Customer Support Analytics Project
Customer Support Intelligence Dashboard & Future Ticket Prediction System

Power BI +SQL Project 


---

### Business Problem
A growing company receives thousands of customer support tickets every month through multiple channels such as email, chat, and calls. Management faces the following challenges:

- High ticket resolution time
- Increasing number of unresolved high-priority tickets 
- Poor visibility into agent performance
- Difficulty predicting future support workload
- Lack of centralized reporting system
- Customer satisfaction inconsistencies

---

### Project Overview  
This project dataset:

Dataset: enhanced_customer_support_data.csv

Main Columns:

- Column	
- Ticket_ID	
- Customer_Name	
- Issue_Category	
- Priority_Level	
- Ticket_Channel	
- Submission_Date	
- Resolution_Time_Hours	
- Assigned_Agent	
- Satisfaction_Score
  
The goal is to extract insights into:
- Stores customer support data in PostgreSQL
- Performs SQL-based business analysis
- Creates interactive dashboards with filters and slicers
- Uses Python machine learning to predict future ticket volume


---

### SQL Database

- Create Database
```
CREATE DATABASE customer_support_project;
```
- Create Table
```
CREATE TABLE support_tickets (
    ticket_id VARCHAR(20),
    customer_name VARCHAR(100),
    customer_email VARCHAR(150),
    ticket_subject TEXT,
    ticket_description TEXT,
    issue_category VARCHAR(100),
    priority_level VARCHAR(50),
    ticket_channel VARCHAR(50),
    submission_date DATE,
    resolution_time_hours NUMERIC,
    assigned_agent VARCHAR(100),
    satisfaction_score INTEGER
);
```

### SQL Analysis Queries
- Total Tickets
```
SELECT COUNT(*) AS total_tickets
FROM support_tickets;
```
- Tickets by Priority
```
SELECT priority_level,
       COUNT(*) AS total_tickets
FROM support_tickets
GROUP BY priority_level
ORDER BY total_tickets DESC;

```

- Average Resolution Time
```
SELECT ROUND(AVG(resolution_time_hours),2) AS avg_resolution_time
FROM support_tickets;
```
- Best Performing Agents
```
SELECT assigned_agent,
       ROUND(AVG(satisfaction_score),2) AS avg_rating,
       COUNT(ticket_id) AS resolved_tickets
FROM support_tickets
GROUP BY assigned_agent
ORDER BY avg_rating DESC;

```

- Monthly Ticket Trend
```
SELECT DATE_TRUNC('month', submission_date) AS month,
       COUNT(*) AS total_tickets
FROM support_tickets
GROUP BY month
ORDER BY month;

``` 
- High Priority Tickets
```
SELECT *
FROM support_tickets
WHERE priority_level = 'High';

```
- Channel Performance
```
SELECT ticket_channel,
       ROUND(AVG(resolution_time_hours),2) AS avg_resolution_time,
       ROUND(AVG(satisfaction_score),2) AS avg_satisfaction
FROM support_tickets
GROUP BY ticket_channel;

```

### Data Transformation  
Using **Power Query** and **DAX**, the data was transformed to calculate:  
- Total Tickets
```
Total Tickets = COUNT(support_tickets[ticket_id])

```
- Average Satisfaction
```
Avg Satisfaction = AVERAGE(support_tickets[satisfaction_score])

```
- Average Resolution Time
```
Avg Resolution Time = AVERAGE(support_tickets[resolution_time_hours])

```
- High Priority Tickets
```
High Priority Tickets =
CALCULATE(
    COUNT(support_tickets[ticket_id]),
    support_tickets[priority_level] = "High"
)
```



### Power BI Dashboard  

<img width="951" height="589" alt="image" src="https://github.com/user-attachments/assets/182eba9d-ba52-44e7-ab80-639bc90387e4" />

------

<img width="905" height="496" alt="image" src="https://github.com/user-attachments/assets/797f857a-289d-4480-ab0d-7e3323892b20" />

------

## Python Forcast

<img width="768" height="342" alt="image" src="https://github.com/user-attachments/assets/4f3edb27-e099-4b77-9892-c24ee73ffc3c" />

- Install Libraries
```
pip install pandas sqlalchemy psycopg2-binary scikit-learn matplotlib jupyter

```
- import
```
import pandas as pd

df = pd.read_csv("enhanced_customer_support_data.csv")
df.head()

```
- Convert Date Column
```
df['Submission_Date'] = pd.to_datetime(df['Submission_Date'])

```
- Group Monthly Ticket Counts
```
monthly = df.groupby(
    pd.Grouper(key='Submission_Date', freq='ME')
).size().reset_index(name='ticket_count')

print(monthly)

```

## Key Influencers — what drives high/low satisfaction
- Key Influencers works best on a binary outcome rather than a raw continuous score
```
Satisfaction_Flag = IF('public support_tickets'[satisfaction_score] <= 2, "Low Satisfaction", "High Satisfaction")

```


## Create Machine Learning Features
- Create Month Numbers
```
monthly['month_number'] = range(1, len(monthly)+1)

X = monthly[['month_number']]
y = monthly['ticket_count']

print(X.head())
print(y.head())

```
- Import ML Library
```
from sklearn.linear_model import LinearRegression

```
- Train Model
```
model = LinearRegression()

model.fit(X, y)

```
- Predict Future Months
- Create Future Months
```
future_months = pd.DataFrame({
    'month_number': range(len(monthly)+1, len(monthly)+7)
})

print(future_months)

```

- Predict Future Tickets
```
future_predictions = model.predict(future_months)

future_months['predicted_tickets'] = future_predictions

print(future_months)

```

```
future_dates = pd.date_range(
    start=monthly['Submission_Date'].max(),
    periods=7,
    freq='ME'
)[1:]

future_months = pd.DataFrame({
    'Submission_Date': future_dates,
    'month_number': range(len(monthly)+1, len(monthly)+7)
})

future_predictions = model.predict(
    future_months[['month_number']]
)

future_months['predicted_tickets'] = future_predictions

print(future_months)

```
- Instead of month numbers, create actual future dates.
```
future_months['Submission_Date'] = pd.date_range(
    start=monthly['Submission_Date'].max(),
    periods=6,
    freq='ME'
)

print(future_months)
----

- Save CSV
```
future_months.to_csv(
    'ticket_predictions.csv',
    index=False
)

print("CSV file saved successfully")

---

### Insights & Findings  
1. **Identified that High and Medium priority tickets formed the majority of customer requests.
2. **Discovered that longer resolution times reduced customer satisfaction.  
3. **Detected recurring issue categories causing high ticket volumes.
4. **Average resolution time remained around:39 hours
5. **Critical tickets were fewer but required immediate attention.


---

### Recommendations  

#### 1.Improve response efficiency by reducing average ticket resolution time.

#### 2.Implement automated ticket prioritization for Critical and High-priority issues.

#### 3. Introduce AI/chatbot support for handling repetitive customer queries.

#### 4.Create SLA alerts and escalation workflows for delayed tickets.

---

### Future Work
* Integrate real-time streaming data for live customer support monitoring.
* Implement advanced machine learning models for more accurate ticket forecasting.
* Add sentiment analysis on customer messages using NLP techniques.






