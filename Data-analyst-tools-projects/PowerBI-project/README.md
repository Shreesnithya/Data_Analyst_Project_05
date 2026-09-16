# Hospital Patient Analytics Dashboard — Power BI Dashboard

## Problem Statement
Built an interactive dashboard analyzing hospital patient data — admissions, treatment costs, length of stay, and readmission rates — to help hospital administrators identify cost drivers and readmission risk across departments.

## Tools Used
- Power BI Desktop
- DAX, Power Query

## Data Overview
- **500 patient records** across 6 departments: Orthopedics, Oncology, Cardiology, General Medicine, Pediatrics, Neurology.
- Fields tracked: Patient ID, Age, Gender, Department, Admission Type, Admission/Discharge Date, Length of Stay, Treatment Cost, Readmission Status, Bed Occupancy.

## Approach
1. Cleaned and shaped patient data using Power Query (validated data types, checked for missing values, formatted dates).
2. Built DAX measures for key metrics — average treatment cost, average length of stay, readmission rate, and bed occupancy rate.
3. Designed an interactive dashboard with slicers (Department, Admission Type, Gender) enabling cross-filtering across all visuals.

## Key Insights

**1. Cost varies significantly by department**
| Department | Avg. Treatment Cost |
|------------|---------------------|
| Oncology | ₹15,586 |
| Neurology | ₹12,056 |
| Cardiology | ₹11,059 |
| Orthopedics | ₹9,290 |
| General Medicine | ₹5,956 |
| Pediatrics | ₹5,673 |

💰 **Oncology costs nearly 3x more than Pediatrics on average** — the biggest cost driver in the hospital, and a key area for cost-management focus.

**2. Readmissions**
- Overall readmission rate: **9.8%** (49 out of 500 patients).
- **Orthopedics has the highest readmission rate (12.4%)**, despite not having the highest treatment cost — suggesting a possible gap in post-discharge care or follow-up protocols specifically in that department.
- Oncology, despite being the most expensive department, has the **lowest readmission rate (7.0%)** — indicating thorough treatment even at higher cost.

**3. Admission patterns**
- Admission type split: Urgent (184), Elective (160), Emergency (156) — a fairly even three-way split, with Urgent slightly leading.
- Average length of stay across the hospital: **7.8 days**, with Oncology (8.3 days) and Orthopedics (8.1 days) staying longest.

**4. Capacity**
- **Bed occupancy rate: 77.4%** — a healthy but not overcrowded utilization level, leaving some buffer capacity for surges.

## Files
- `Hospital_patient_analysis_data.pbix` — Power BI dashboard file
- `Hospital_Patient_Dataset.csv` — Source dataset (500 patient records)

## Key Takeaways
- Oncology drives the highest costs but has the best readmission outcomes — cost isn't necessarily a red flag here, it may reflect thorough care.
- Orthopedics stands out as a priority area: high volume, high readmission rate, and long stays — worth investigating discharge planning and follow-up care in that department specifically.
- With bed occupancy at ~77%, there's some room to absorb demand spikes, but it's worth monitoring department-level occupancy separately since overall figures can mask localized strain.
