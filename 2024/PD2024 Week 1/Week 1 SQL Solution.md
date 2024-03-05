# Preppin’ Data - 2024: Week 1 - Prep Air's Flow Card

See challenge here: https://preppindata.blogspot.com/2024/01/2024-week-1-prep-airs-flow-card.html 

***
## 2024: Week 1 - Prep Air's Flow Card

January 03, 2024
Created by: Carl Allchin

At Preppin' Data we use a number of (mock) companies to look at the challenges they have with their data. For January, we're going to focus on our own airline, Prep Air. The airline has introduced a new loyalty card called the Flow Card. We need to clean up a number of data sets to determine how well the card is doing. 

The first task is setting some context for later weeks by understanding how popular the Flow Card is. Our stakeholder would like two data sets about our passengers. One data set for card users and one data set for those who don't use the card. 

**Requirements**
- Input the data
- Split the Flight Details field to form:
  - Date
  - Flight Number
  - From
  - To
  - Class
  - Price
- Convert the following data fields to the correct data types:
  - Date to a date format
  - Price to a decimal value
- Change the Flow Card field to Yes / No values instead of 1 / 0
- Create two tables, one for Flow Card holders and one for non-Flow Card holders
- Output the data sets
***
##  Data Set

Data fields:
- FLIGHT_DETAILS
- FLOW_CARD
- BAGS_CHECKED
- MEAL_TYPE
- 
Table:
<kbd>![1 Preppin Data 2024 Wk 1](https://github.com/mbellamybb/Preppin_Data_Challenges_SQL_Python/assets/95842597/196b81a1-bde2-4423-878f-8b5adc78a761)
  <kbd>


***
## Solution

### Explore data set
````sql
SELECT *
FROM PD2024_WK01;
````
Table:
<kbd>![1 Preppin Data 2024 Wk 1](https://github.com/mbellamybb/Preppin_Data_Challenges_SQL_Python/assets/95842597/196b81a1-bde2-4423-878f-8b5adc78a761)
  <kbd>


### Splitting Flight_details field
````sql
SELECT split_part(FLIGHT_DETAILS,'//',1) as Date,
       split_part(FLIGHT_DETAILS,'//',2) as Flight_Number,
       split_part(FLIGHT_DETAILS,'//',3) as From_To,
       split_part(FLIGHT_DETAILS,'//',4) as Class,
       split_part(FLIGHT_DETAILS,'//',5) as Price,
       FLOW_CARD,
       BAGS_CHECKED,
       MEAL_TYPE      
FROM PD2024_WK01;
````
<kbd>![2 Preppin Data 2024 Wk 1](https://github.com/mbellamybb/Preppin_Data_Challenges_SQL_Python/assets/95842597/d83adc3c-a979-45c9-acb6-898d7a93f1b6)
 <kbd>

### Return table with customers who are Flow Card users
````sql
WITH
Flow_Card_Yes AS
(SELECT split_part(FLIGHT_DETAILS,'//',1) as Date,
       split_part(FLIGHT_DETAILS,'//',2) as Flight_Number,
       split_part(FLIGHT_DETAILS,'//',3) as From_To,
       split_part(FLIGHT_DETAILS,'//',4) as Class,
       split_part(FLIGHT_DETAILS,'//',5) as Price,
       FLOW_CARD,
       BAGS_CHECKED,
       MEAL_TYPE      
FROM PD2024_WK01
)

SELECT Date,
        Flight_Number,
        Split_part(From_To,'-',1) as From_,
        Split_part(From_To,'-',2) as To_,
        Class,
        Round(Price,2) as Price,
        CASE FLOW_CARD
            WHEN 1 THEN 'Yes'
            WHEN 0 THEN 'No'
            END as Flow_Card_Status,
        BAGS_CHECKED,
       MEAL_TYPE      
FROM Flow_Card_Yes
WHERE Flow_Card_Status = 'Yes'
LIMIT 20;
````
<kbd>!![3 Preppin Data 2024 Wk 1](https://github.com/mbellamybb/Preppin_Data_Challenges_SQL_Python/assets/95842597/89f64296-dc34-4464-a794-0b7649541926)
<kdb>

### Return table with customers who are not Flow Card users
````sql
WITH
Flow_Card_No AS
(SELECT split_part(FLIGHT_DETAILS,'//',1) as Date,
       split_part(FLIGHT_DETAILS,'//',2) as Flight_Number,
       split_part(FLIGHT_DETAILS,'//',3) as From_To,
       split_part(FLIGHT_DETAILS,'//',4) as Class,
       split_part(FLIGHT_DETAILS,'//',5) as Price,
       FLOW_CARD,
       BAGS_CHECKED,
       MEAL_TYPE      
FROM PD2024_WK01
)

SELECT Date,
        Flight_Number,
        Split_part(From_To,'-',1) as From_,
        Split_part(From_To,'-',2) as To_,
        Class,
        Round(Price,2) as Price,
        CASE FLOW_CARD
            WHEN 1 THEN 'Yes'
            WHEN 0 THEN 'No'
            END as Flow_Card_Status,
        BAGS_CHECKED,
       MEAL_TYPE      
FROM Flow_Card_No
WHERE Flow_Card_Status = 'No'
LIMIT 20;
````
<kbd>!![4 Preppin Data 2024 Wk 1](https://github.com/mbellamybb/Preppin_Data_Challenges_SQL_Python/assets/95842597/ed7a8d65-bce2-4db2-a21a-05f174177d60)<kdb>



