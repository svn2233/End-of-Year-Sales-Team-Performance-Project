# Team Beverage End of Year Perfromance Project SQL --> Tableau

###### By: Steven Nguyen

## Introduction

#### In this project I will use MySQL to explore and analyze data and then visualize the data in Tableau to provide insights to the stakeholders.

## Objective

##### I am a manager in sales covering NYC for a beverage company. Since it is the end of the year I am tasked with evaluating my teams performance this year and presenting it in a final Tableau story. The stakeholders need to know which employees hitting their metrics or not achieving their metrics. If not they need to know how will I improve their performance for the coming year. They need to also know which employees deserves a full merit increase and which employee is ready for a promotion. They also what KPI directly affect cases sold as it drives profit. They will need data driven insights from me. To deliver data driven insights to my stakeholder I will answer questions like:
	1. Which rep is performing the best or worst?
	2. Is there a correlation between # of account visits and cases sold?
	3. How do we improve the performance of the worst performing rep
	4. Is there any trends that is seen in the database?
	5. How do we makes sure we sell more cases for next year?

## Stakeholders

    [] John David-Regional Manager
    [] Kim Tran- Director of Sales


#### Before analyzing data

#### I will examine the table to look for anything to fix/clean before I do my analysis
```
SELECT * FROM reps;
```

####  Renaming columns total_cases_sold & total cases to avoid confusion between two columns 
```
ALTER TABLE reps
RENAME COLUMN total_cases_sold TO cases_sold_$;

ALTER TABLE reps
RENAME COLUMN `total cases` TO cases_sold;
```

#### Check if columns have been changed 
```
SELECT * FROM reps;
```
![](/1.png)<!-- -->

####  Finding the name of the sales rep, region covered and manager
```
SELECT distinct CONCAT(city,', ',state) as team_region,
      manager,employee_id,
      CONCAT(first_name,' ',last_name) as full_name,
      borough as region_covered
FROM reps;
```
![](/2.png)<!-- -->

### STEPS

#### 1. Finding total amount of accounts visited for all reps ordered by the rep with the most visits.
```
SELECT first_name,last_name,year,borough as region_covered,
       SUM(account_visits) as total_visits
FROM reps
GROUP BY 1,2,3,4
ORDER BY 5 DESC;
```
![](/3.png)<!-- -->
#### 2. Finding the total of cases sold and # of cases sold
```
SELECT first_name,last_name,year,borough as region_covered,
       SUM(cases_sold_$) as total_cases_sold_$, SUM(cases_sold) as num_of_cases
FROM reps
GROUP BY 1,2,3,4
ORDER BY 5 DESC;
```
![](/4.png)<!-- -->
#### 3. Finding the average of cases sold and # of cases sold
```
SELECT first_name,last_name,year,borough as region_covered,
       ROUND(AVG(cases_sold_$),0) as total_cases_sold_$,
        ROUND(AVG(cases_sold),2) as num_of_cases
FROM reps
GROUP BY 1,2,3,4
ORDER BY 5 DESC;
```
![](/5.png)<!-- -->
#### 4. Finding the average daily visits 
```
SELECT first_name,last_name,borough as region_covered,
       ROUND(AVG(avg_daily_visits),2) as average_daily_visits
FROM reps
GROUP BY 1,2,3
ORDER BY 4 DESC;
```
![](/6.png)<!-- -->
#### 5. Seeing if there is a correlation between account visits and cases sold
```
SELECT first_name,last_name,borough as region_covered,month,
       account_visits,avg_daily_visits,cases_sold_$,cases_sold
FROM reps
WHERE month IN('June','July','August')
ORDER BY 7 DESC;
```
![](/7.png)<!-- -->
#### 6. Examining performance in the 'Summer Heat' contest which is from June-July
```
SELECT first_name,last_name,borough as region_covered,
      SUM(account_visits) as total_visits,
      ROUND(AVG(avg_daily_visits),2) as average_daily_visits,
      SUM(cases_sold_$) as total_cases_$,
      SUM(cases_sold) as total_cases_sold
FROM reps
WHERE month IN('June','July','August')
GROUP BY 1,2,3
ORDER BY 7 DESC;
```
![](/8.png)<!-- -->
#### 7. Examining the best rep compared to the worst performing rep month to month
```
SELECT first_name,last_name,account_visits, avg_daily_visits,
       month,cases_sold_$, cases_sold
FROM reps
WHERE first_name IN('Luca','Brad')
ORDER BY 7 DESC
```
![](/9.png)<!-- -->
#### 8. Finding the top 5 worst selling months
```
SELECT month,SUM(account_visits) as total_account_visits,
       SUM(cases_sold_$) as total_cases_$,SUM(cases_sold) as total_cases_sold
FROM reps
GROUP BY 1
ORDER BY 3 ASC
LIMIT 5;
```
![](/10.png)<!-- -->
#### 9. Finding the top 5 selling months
```
SELECT month,SUM(account_visits) as total_account_visits,
       SUM(cases_sold_$) as total_cases_$,SUM(cases_sold) as total_cases_sold
FROM reps
GROUP BY 1
ORDER BY 3 DESC
LIMIT 5;
```
![](/11.png)<!-- -->
#### 10. Comparing core skus and innovations 
```
SELECT first_name,last_name,borough as region_covered,
       SUM(core_skus) as total_skus,SUM(innovations) as total_innovations
FROM reps
GROUP BY 1,2,3
ORDER BY 4 DESC;
```
![](/12.png)<!-- -->
#### 11. Seeing if placing POS and Coolers has a correlation in the # of cases sold for the year 2022
```
SELECT first_name,last_name,borough as region_covered, SUM(POS) as total_POS,
       SUM(coolers) as total_coolers,SUM(cases_sold_$) as total_cases_$,SUM(cases_sold) as total_cases_sold
FROM reps
GROUP BY 1,2,3
ORDER BY 7 DESC;
```
![](/13.png)<!-- -->
#### 12. Finding the average weekly visits
```
SELECT first_name,last_name,borough as region_covered, 
       ROUND(SUM(Account_Visits)/52,2) as average_weekly
FROM reps
GROUP BY 1,2,3
ORDER BY 4 DESC;
```
![](/14.png)<!-- -->
#### 13. Finding the teams monthly visits by month and correlation if any with cases sold.
```
SELECT  month,SUM(cases_sold) as total_cases_sold,
       ROUND(SUM(Account_Visits),2) as team_month_visits
FROM reps
GROUP BY 1
ORDER BY 2 DESC;
```
![](/15.png)<!-- -->
#### 14. Seeing if there is a correlation between facings gained in the cold vault and # of cases sold.
```
SELECT first_name,last_name,borough as region_covered, SUM(facing_gained) as total_facings,
       SUM(cases_sold_$) as total_cases_$,SUM(cases_sold) as total_cases_sold
FROM reps
GROUP BY 1,2,3
ORDER BY 6 DESC;
```
![](/16.png)<!-- -->
## Initial insights before providing final insights in Tableau
1. There is a direct correlation between account visits and cases sold
2. Luca Toni is the best performing rep and Brad is the worst performing rep
3. Luca Toni deserves a full merit increase and he is ready for a promotion. 
4. Have worst performing rep Brad do a route ride with Luca Toni so he can learn best practices. 
5. June, July, August are the busiest months while the winter/holidays months are the slowest 
6. There is a direct correlation between facing gained, POS, coolers placed and # of cases sold. To increase this KPIs I suggest that we have an incentive or make it apart of their bonus.


#### Now I will the data is ready to be visualized in Tableau
#### Below is the link to the Tableau Dashboard
## Tableau Link


