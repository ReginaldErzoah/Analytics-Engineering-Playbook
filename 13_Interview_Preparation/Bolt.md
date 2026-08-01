# Bolt Customer Support Data Analyst Interview Preparation Notes

## Candidate

Reginald Erzoah

## Role

Customer Support Data Analyst (CS Data Analyst)

## Company

Bolt

---

# 1. Understanding The Role

## What Bolt Is Looking For

Bolt wants a data analyst who can transform Customer Support data into insights that improve:

- Customer experience
- Support operations
- Agent performance
- Reporting processes
- Decision-making

The role is not only about creating dashboards.

It requires someone who can:

```

Raw Customer Support Data

↓

Clean And Reliable Data

↓

Analytics Models

↓

Dashboards

↓

Business Insights

↓

Operational Improvements

```

---

# 2. My Positioning For This Role

## My Introduction

"I am a Data Analyst specializing in customer analytics and business intelligence, with experience building dashboards, KPI frameworks, and reporting workflows that help organizations make better decisions.

I have hands-on experience using SQL, Python, Power BI, dbt, and data modeling to transform raw operational data into reliable analytical solutions.

In my current role at Koa, I build dashboards, monitor operational KPIs, perform data validation, and work with stakeholders to improve reporting processes.

I also built SupportOps Intelligence, an end-to-end customer support analytics platform that analyzes support performance metrics such as ticket volume, response time, resolution time, SLA compliance, and customer satisfaction.

I am excited about this opportunity because it combines my technical skills with my interest in using data to improve customer experiences."

---

# 3. Why Bolt?

## Answer

"I am interested in Bolt because it operates at a scale where analytics can create significant operational impact.

Customer Support is an area where data can directly improve customer satisfaction, response times, and operational efficiency.

I like that this role combines analytics, automation, dashboards, and collaboration with Operations and Engineering teams.

My background in building BI solutions and my SupportOps Intelligence project align strongly with the challenges this role addresses."

---

# 4. Why This Role?

## Answer

"This role matches the direction I have been developing professionally.

I enjoy working at the intersection of data and business operations.

I like taking complex operational data, creating reliable analytical models, and turning them into insights that help teams make better decisions.

The Customer Support domain is especially interesting because improvements in analytics can directly affect customer satisfaction."

---

# 5. Tell Me About Yourself

## Structure

Use:

```

Current Role

↓

Technical Skills

↓

Projects

↓

Why This Role

```

## Answer

"I am a Data Analyst with experience building business intelligence solutions, KPI dashboards, and reporting workflows.

Currently, I work as a Data Analytics Officer where I develop Power BI dashboards, analyze operational performance, validate data quality, and collaborate with teams to improve reporting.

My technical toolkit includes SQL, Python, Power BI, dbt, DuckDB, and data modeling.

Beyond my professional experience, I have built analytics projects including SupportOps Intelligence, a customer support analytics platform designed to transform support data into operational insights.

I enjoy solving business problems through data and building analytics solutions that are reliable, scalable, and actionable."

---

# 6. SupportOps Intelligence Project Explanation

## Project Overview

"I built SupportOps Intelligence as an end-to-end customer support analytics platform.

The goal was to transform raw support data into trusted analytical models and dashboards that help teams understand support performance."

---

## Problem

"Customer support teams generate large amounts of data, but without proper analysis it can be difficult to identify operational bottlenecks, customer issues, and performance trends."

---

## Solution

"I designed a workflow that cleans, transforms, models, and visualizes support data."

Architecture:

```

Raw Support Data

↓

Python Processing

↓

DuckDB Analytical Layer

↓

dbt Transformation Models

↓

Power BI Dashboard

```

---

## Metrics Analyzed

Mention:

- Ticket volume
- First response time
- Resolution time
- SLA compliance
- Customer satisfaction
- Backlog trends
- Agent performance

---

## Data Modeling

Explain:

"I used dimensional modeling principles by separating analytical entities into fact and dimension structures to make reporting easier and more maintainable."

Example:

Fact table:

```

fact_tickets

ticket_id
customer_id
agent_id
created_date
resolved_date
resolution_time

```

Dimensions:

```

dim_customer

dim_agent

dim_date

dim_issue_category

````

---

## Data Quality

"I implemented validation checks to ensure analytical outputs were reliable.

Examples included:

- Missing values
- Duplicate records
- Invalid timestamps
- Schema consistency checks"

---

# 7. SQL Interview Preparation

## SQL Concepts To Demonstrate

Know:

- SELECT
- WHERE
- GROUP BY
- HAVING
- JOIN
- CASE WHEN
- CTEs
- Window functions

---

# SQL Scenario 1

## Find Total Tickets Per Customer

Question:

"Which customers contacted support the most?"

Answer:

```sql
SELECT
    customer_id,
    COUNT(ticket_id) AS total_tickets
FROM tickets
GROUP BY customer_id
ORDER BY total_tickets DESC;
````

---

# SQL Scenario 2

## Average Resolution Time

```sql
SELECT
    AVG(
        resolved_at - created_at
    ) AS avg_resolution_time
FROM tickets
WHERE resolved_at IS NOT NULL;
```

---

# SQL Scenario 3

## Monthly Ticket Trend

```sql
SELECT
    DATE_TRUNC('month', created_at) AS month,
    COUNT(*) AS tickets
FROM tickets
GROUP BY month
ORDER BY month;
```

---

# SQL Scenario 4

## SLA Compliance

Logic:

```
Tickets solved within SLA

/

Total tickets
```

