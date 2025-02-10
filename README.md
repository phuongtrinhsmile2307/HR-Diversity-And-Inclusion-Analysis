# HR-Diversity-And-Inclusion-Analysis
## Project Overview
This is the 4th BI task from a virtual internship at PwC through Forage. This dashboard is prepared for HR manager to see the employees overview of hiring, leaving, performance, promotions and what's the trend of working. 
Here is my proccess of building the Power BI Dashboard and some insights from it. 
## Data Source
Diversity Inclusion Dataset: The primary dataset used for this analysis is the [Diversity-Inclusion-Dataset.xlsx](https://github.com/phuongtrinhsmile2307/HR-Diversity-And-Inclusion-Analysis/blob/main/03%20Diversity-Inclusion-Dataset.xlsx)file.

The metadata of Diversity & Inclusion dataset:
| Attribute   | Description |
|------------|-------------|
| **File name** | 03 Diversity-Inclusion-Dataset |
| **Format** | .xlsx |
| **Size** | 177 KB |
| **Fields** | 32 |
| **Entities** | 500 |

The Diversity & Inclusion table field's names and their description:

| Column Name                                | Description |
|--------------------------------------------|-------------|
| Employee ID                                | Represents the unique number of the employee in the dataset |
| Gender                                     | Describes the gender of the employee |
| Job Level after FY20 promotions           | Describes the job level of the employee after being promoted in FY20 |
| New hire FY20?                             | Describes if the employee is a new hire in FY20 |
| FY20 Performance Rating                    | Represents the performance rating of the employee in FY20 |
| Promotion in FY21?                         | Describes if the employee is being promoted in FY21 |
| In base group for Promotion FY21           | Describes if the employee is being selected for promotion in FY21 |
| Target hire balance                        | Describes the target hire balance of the employee |
| FY20 leaver?                               | Describes if the employee is a leaver in FY20 |
| In base group for turnover FY20            | Describes if the employee is in a group for turnover in FY20 |
| Department @01.07.2020                     | Describes the department each employee belongs to as of January 7, 2020 |
| Leaver FY                                  | Describes if the employee is a leaver in an FY |
| Job Level after FY21 promotions           | Describes the job level of the employee after being promoted in FY21 |
| Last Department in FY20                    | Describes the last department each employee belongs to in FY20 |
| FTE group                                  | Describes if the employee belongs to an FTE group |
| Time type                                  | Describes the contract type of the employee |
| Department & JL group PRA status           | Describes the department and JL group PRA status of the employee |
| Department & JL group for PRA              | Describes the department and JL group PRA of the employee |
| Job Level group PRA status                 | Describes the job level group PRA status of the employee |
| Job Level group for PRA                    | Describes the job level group PRA of the employee |
| Time in Job Level @01.07.2020              | Describes the time in job level of the employee |
| Job Level before FY20 promotions           | Describes the job level of the employee before being promoted in FY20 |
| Promotion in FY20?                         | Describes if the employee is being promoted in FY20 |
| FY19 Performance Rating                    | Describes the performance rating of the employee in FY19 |
| Age group                                  | Describes the age group of the employee |
| Age @01.07.2020                            | Represents the age of the employee as of January 7, 2020 |
| Nationality 1                              | Describes the nationality of the employee at the state level |
| Region group: nationality 1                | Describes the nationality of the employee at the country level |
| Broad region group: nationality 1          | Describes the nationality of the employee at the regional level |
| Last hire date                             | Describes the last hire date of the employee |
| Years since the last hire                  | Represents the number of years since the last hire of the employee |
| Rand                                       | Generates a random number for each entry in the dataset |


## Data Cleaning
Data Cleaning for the dataset was done in Power Query as follows:
- Removal of unnecessary columns:
  
```
= Table.RemoveColumns(#"Changed Type1",{"Rand", "Broad region group: nationality 1", "Time in Job Level @01.07.2020", "Job Level group PRA status", "Department & JL group PRA status", "FTE group"})
```

- Handle null values: The null values that were relevant to the analysis, especially in the Performance Rating columns were replaced with 0’s because they are numeric columns.
- Standardize data formats.
  
## Measures
### Leavers
- **Percentage of female employees leaved in 2020:**
```
%Women Leavers = DIVIDE(CALCULATE(COUNT('Pharma Group AG'[FY20 leaver?]),'Pharma Group AG'[FY20 leaver?]="Yes",'Pharma Group AG'[Gender]="female"),[Number of Leavers])
```

- **Number of employees leaved in 2020:**
```
Number of Leavers = CALCULATE(COUNT('Pharma Group AG'[FY20 leaver?]),'Pharma Group AG'[FY20 leaver?]="Yes")
```

- **The percentage of employees leaved during 2020:**
```
Turnover Rate = DIVIDE(CALCULATE(COUNT('Pharma Group AG'[FY20 leaver?]),'Pharma Group AG'[FY20 leaver?]="Yes"),DIVIDE(CALCULATE(COUNT('Pharma Group AG'[Employee ID]),'Pharma Group AG'[In base group for turnover FY20]="Y")+CALCULATE(COUNT('Pharma Group AG'[Employee ID]),NOT('Pharma Group AG'[Department @01.07.2020]=BLANK())),2))
```

### New Hire
- **Percentage of employees were hired in 2020:**
```
% New Hire = DIVIDE(CALCULATE(COUNT('Pharma Group AG'[New hire FY20?]),'Pharma Group AG'[New hire FY20?]="Y"),CALCULATE(COUNT('Pharma Group AG'[Employee ID]),ALLSELECTED('Pharma Group AG'[New hire FY20?])))
```

- **Percentage of male employees were hired in 2020:**
```
%Men Hired = DIVIDE(CALCULATE(COUNT('Pharma Group AG'[New hire FY20?]),'Pharma Group AG'[New hire FY20?]="Y",'Pharma Group AG'[Gender]="male"),CALCULATE(COUNT('Pharma Group AG'[New hire FY20?]),'Pharma Group AG'[New hire FY20?]="Y"))
```

- **Percentage of female employees were hired in 2020:**
```
%Women Hired = DIVIDE(CALCULATE(COUNT('Pharma Group AG'[New hire FY20?]),'Pharma Group AG'[New hire FY20?]="Y",'Pharma Group AG'[Gender]="female"),CALCULATE(COUNT('Pharma Group AG'[New hire FY20?]),'Pharma Group AG'[New hire FY20?]="Y"))
```

### Promotion
- **Percentage of employees were promoted in 2020:**
```
%Employee Promoted FY20 = DIVIDE(CALCULATE(COUNT('Pharma Group AG'[Promotion in FY20?]),'Pharma Group AG'[Promotion in FY20?]="Y"),CALCULATE(COUNT('Pharma Group AG'[Promotion in FY20?]),ALLSELECTED('Pharma Group AG'[Promotion in FY20?])))
```

- **Percentage of employees were promoted in 2021:**
```
%Employee Promoted FY21 = DIVIDE(CALCULATE(COUNT('Pharma Group AG'[Promotion in FY21?]),'Pharma Group AG'[Promotion in FY21?]="Yes"),CALCULATE(COUNT('Pharma Group AG'[In base group for Promotion FY21]),'Pharma Group AG'[In base group for Promotion FY21]="Yes"))
```

### Performance Rating
- **Average women' performance rating in 2020:**
```
Women Performance = CALCULATE(AVERAGE('Pharma Group AG'[FY20 Performance Rating]),'Pharma Group AG'[Gender]="female")
```

- **Average men' performance rating in 2020:**
```
Men Performance = CALCULATE(AVERAGE('Pharma Group AG'[FY20 Performance Rating]),'Pharma Group AG'[Gender]="male")
```
## Analysis and Visualization 
Data visualization for the dataset was done in Microsoft Power BI Desktop, the dashboard includes four main pages:
### Overview
  
![image](https://github.com/user-attachments/assets/6d1febb7-a4ae-4128-9bb4-12825a75af6f)

### New Hire

![image](https://github.com/user-attachments/assets/8025285b-cac9-4a07-8ccc-0d9c1a2e0e9c)

### Turnover
  
![image](https://github.com/user-attachments/assets/1521553b-0f21-400c-b32f-dd47962bcd6e)

### Promotion
  
![image](https://github.com/user-attachments/assets/d7d72c01-b95d-44d6-bd05-5e8fa8feb8e4)

Full Dashboard: 
- 

## Diversity and Inclusion Insights 

###
- Sales and Marketing Department favor men, proving by higher number of new hired men employees and higher number of female leavers.
- Leadership roles show lower figure of female representation
- 
### Gender Distribution
-  Gender imbalance, with 8% higher percentage of male employees than female employees.
-  Clear gender imbalance at senior levels (Director and Executive)
-  Except the HR department, other departments have larger number of men than women. 
-  Except the Junior lever, other positions have larger number of men than women.
-  Employee performance scores are almost equal for men (2.41) and women (2.42), indicating no significant gender-based performance gaps.
### Age distribution
- Younger employees (20-29 years old) are mostly in junior roles (74.42%), while leadership roles (Executives, Directors, and Senior Managers) are predominantly held by older employees (40+ years).
### Hiring Practices
- The gap between genders is gradually smaller from 2011 to 2020 
- The hiring practices demonstrate a balanced approach with 51.52% women hires, showing a deliberate effort to promote diversity within the workforce.
- New hired women have slightly lower average age (30.38) compared to men (33.22)
### Turnover Rate
- 47 employees have left the organization.
- Higher turnover among male employees (55.32% of leavers).
- Female leavers have higher average age (42) and shorter tenure (3 years) than male leavers.
- Operations and Sales/Marketing departments show highest turnover.
### Promotion
- Positive trend in promotions for women, increasing from 22.22% in FY20 to 35.29% in FY21.
- Higher promotion rates for women in Senior Officer positions (61% vs 39% for men in 2021).

## Recommendation:
- Address female retention issues, particularly the shorter tenure
- Continue supporting women's advancement into executive roles
- Expand geographical diversity, especially in underrepresented regions
- Investigate high turnover in Operations and Sales/Marketing departments
