# job-seeker-insights-sql
An SQL-powered dashboard for analyzing job market trends, interview success rates, and salary insights.
# 🌟 Job Seeker Insights Dashboard - SQL Project

## 🔖 Overview
The **Job Seeker Insights Dashboard** is an SQL-based project designed to analyze job market trends, interview success rates, and salary patterns. This project enables recruiters, businesses, and job seekers to make **data-driven hiring decisions** by leveraging structured database queries.

## 💻 Tech Stack
- **Database:** PostgreSQL / MySQL / SQL Server
- **Data Visualization:** Tableau / Power BI (optional)
- **Scripting & Automation:** Python (Pandas, SQLAlchemy) (optional)

## 🔧 Database Schema
### 1. Job Seekers Table
```sql
CREATE TABLE job_seekers (
    seeker_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    phone_number VARCHAR(20),
    resume_link VARCHAR(255),
    experience_years INT,
    skills TEXT,
    registered_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
### 2. Companies Table
```sql
CREATE TABLE companies (
    company_id SERIAL PRIMARY KEY,
    company_name VARCHAR(100),
    industry VARCHAR(50),
    location VARCHAR(100),
    website VARCHAR(255)
);
```
### 3. Job Applications Table
```sql
CREATE TABLE job_applications (
    application_id SERIAL PRIMARY KEY,
    seeker_id INT REFERENCES job_seekers(seeker_id),
    company_id INT REFERENCES companies(company_id),
    job_title VARCHAR(100),
    application_date DATE,
    interview_date DATE,
    interview_status VARCHAR(50) CHECK (interview_status IN ('Pending', 'Interview Scheduled', 'Rejected', 'Offered')),
    salary_offer DECIMAL(10,2),
    FOREIGN KEY (seeker_id) REFERENCES job_seekers(seeker_id),
    FOREIGN KEY (company_id) REFERENCES companies(company_id)
);
```

## 🔄 Key SQL Queries

### 1. 🌐 Top Hiring Industries
```sql
SELECT c.industry, COUNT(ja.application_id) AS total_applications
FROM job_applications ja
JOIN companies c ON ja.company_id = c.company_id
GROUP BY c.industry
ORDER BY total_applications DESC
LIMIT 5;
```

### 2. 💰 Average Salary by Industry
```sql
SELECT c.industry, ROUND(AVG(ja.salary_offer), 2) AS avg_salary
FROM job_applications ja
JOIN companies c ON ja.company_id = c.company_id
WHERE ja.interview_status = 'Offered'
GROUP BY c.industry
ORDER BY avg_salary DESC;
```

### 3. 👨‍💼 Job Seekers with Most Interviews
```sql
SELECT js.full_name, COUNT(ja.application_id) AS total_interviews
FROM job_seekers js
JOIN job_applications ja ON js.seeker_id = ja.seeker_id
WHERE ja.interview_status = 'Interview Scheduled'
GROUP BY js.full_name
ORDER BY total_interviews DESC
LIMIT 5;
```

### 4. 🔄 Offer Conversion Rate
```sql
SELECT 
    COUNT(CASE WHEN interview_status = 'Offered' THEN 1 END) * 100.0 / COUNT(*) AS offer_conversion_rate
FROM job_applications;
```

## 🎨 Visualizing the Data (Optional)
- Connect your SQL database to **Tableau or Power BI** for creating interactive dashboards.
- Use Python's Pandas + Matplotlib for additional insights.

## 📚 Project Impact
✅ Helps businesses understand hiring trends in various industries.  
✅ Assists job seekers in identifying high-demand skills and companies offering competitive salaries.  
✅ Optimizes recruitment pipelines through **data-driven decision-making**.

## 💪 How to Use
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/job-seeker-insights-sql.git
   cd job-seeker-insights-sql
   ```
2. Import the SQL schema into your **PostgreSQL/MySQL** database.
3. Run queries and analyze hiring trends.
4. Optional: Connect to **Tableau / Power BI** for visual insights.

## 💡 Future Enhancements
- Implement **stored procedures** for automated reports.
- Integrate **Python scripting** for AI-driven hiring predictions.
- Expand the database with **real-world job market data**.

## 👤 Author
**Bibin Nakarmi** – Data Enthusiast | SQL Developer | AI & Analytics 🚀  

 
**LinkedIn:** https://www.linkedin.com/in/bibinnakarmi/ 

---
### ⭐ If you find this project useful, don’t forget to give it a **star** on GitHub! ⭐
