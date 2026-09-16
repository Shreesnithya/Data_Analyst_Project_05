# Employee Performance & Attrition Analytics

## Problem Statement
Analyzed HR data for 250 employees to understand attrition patterns, salary distribution, and workforce demographics across departments — helping identify which departments and employee groups are at higher risk of attrition.

## Tools Used
- Microsoft Excel (Pivot Tables, Pivot Charts, Formulas)

## Approach
1. Cleaned and structured raw HR records (250 employees) — EmployeeID, Age, Gender, Department, Salary, Attrition Status, and Age Group.
2. Built pivot tables to summarize attrition by department and gender, average salary by department, and headcount by age group.
3. Created a dashboard combining these pivot tables and charts into a single interactive summary view.

## Data Overview
- **250 employee records** across 6 departments: Finance, HR, IT, Marketing, Operations, Sales.
- Fields tracked: Employee ID, Age, Gender, Department, Salary, Attrition Status, Age Group.

## Key Insights

**1. Attrition by department and gender**
| Department | Female | Male | Total Attrition |
|------------|--------|------|------------------|
| Finance | 3 | 7 | 10 |
| IT | 6 | 1 | 7 |
| Marketing | 5 | 0 | 5 |
| Operations | 0 | 5 | 5 |
| Sales | 2 | 2 | 4 |
| HR | 1 | 2 | 3 |

📉 **Overall attrition rate: 34 out of 250 employees (13.6%)** — Finance has the highest attrition (10 employees), driven mostly by male employees (7 of 10).

**2. Average salary by department**
| Department | Avg. Salary |
|------------|------------|
| IT | ₹76,783 |
| Finance | ₹75,397 |
| Marketing | ₹61,054 |
| Sales | ₹58,360 |
| Operations | ₹56,285 |
| HR | ₹53,245 |

💰 **IT and Finance pay the highest average salaries**, while HR has the lowest — yet Finance still has the highest attrition despite strong pay, suggesting compensation alone isn't driving attrition there.

**3. Workforce age distribution**
| Age Group | Employee Count |
|-----------|-----------------|
| <25 | 11 |
| 25-34 | 62 |
| 35-44 | 55 |
| 45-54 | 68 |
| 55+ | 54 |

👥 The workforce skews toward the **45-54 age group** (68 employees, the largest single group), with the fewest employees under 25 — indicating relatively low entry-level hiring.

## Files
- `HR_Analytics_Dashboard_Nithyashree.xlsx` — Excel workbook (Dashboard, HR Records, Attrition Analysis, Salary & Age Analysis sheets)

## Key Takeaways
- Attrition isn't evenly spread — Finance and IT together account for half of all attrition, despite paying above-average salaries, suggesting non-pay factors (workload, management, growth opportunities) may be worth investigating further.
- Gender split in attrition varies sharply by department (e.g., IT attrition is mostly female, Operations attrition is entirely male) — worth a deeper look into department-specific retention issues.
- The workforce is fairly mature (over half are 45+), which may mean succession planning and knowledge transfer should be a growing priority.
