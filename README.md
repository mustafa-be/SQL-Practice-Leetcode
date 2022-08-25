# SQL-Practice-Leetcode


### Problem Title : Median Employee Salary  
**Link** : https://leetcode.com/problems/median-employee-salary/   
**Description** : Write a SQL query to find the median salary of each company.  
**Solution** :  

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


### Problem Title : Second Highest Salary  
**Link** : https://leetcode.com/problems/median-employee-salary/  
**Description** : Write an SQL query to report the second highest salary from the Employee table. If there is no second highest salary, the query should report null  
**Solution** :   
  
Mapping empty result set into a SELECT field produces NULL

        select (
                select 
                        distinct salary 
                from 
                        Employee 
                order by 
                        salary desc 
                limit 1,1
        ) as "SecondHighestSalary" ;

Easy Solution

        SELECT (CASE 
                WHEN (SELECT COUNT(DISTINCT Salary) FROM Employee) < 2 
                THEN NULL 
                ELSE (SELECT Salary FROM Employee 
                      ORDER BY Salary DESC LIMIT 1,1) 
                END) AS SecondHighestSalary

Important Link : https://leetcode.com/problems/second-highest-salary/discuss/1168444/Summary-Five-ways-to-solve-the-top-n-nth-problems 
