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
- Handle missing values.
- Remove duplicates.
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
Data visualization for the dataset was done in Microsoft Power BI Desktop, the dashboard includes four main dashboards:
- Overview
- New Hire
- Turnover
- Promotion

![image](https://github.com/user-attachments/assets/a770ca3c-e39e-4adb-8467-f6dd31569da7)
![image](https://github.com/user-attachments/assets/506a6867-ccf1-4321-b8f0-8c2abbafb4ca)
![image](https://github.com/user-attachments/assets/14be0e09-869d-4abb-a211-f36a9cb285a2)
![image](https://github.com/user-attachments/assets/b3d29453-d420-4bb0-82b4-e11cbfd32913)

## Key Performance Indicators and Metrics
Overview:
Number of employees, leavers
Number of promotions and hiring in FY20
Average Performance
Age distribution
Nationality distribution
Job-level distribution
Time type distribution
About Performance:

Average Performance of gender, leavers
Performance Rate distribution
Average Performance Rate by Positions, Age, Region
Comparison performance between leaver and non-leaver by department
About Hiring & Promotion:

Indicators of the gender balance of new hire FY20, Promotion FY21, Turnover rate
Executive gender balance
Average Time at the Job level
Hiring trend and hiring distribution by nationality
Promotion and Turnover rate FY 21

Here’s my full Power BI dashboard:
- 
