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


<img width="944" height="582" alt="image" src="https://github.com/user-attachments/assets/9c04b8ba-ce0c-44df-abdc-65b6916a2f94" />

--------------

<img width="905" height="496" alt="image" src="https://github.com/user-attachments/assets/797f857a-289d-4480-ab0d-7e3323892b20" />


---



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