Example:

```sql
SELECT
    AVG(
        CASE
            WHEN resolution_time <= sla_time
            THEN 1
            ELSE 0
        END
    ) AS sla_rate
FROM tickets;
```

---

# 8. Customer Support Metrics

## Ticket Volume

Definition:

Number of customer issues created.

Why important:

Shows support demand.

---

## First Response Time

Definition:

Time between ticket creation and first agent response.

Why important:

Measures responsiveness.

---

## Resolution Time

Definition:

Time required to solve customer issues.

Why important:

Measures efficiency.

---

## SLA Compliance

Definition:

Percentage of tickets resolved within agreed service targets.

Formula:

```
Resolved Within SLA

/

Total Tickets
```

---

## CSAT

Customer Satisfaction Score.

Measures:

Customer happiness after support interaction.

---

## Backlog

Number of unresolved tickets.

Important because:

Growing backlog may indicate:

* Capacity problems
* Process issues
* Product issues

---

# 9. Business Scenario Questions

---

# Scenario 1

## Ticket Volume Increased By 40%

Question:

"What would you investigate?"

Answer:

"First, I would validate whether the increase represents customer growth or an operational issue.

I would analyze the increase by:

* Date
* Issue category
* Customer segment
* Region
* Priority level

Then I would compare:

* Response time
* Resolution time
* SLA compliance

to understand whether support capacity is being affected."

---

# Scenario 2

## CSAT Dropped

Answer:

"I would investigate where the decline is occurring.

I would analyze:

* Issue categories
* Agent performance
* Resolution times
* Customer segments
* Recent product changes

The goal would be identifying the root cause before recommending actions."

---

# Scenario 3

## Leadership Wants A Dashboard

Answer:

"I would first understand the decisions they need to make.

For Customer Support leadership, I would include:

Executive KPIs:

* Total tickets
* SLA compliance
* CSAT
* Average resolution time

Operational views:

* Ticket trends
* Agent performance
* Issue categories
* Backlog analysis

The dashboard should answer business questions, not only display numbers."

---

# 10. Dashboard Design Answer

## Customer Support Dashboard

## Page 1: Executive Overview

KPIs:

* Total Tickets
* Open Tickets
* Resolution Rate
* CSAT
* SLA Compliance

---

## Page 2: Support Performance

Charts:

* Tickets by agent
* Resolution time by agent
* CSAT by agent

---

## Page 3: Customer Issues

Charts:

* Top issue categories
* Trends over time
* Customer segments

---

## Page 4: SLA Monitoring

Charts:

* SLA breaches
* Breaches by category
* Breaches by team

---

# 11. Stakeholder Communication

## STAR Framework

Use:

```
Situation

Task

Action

Result
```

---

# Example

## Question:

"Tell me about working with stakeholders."

Answer:

"At Koa, leadership needed better visibility into operational performance.

My responsibility was to understand reporting needs and build dashboards that provided actionable insights.

I worked with stakeholders to define KPIs, cleaned and validated data sources, developed Power BI dashboards, and communicated findings.

The result was improved visibility into operations and reduced manual reporting effort."

---

# 12. Data Quality Questions

## Question:

"How do you ensure dashboard accuracy?"

Answer:

"I approach data quality at multiple levels:

First, I validate source data completeness and consistency.

Second, I apply transformation checks during data processing.

Third, I validate final dashboard metrics against expected business definitions.

I also document KPI definitions so stakeholders understand exactly how metrics are calculated."

---

# 13. dbt Questions

## Question:

"What experience do you have with dbt?"

Answer:

"I use dbt concepts for analytics engineering workflows, including modular transformations, testing, documentation, and creating reliable analytical models.

I like dbt because it brings software engineering practices such as version control, testing, and documentation into analytics workflows."

---

# 14. Python Questions

## Question:

"How do you use Python?"

Answer:

"I use Python mainly for data preparation, automation, profiling, and analytics workflows.

Examples include:

* Cleaning datasets
* Automating repetitive reporting tasks
* Building data quality checks
* Creating analytical applications using Streamlit"

---

# 15. Questions To Ask Interviewers

## Question 1

"What does the current Customer Support analytics stack look like? Are you currently using Looker, dbt, or other analytics tools?"

---

## Question 2

"What are the biggest data challenges the Customer Support team currently faces?"

---

## Question 3

"What metrics are most important for measuring success in this role?"

---

## Question 4

"What would success look like for this position after six months?"

---

# 16. Final Interview Reminders

## Always Connect Data To Business

Do not say:

"I created a dashboard."

Say:

"I created a dashboard that helped stakeholders monitor performance, identify bottlenecks, and make operational decisions."

---

## Always Explain The Why

Example:

Not:

"I calculated resolution time."

Better:

"I analyzed resolution time to identify efficiency issues and understand where support processes could improve."

---

## Remember Your Advantage

Your strongest selling points:

1. SupportOps Intelligence project
2. SQL + Power BI skills
3. Data quality experience
4. dbt/data modeling knowledge
5. Stakeholder reporting experience


## Resources To Revisit From Our Earlier Work

SQL mastery
dbt
data modeling
data quality
observability

Very relevant because Bolt mentions:

pipelines
governance
data quality
dbt
GitHub Portfolio

Make sure your repositories demonstrate:

Clean README files
Architecture diagrams
Setup instructions
Screenshots
Business context

---

# Final Message To Remember

"I am not only someone who analyzes data. I build reliable analytics solutions that help teams understand problems, improve operations, and make better decisions."
