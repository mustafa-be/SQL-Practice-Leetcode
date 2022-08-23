# SQL-Practice-Leetcode


### Problem Title : Median Employee Salary
### Link : https://leetcode.com/problems/median-employee-salary/
### Description : Write a SQL query to find the median salary of each company.
### Solution :

        select 
            company,median 
        from 
            (
              select "*",
                case when "c"%2=0 then floor("c"/2.0)+1 else 0 end as "row1",
                ceil("c"/2.0)::Integer as "row2"  
              from
              (
                select 
                    id,
                    salary as "median",
                    company, 
                    count(*) over (partition by company) as "c", 
                    row_number() over (partition by company order by salary) as "r"  
                 from 
                    Employee
              ) drv
            ) drv1
        where 
          r = row1 or r = row2;
