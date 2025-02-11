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
  
![image](https://github.com/user-attachments/assets/02c9786e-76b2-4124-a572-9342c8f326b9)

### New Hire

![image](https://github.com/user-attachments/assets/8025285b-cac9-4a07-8ccc-0d9c1a2e0e9c)

### Turnover
  
![image](https://github.com/user-attachments/assets/1521553b-0f21-400c-b32f-dd47962bcd6e)

### Promotion
  
![image](https://github.com/user-attachments/assets/d7d72c01-b95d-44d6-bd05-5e8fa8feb8e4)

Full Dashboard: 
- 

## Diversity and Inclusion Insights 
### Gender Representation and Leadership Gap:
- While the overall workforce is relatively balanced (59% male, 41% female), there's a significant disparity in leadership positions.
- Only 20% of executives are female, and the percentage of women decreases as job level increases (from 51.81% at Junior Officer to 12.5% at Director level). \\\
→ This suggests a clear "glass ceiling" issue that needs to be addressed.
- Sales and Marketing Department might favor men, proving by higher number of new hired men employees and higher number of female leavers.
  
### Turnover Concerns:
- 10.06% turnover rate with 47 leavers.
- Female leavers have a lower average tenure (3 years) compared to male leavers (4 years).
- Female leavers are older on average (42 years) compared to male leavers (38 years)
- Higher turnover in Operations and Sales & Marketing departments. \\\
_→ This might indicate potential issues with retention of experienced female talent._

### Promotion Patterns:
- There is improvement in women's promotion rates (from 22.22% in FY20 to 35.29% in FY21).
- However, promotion rates still favor men (33 male vs 18 female promotions in 2021).
- Sales & Marketing shows the highest number of promotions in 2021 with 17 male employees and only 2 female employees.

### Geographical Distribution:
- Most employees are concentrated in Switzerland (169) and Europe (120).
- Very limited presence in Americas, Asia Pacific, and Middle East.\\\
_→ This could limit diversity of perspective and global talent acquisition._

### Age Distribution:
- Average age is 31.97 years
- Majority of workforce is between 20-39 years
- Very few employees in 50+ age groups

## Hiring Practices:
- Positive trend in gender-balanced recruitment compared to earlier years.
- In 2020, slightly more women (51.52%) than men (48.48%) were hired.\\\
_→ This shows a positive trend toward gender-balanced recruitment._

## Recommendations:
- Implement structured leadership development programs specifically targeting high-potential female employees.
- Review promotion criteria and processes to eliminate potential gender bias.
- Conduct stay interviews and exit interviews to understand why female employees have shorter tenure.
- Develop targeted retention strategies for experienced employees, especially women.
- Consider expanding geographical presence to increase diversity.
- Review recruitment practices to ensure age diversity.
- Regular pay equity analysis to ensure fair compensation across gender and age groups.
- Investigate high turnover in Operations and Sales/Marketing departments.
