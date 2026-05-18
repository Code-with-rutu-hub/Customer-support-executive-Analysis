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

<img width="951" height="589" alt="image" src="https://github.com/user-attachments/assets/182eba9d-ba52-44e7-ab80-639bc90387e4" />

------

<img width="905" height="496" alt="image" src="https://github.com/user-attachments/assets/797f857a-289d-4480-ab0d-7e3323892b20" />

------

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


- Save Forecast to csv
```
future_months.to_csv(
    'ticket_predictions.csv',
    index=False
)

print("CSV file saved successfully")

```

- Check File Exists
```
import os

print(os.getcwd())

```



<img width="752" height="349" alt="image" src="https://github.com/user-attachments/assets/86180e2d-48f9-4acc-916b-56fc8465cc97" />









## Python Forecasting



### Insights & Findings  
1. **Mumbai Indians and Chennai Super Kings are among the most successful IPL teams.
2. **Winning the toss and choosing to field gives a higher success rate in some seasons.  
3. **Virat Kohli, David Warner, and Suresh Raina are top consistent scorers.
4. **Some venues heavily favor chasing teams. 


---

### Recommendations  

#### 1. Teams should prioritize players with consistently high strike rates and match-winning contributions across seasons.

#### 2. Toss decisions should be optimized based on venue trends and historical chase/defend success rates.

#### 3. Bowlers with low economy rates in death overs should be utilized more effectively in high-pressure situations.

#### 4. Teams should focus on boundary efficiency (4s and 6s) as it strongly impacts total score and win probability.

---

### Future Work
* Predict Match Winner : Use Python + Machine Learning Algorithms.
* Player Recommendation System : Recommend best batsman/bowler based on conditions.
* Live IPL Dashboard : Connect Power BI with APIs.
