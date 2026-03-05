Healthcare Data Analysis & Power BI Dashboard
Project Overview
This project analyses operational and financial performance for a multi-hospital healthcare network using MySQL and Power BI.
The goal of the project is to transform raw healthcare data into actionable insights that help hospital leadership improve efficiency, patient care, and revenue performance.
The dataset represents hospitals, doctors, patients, and visit records. It includes realistic challenges such as:
* Missing values
* Cancelled and no-show appointments
* Inconsistent revenue data
These issues make the dataset suitable for demonstrating real-world data cleaning and analytics skills.
The project combines:
* SQL for data cleaning and analysis
* Power BI for interactive dashboard visualization

 Tools Used
* MySQL Workbench – Data cleaning and analysis
* Power BI Desktop – Dashboard and data visualization

 Dataset Description
The database contains four relational tables.
Hospitals
Contains hospital information including:
* hospital name
* location
* opening year

Doctors
Includes doctor profiles:
* doctor name
* department
* salary
* hospital affiliation

Patients
Contains patient demographic information:
* gender
* age
* insurance type
* city
Some age values were intentionally missing to simulate real-world data quality issues.

Visits
Tracks patient appointments including:
* visit date
* visit status (Completed, Cancelled, No-Show)
* treatment cost
* doctor ID
* patient ID
Together these tables form a relational healthcare analytics dataset.

 Business Objectives
Hospital leadership requested insights to answer the following questions:
1. Which hospitals generate the most revenue?
2. Which departments are the most profitable?
3. Which doctors contribute the most revenue?
4. What percentage of visits are cancelled or no-shows?
5. How does revenue change over time?
6. Are certain hospitals or doctors disproportionately responsible for performance?
7. What trends emerge when revenue is smoothed using moving averages?

 Data Cleaning
Several data quality issues were addressed using SQL.
Missing Patient Ages
Missing patient ages were replaced with the average age of available records.
UPDATE patients
SET age =
(
SELECT ROUND(AVG(age))
FROM patients
WHERE age IS NOT NULL
)
WHERE age IS NULL;

Standardizing Visit Status
UPDATE visits
SET visit_status = TRIM(visit_status);

Handling Missing Treatment Costs
UPDATE visits
SET treatment_cost = 0
WHERE treatment_cost IS NULL;

?? SQL Analysis
Hospital Revenue
SELECT
h.hospital_name,
COUNT(v.visit_id) AS total_visits,
ROUND(SUM(v.treatment_cost),2) AS total_revenue
FROM visits v
JOIN doctors d ON v.doctor_id = d.doctor_id
JOIN hospitals h ON d.hospital_id = h.hospital_id
GROUP BY h.hospital_name
ORDER BY total_revenue DESC;


Doctor Productivity
SELECT
d.doctor_name,
COUNT(v.visit_id) AS completed_visits,
ROUND(SUM(v.treatment_cost),2) AS revenue_generated
FROM visits v
JOIN doctors d ON v.doctor_id=d.doctor_id
WHERE v.visit_status='Completed'
GROUP BY d.doctor_name
ORDER BY revenue_generated DESC;

Monthly Revenue Trend
SELECT
DATE_FORMAT(visit_date,'%Y-%m') AS month,
ROUND(SUM(treatment_cost),2) AS revenue
FROM visits
WHERE visit_status='Completed'
GROUP BY month
ORDER BY month;

 Power BI Dashboard
An interactive Healthcare Analytics Dashboard was created in Power BI to visualize operational and financial insights.
Dashboard Features
The dashboard includes:
KPI Metrics
* Total Revenue
* Total Visits
* Completed Visits
* Total Patients
* Average Treatment Cost

Key Visualizations
1. Monthly Revenue Tre
2. Shows how healthcare revenue changes over time.
3. Revenue by Hospital
o Compares performance across hospitals.
4. Top Doctors by Revenue
o Identifies high-performing doctors.
5. Visits by Department
o Shows which departments handle the most patient volume.
6. Visit Status Distribution
o Highlights cancellations and no-show rates.


 Key Findings
Operational Insights
* Completed visits represent the majority of appointments.
* Cancelled and no-show visits still represent lost revenue opportunities.

Department Performance
* Cardiology and Oncology generate the highest average treatment costs.
* Pediatrics has high patient volume but lower revenue per visit.

Doctor Productivity
A small group of doctors generates a disproportionately large share of revenue.
In some hospitals, a single doctor contributes more than 25% of total revenue.

Revenue Trends
Revenue fluctuates throughout the year.
Using a 3-month moving average helps reveal the underlying performance trend.

 Business Impact
This analysis provides hospital leadership with:
* Visibility into top-performing hospitals
* Identification of high-impact doctors
* Insight into lost revenue from cancelled appointments
* Trend awareness for financial planning
These insights can support:
* staffing decisions
* scheduling optimization
* resource allocation

 Future Improvements
This project could be expanded by adding:
* Diagnosis and procedure codes
* Patient readmission tracking
* Predictive modeling for no-show risk
* Length-of-stay metrics
* Patient satisfaction scores

 Skills Demonstrated
* SQL data cleaning
* Relational database design
* Multi-table joins
* Aggregations and KPI creation
* Window functions
* Power BI dashboard design
* Business analytics storytelling
* Healthcare domain analysis

Conclusion
This project demonstrates how SQL and Power BI can transform raw healthcare data into meaningful operational and financial insights.

