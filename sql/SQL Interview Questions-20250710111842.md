# SQL Interview Questions

1. **How to find Nth highest salary ?**

  

CREATE Table Employee (emp\_id INT NOT NULL IDENTITY(1,1), firstName VARCHAR(200), lastName VARCHAR(200), Gender VARCHAR(2),Salary BIGINT);

  

| emp\_id | firstName | lastName | Gender | Salary |
| ---| ---| ---| ---| --- |
| 1 | SAURAV | SUMAN | M | 100000 |
| 2 | GAURAV | SRIVASTAVA | M | 90000 |
| 3 | Lucky | GUpta | M | 60000 |
| 4 | Adersh | Mehta | M | 45000 |
| 5 | Ronav | Chaddha | M | 80000 |
| 6 | Raghav | Solanki | M | 95000 |
| 7 | Snheal | Rut | M | 195000 |
| 8 | Romalika | sen | F | 55000 |

  

**Find 2nd Highest Salary**

  

SELECT \* FROM Employee ORDER BY Salary DESC

  

| 7 | Snheal | Rut | M | 195000 |
| ---| ---| ---| ---| --- |
| 1 | SAURAV | SUMAN | M | 100000 |
| 6 | Raghav | Solanki | M | 95000 |
| 2 | GAURAV | SRIVASTAVA | M | 90000 |
| 5 | Ronav | Chaddha | M | 80000 |
| 3 | Lucky | GUpta | M | 60000 |
| 8 | Romalika | sen | F | 55000 |
| 4 | Adersh | Mehta | M | 45000 |

  

SELECT MAX(Salary) AS AS MaxSal FROM FROM Employee

| MaxSal |
| --- |
| 195000 |

SELECT MAX(Salary) AS SecondMaxSal FROM Employee

WHERE Salary<( SELECT MAX(Salary) AS MaxSal FROM Employee )

| SecondMaxSal |
| --- |
| 100000 |

  

**Now try to Find nth Highest Salary with different Approaches**

  

**Subquery Approach**

  

SELECT TOP 2 Salary FROM Employee ORDER BY Salary DESC

| Salary |
| --- |
| 195000 |
| 100000 |

**2th highest salary**

SELECT TOP 1 Salary FROM

(

SELECT TOP 2 Salary FROM Employee ORDER BY Salary DESC

) Result ORDER BY Salary

| Salary |
| --- |
| 100000 |

  

**3rd highest salary**

SELECT TOP 1 Salary FROM

(

SELECT TOP 3 Salary FROM Employee ORDER BY Salary DESC

) Result ORDER BY Salary

| Salary |
| --- |
| 95000 |

| 7 | Snheal | Rut | M | 195000 |
| ---| ---| ---| ---| --- |
| 1 | SAURAV | SUMAN | M | 100000 |
| 6 | Raghav | Solanki | M | 95000 |
| 2 | GAURAV | SRIVASTAVA | M | 90000 |
| 5 | Ronav | Chaddha | M | 80000 |
| 3 | Lucky | GUpta | M | 60000 |
| 8 | Romalika | sen | F | 55000 |
| 4 | Adersh | Mehta | M | 45000 |

  

**COMMON TABLE EXPRESSION APPROACH** **WITH CTE**

  

SELECT Salary, DENSE\_RANK() OVER (ORDER BY Salary DESC) AS DENSERANK FROM Employee

  

**DENSE\_RANK()**

*   It's ranking row over given instruction like OVER (ORDER BY Salary DESC) AS DENSERANK

| Salary | DENSERANK |
| ---| --- |
| 195000 | 1 |
| 195000 | 1 |
| 100000 | 2 |
| 95000 | 3 |
| 90000 | 4 |
| 80000 | 5 |
| 60000 | 6 |
| 55000 | 7 |
| 55000 | 7 |
| 45000 | 8 |

  

**Now We can apply WITH CTE**

  

**DENSERANK** **Will be** **nth** **SALARY**

  

SELECT \* FROM Employee ORDER BY Salary DESC

  

GO

  

WITH Results AS

(

SELECT Salary, DENSE\_RANK() OVER (ORDER BY Salary DESC) AS DENSERANK

FROM Employee

)

SELECT TOP 1 Salary FROM Results

WHERE Results.DENSERANK=1 or N-TH

| emp\_id | firstName | lastName | Gender | Salary |
| ---| ---| ---| ---| --- |
| 7 | Snheal | Rut | M | 195000 |
| 10 | Bharat | Saxena | M | 195000 |
| 1 | SAURAV | SUMAN | M | 100000 |
| 6 | Raghav | Solanki | M | 95000 |
| 2 | GAURAV | SRIVASTAVA | M | 90000 |
| 5 | Ronav | Chaddha | M | 80000 |
| 3 | Lucky | GUpta | M | 60000 |
| 8 | Romalika | sen | F | 55000 |
| 9 | Ram | Srivastava | M | 55000 |
| 4 | Adersh | Mehta | M | 45000 |

| Salary |
| --- |
| 95000 |

  

**CAN WE APPLY** **ROW\_NUMBER()** **AS WELL**

*   No it's Big No because sequential numbering to every row

  

SELECT Salary, ROW\_NUMBER() OVER (ORDER BY Salary DESC) AS ROWNUMBER

FROM Employee

| Salary | ROWNUMBER |
| ---| --- |
| 195000 | 1 |
| 195000 | 2 |
| 100000 | 3 |
| 95000 | 4 |
| 90000 | 5 |
| 80000 | 6 |
| 60000 | 7 |
| 55000 | 8 |
| 55000 | 9 |
| 45000 | 10 |

**So, I want 3rd Highest Salary**

*   But we will get 2nd one.
*   **ROW\_NUMBER()** Only going to work when there is no **diplicate data**.

  

SELECT \* FROM Employee ORDER BY Salary DESC

  

GO

  

WITH Results AS

(

SELECT Salary, ROW\_NUMBER() OVER (ORDER BY Salary DESC) AS ROWNUMBER

FROM Employee

)

SELECT TOP 1 Salary FROM Results

WHERE Results.ROWNUMBER=3 or N-TH

  

| emp\_id | firstName | lastName | Gender | Salary |
| ---| ---| ---| ---| --- |
| 7 | Snheal | Rut | M | 195000 |
| 10 | Bharat | Saxena | M | 195000 |
| 1 | SAURAV | SUMAN | M | 100000 |
| 6 | Raghav | Solanki | M | 95000 |
| 2 | GAURAV | SRIVASTAVA | M | 90000 |
| 5 | Ronav | Chaddha | M | 80000 |
| 3 | Lucky | GUpta | M | 60000 |
| 8 | Romalika | sen | F | 55000 |
| 9 | Ram | Srivastava | M | 55000 |
| 4 | Adersh | Mehta | M | 45000 |

**we want 3rd highest but we got 2nd highest because of ROW\_NUMBER**

| Salary |
| --- |
| 100000 |

  

**2\. SQL Query to get organization to hierarchy.**

  

To get the best out of this tut, the following concepts need to be understood first. These are already discussed in [SQL Server Tutorial](https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB).

1. [Self-Join](https://www.youtube.com/watch?v=qnYSN_7qwgg)

2. [CTE](https://www.youtube.com/watch?v=ZXB5b-7HJHk)

3. [Recursive CTE](https://www.youtube.com/watch?v=GGoV0wTMCg0)

  

CREATE TABLE EmployeeManagerTable (EmployeeId INT NOT NULL IDENTITY(1,1),

employeeName VARCHAR(200),managerId BIGINT);

  

SELECT \* FROM EmployeeManagerTable

  

| EmployeeId | employeeName | managerId |
| ---| ---| --- |
| 1 | John | 5 |
| 2 | Mark | 8 |
| 3 | Steve | 8 |
| 4 | Tom | 3 |
| 5 | Lara | 8 |
| 6 | Simon | 2 |
| 7 | David | 4 |
| 8 | Ben | NULL |
| 9 | Stacy | 2 |
| 10 | Sam | 5 |

**Here is the problem definition:**

1. Employees table contains the following columns 

    a) EmployeeId, 

    b) EmployeeName 

    c) ManagerId 

2. If an EmployeeId is passed, the query should list down the entire organization hierarchy i.e who is the manager of the EmployeeId passed and who is managers manager and so on till full hierarchy is listed.

  

![](https://t9016373936.p.clickup-attachments.com/t9016373936/6d806d4e-f4f4-46da-b56f-0ed6d5334f57/image.png)

  

**For example,** 

**Scenario 1: If we pass David's EmployeeId to the query, then it should display the organization hierarchy as shown below.**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiLHZNWTwYkqGzEtiUnvIzxk_mu7XomFhcrFodZ3OvUVccJBki8p4k4gcaZYpnFe1Q2WO3XnlEzxFnUhPavwK0rVVS-wWz5sH9PyAAj4IyqLaAmZenlMbCK2yI4luyfoMCRNZLajbdCdrsL/s1600/organization+hierarchy+sql+server.png)

  

**Scenario 2: If we pass Lara's EmployeeId to the query, then it should display the organization hierarchy as shown below.**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVCy4D7-ou1S_UxPc5ioPTp7X_5rXXty2JOpqYQvfAP_FMjVYJ4-ZNQANozj0PE3uevTL-PIU_uHy1tKzCsJMA-lkbq-Ju1hhiAujUsjyajxGYQNLCy6Vk1X92fHWQh8OJDzFkU4eAImeW/s1600/organizational+hierarchy+sql.png)

  
  

\--David Id with managerId=3

DECLARE @ID BIGINT\=7;

  

WITH EmployeeManagerCTC AS

(

SELECT EmployeeId,employeeName,managerId

FROM EmployeeManagerTable

WHERE EmployeeId=@ID

UNION ALL

  

SELECT EmployeeManagerTable.EmployeeId, EmployeeManagerTable.employeeName, EmployeeManagerTable.managerId

FROM EmployeeManagerTable

JOIN EmployeeManagerCTC ON EmployeeManagerTable.EmployeeId=EmployeeManagerCTC.managerId

  

)

  

SELECT \* FROM EmployeeManagerCTC;

  

| EmployeeId | employeeName | managerId |
| ---| ---| --- |
| 7 | David | 4 |
| 4 | Tom | 3 |
| 3 | Steve | 8 |
| 8 | Ben | NULL |

  
  

**After Applying SELF JOIN**

\--David Id with managerId=3

DECLARE @ID BIGINT\=7;

  

WITH EmployeeManagerCTC AS

(

SELECT EmployeeId,employeeName,managerId

FROM EmployeeManagerTable

WHERE EmployeeId=@ID

  

UNION ALL

  

SELECT EmployeeManagerTable.EmployeeId, EmployeeManagerTable.employeeName, EmployeeManagerTable.managerId

FROM EmployeeManagerTable

JOIN EmployeeManagerCTC ON EmployeeManagerTable.EmployeeId=EmployeeManagerCTC.managerId

)

  

\--SELF JOIN

SELECT E1.EmployeeId, E1.employeeName, E2.employeeName AS ManagerName

FROM EmployeeManagerCTC E1

JOIN EmployeeManagerCTC E2 ON E1.managerId=E2.EmployeeId

  

| EmployeeId | employeeName | ManagerName |
| ---| ---| --- |
| 7 | David | Tom |
| 4 | Tom | Steve |
| 3 | Steve | Ben |

**HERE WHAT Happens We dont't GET THE BEN DETAILS**

**Applying Left Join**

  
  
  

\--David Id with managerId=3

DECLARE @ID BIGINT\=7;

  

WITH EmployeeManagerCTC AS

(

  

 -- Anchor

SELECT EmployeeId,employeeName,managerId

FROM EmployeeManagerTable

WHERE EmployeeId=@ID

  

UNION ALL

  

\-- Recursive Member

SELECT EmployeeManagerTable.EmployeeId, EmployeeManagerTable.employeeName, EmployeeManagerTable.managerId

FROM EmployeeManagerTable

JOIN EmployeeManagerCTC ON EmployeeManagerTable.EmployeeId=EmployeeManagerCTC.managerId

)

  

\--APPLying LEFT JOIN

SELECT E1.EmployeeId, E1.employeeName, ISNULL(E2.employeeName,'No Boss') AS ManagerName

FROM EmployeeManagerCTC E1

LEFT JOIN EmployeeManagerCTC E2 ON E1.managerId=E2.EmployeeId

  

| EmployeeId | employeeName | ManagerName |
| ---| ---| --- |
| 7 | David | Tom |
| 4 | Tom | Steve |
| 3 | Steve | Ben |
| 8 | Ben | No Boss |

**Let's now discuss how the CTE executes line by line.**

Step 1: Execute the **anchor** part and get result R0

Step 2: Execute the **recursive** member using R0 as input and generate result R1

Step 3: Execute the **recursive** member using R1 as input and generate result R2

Step 4: **Recursion** goes on until the **recursive** member output becomes NULL

Step 5: Finally apply UNION ALL on all the results to produce the final output

  
  

**Example 2**

\--Lara Id with managerId=8

DECLARE @ID BIGINT\=5;

  

WITH EmployeeManagerCTC AS

(

SELECT EmployeeId,employeeName,managerId

FROM EmployeeManagerTable

WHERE EmployeeId=@ID

  

UNION ALL

  

SELECT EmployeeManagerTable.EmployeeId, EmployeeManagerTable.employeeName, EmployeeManagerTable.managerId

FROM EmployeeManagerTable

JOIN EmployeeManagerCTC ON EmployeeManagerTable.EmployeeId=EmployeeManagerCTC.managerId

)

  

\--APPLying LEFT JOIN

SELECT E1.EmployeeId, E1.employeeName, ISNULL(E2.employeeName,'No Boss') AS ManagerName

FROM EmployeeManagerCTC E1

LEFT JOIN EmployeeManagerCTC E2 ON E1.managerId=E2.EmployeeId

| EmployeeId | employeeName | ManagerName |
| ---| ---| --- |
| 5 | Lara | Ben |
| 9 | Ben | No Boss |

  

**3\. DELETE Duplicate Rows In SQL.**

  

CREATE TABLE #EmpTemp

(

ID int,

FirstName nvarchar(50),

LastName nvarchar(50),

Gender nvarchar(50),

Salary int

)

GO

  

Insert into #EmpTemp values (1, 'Mark', 'Hastings', 'Male', 60000)

Insert into #EmpTemp values (1, 'Mark', 'Hastings', 'Male', 60000)

Insert into #EmpTemp values (1, 'Mark', 'Hastings', 'Male', 60000)

Insert into #EmpTemp values (2, 'Mary', 'Lambeth', 'Female', 30000)

Insert into #EmpTemp values (2, 'Mary', 'Lambeth', 'Female', 30000)

Insert into #EmpTemp values (3, 'Ben', 'Hoskins', 'Male', 70000)

Insert into #EmpTemp values (3, 'Ben', 'Hoskins', 'Male', 70000)

Insert into #EmpTemp values (3, 'Ben', 'Hoskins', 'Male', 70000)

  

SELECT \* FROM #EmpTemp

  

| ID | FirstName | LastName | Gender | Salary |
| ---| ---| ---| ---| --- |
| 1 | Mark | Hastings | Male | 60000 |
| 1 | Mark | Hastings | Male | 60000 |
| 1 | Mark | Hastings | Male | 60000 |
| 2 | Mary | Lambeth | Female | 30000 |
| 2 | Mary | Lambeth | Female | 30000 |
| 3 | Ben | Hoskins | Male | 70000 |
| 3 | Ben | Hoskins | Male | 70000 |
| 3 | Ben | Hoskins | Male | 70000 |

  

WITH TempEmployeeCTE AS

(

SELECT \*, ROW\_NUMBER() OVER (PARTITION BY ID ORDER BY ID) AS ROWNUMBER FROM #EmpTemp

)

  

SELECT \* FROM TempEmployeeCTE;

  

| ID | FirstName | LastName | Gender | Salary | ROWNUMBER |
| ---| ---| ---| ---| ---| --- |
| 1 | Mark | Hastings | Male | 60000 | 1 |
| 1 | Mark | Hastings | Male | 60000 | 2 |
| 1 | Mark | Hastings | Male | 60000 | 3 |
| 2 | Mary | Lambeth | Female | 30000 | 1 |
| 2 | Mary | Lambeth | Female | 30000 | 2 |
| 3 | Ben | Hoskins | Male | 70000 | 1 |
| 3 | Ben | Hoskins | Male | 70000 | 2 |
| 3 | Ben | Hoskins | Male | 70000 | 3 |

*   So, Here what happens every partition start from 1 and goes so, on.
*   If we are taking id 1 it start's from 1 and goes up to 2
*   We want to remove duplicate row.
*   So, basically we can remove row which has row number greater than 1

  

WITH TempEmployeeCTE AS

(

SELECT \*, ROW\_NUMBER() OVER (PARTITION BY ID ORDER BY ID) AS ROWNUMBER FROM #EmpTemp

)

DELETE TempEmployeeCTE WHERE ROWNUMBER>1

GO

  

SELECT \* FROM #EmpTemp

  

| ID | FirstName | LastName | Gender | Salary |
| ---| ---| ---| ---| --- |
| 1 | Mark | Hastings | Male | 60000 |
| 2 | Mary | Lambeth | Female | 30000 |
| 3 | Ben | Hoskins | Male | 70000 |

  

**4\. SQL Query to Find Employee Who hired in last N Months OR Years OR DAY.**

  

Create table #TempEmployees

(

     ID int primary key identity,

     FirstName nvarchar(50),

     LastName nvarchar(50),

     Gender nvarchar(50),

     Salary int,

     HireDate DateTime

)

GO

  

Insert into #TempEmployees values('Mark','Hastings','Male',60000,'5/10/2024')

Insert into #TempEmployees values('Steve','Pound','Male',45000,'4/20/2024')

Insert into #TempEmployees values('Ben','Hoskins','Male',70000,'4/5/2024')

Insert into #TempEmployees values('Philip','Hastings','Male',45000,'3/11/2024')

Insert into #TempEmployees values('Mary','Lambeth','Female',30000,'3/10/2024')

Insert into #TempEmployees values('Valarie','Vikings','Female',35000,'2/9/2024')

Insert into #TempEmployees values('John','Stanmore','Male',80000,'2/22/2024')

Insert into #TempEmployees values('Able','Edward','Male',5000,'1/22/2024')

Insert into #TempEmployees values('Emma','Nan','Female',5000,'1/14/2024')

Insert into #TempEmployees values('Jd','Nosin','Male',6000,'1/10/2023')

Insert into #TempEmployees values('Todd','Heir','Male',7000,'2/14/2023')

Insert into #TempEmployees values('San','Hughes','Male',7000,'3/15/2023')

Insert into #TempEmployees values('Nico','Night','Male',6500,'4/19/2023')

Insert into #TempEmployees values('Martin','Jany','Male',5500,'5/23/2023')

Insert into #TempEmployees values('Mathew','Mann','Male',4500,'6/23/2023')

Insert into #TempEmployees values('Baker','Barn','Male',3500,'7/23/2023')

Insert into #TempEmployees values('Mosin','Barn','Male',8500,'8/21/2023')

Insert into #TempEmployees values('Rachel','Aril','Female',6500,'9/14/2023')

Insert into #TempEmployees values('Pameela','Son','Female',4500,'10/14/2023')

Insert into #TempEmployees values('Thomas','Cook','Male',3500,'11/14/2023')

Insert into #TempEmployees values('Malik','Md','Male',6500,'12/14/2023')

Insert into #TempEmployees values('Josh','Anderson','Male',4900,'5/1/2024')

Insert into #TempEmployees values('Geek','Ging','Male',2600,'4/1/2024')

Insert into #TempEmployees values('Sony','Sony','Male',2900,'4/30/2024')

Insert into #TempEmployees values('Aziz','Sk','Male',3800,'3/1/2024')

Insert into #TempEmployees values('Amit','Naru','Male',3100,'3/31/2024')

GO

  

SELECT \* FROM #TempEmployees

  

**Query To Find the Employee who hired in last 10 months**

  

SELECT \*, DATEDIFF(MONTH, HireDate,GETDATE()) AS MonthDiff FROM #TempEmployees

WHERE DATEDIFF(MONTH, HireDate,GETDATE()) BETWEEN 1 AND 10

  

| ID | FirstName | LastName | Gender | Salary | HireDate | MonthDiff |
| ---| ---| ---| ---| ---| ---| --- |
| 1 | Mark | Hastings | Male | 60000 | 5/10/2024 0:00 | 5 |
| 2 | Steve | Pound | Male | 45000 | 4/20/2024 0:00 | 6 |
| 3 | Ben | Hoskins | Male | 70000 | 4/5/2024 0:00 | 6 |
| 4 | Philip | Hastings | Male | 45000 | 3/11/2024 0:00 | 7 |
| 5 | Mary | Lambeth | Female | 30000 | 3/10/2024 0:00 | 7 |
| 6 | Valarie | Vikings | Female | 35000 | 2/9/2024 0:00 | 8 |
| 7 | John | Stanmore | Male | 80000 | 2/22/2024 0:00 | 8 |
| 8 | Able | Edward | Male | 5000 | 1/22/2024 0:00 | 9 |
| 9 | Emma | Nan | Female | 5000 | 1/14/2024 0:00 | 9 |
| 21 | Malik | Md | Male | 6500 | 12/14/2023 0:00 | 10 |
| 22 | Josh | Anderson | Male | 4900 | 5/1/2024 0:00 | 5 |
| 23 | Geek | Ging | Male | 2600 | 4/1/2024 0:00 | 6 |
| 24 | Sony | Sony | Male | 2900 | 4/30/2024 0:00 | 6 |
| 25 | Aziz | Sk | Male | 3800 | 3/1/2024 0:00 | 7 |
| 26 | Amit | Naru | Male | 3100 | 3/31/2024 0:00 | 7 |

  

**4\. How to transform ROWS INTO Columns IN SQL Server.**

  

Create Table #CountriesTemp

(

     Country nvarchar(50),

     City nvarchar(50)

)

GO

  

Insert into #CountriesTemp values ('USA','New York')

Insert into #CountriesTemp values ('USA','Houston')

Insert into #CountriesTemp values ('USA','Dallas')

  

Insert into #CountriesTemp values ('India','Hyderabad')

Insert into #CountriesTemp values ('India','Bangalore')

Insert into #CountriesTemp values ('India','New Delhi')

  

Insert into #CountriesTemp values ('UK','London')

Insert into #CountriesTemp values ('UK','Birmingham')

Insert into #CountriesTemp values ('UK','Manchester')

  

SELECT \* FROM #CountriesTemp

  

| Country | City |
| ---| --- |
| USA | New York |
| USA | Houston |
| USA | Dallas |
| India | Hyderabad |
| India | Bangalore |
| India | New Delhi |
| UK | London |
| UK | Birmingham |
| UK | Manchester |

  

**Here is the interview question:**

**Write a sql query to transpose rows to columns. The output should be as shown below.**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj34HAEenup2WM76sJZxN3lPhzO9sqN5fULsQOoGpQR5-MM3EgePKcHlEhwf-6GaER5-6jWRt2MvBHTDLVHgL4stkppOzsg-XZRxBXlrWXGgoIZ6C6AKD9XsF8GzvCWv_yspuEUQrLbhXOk/s1600/transform+rows+to+columns+sql.png)

  

SELECT Country, City ,

ROW\_NUMBER() OVER ( PARTITION BY Country ORDER BY Country ) AS CountryRowNumber

FROM #CountriesTemp

  

| Country | City | CountryRowNumber |
| ---| ---| --- |
| India | Hyderabad | 1 |
| India | Bangalore | 2 |
| India | New Delhi | 3 |
| UK | London | 1 |
| UK | Birmingham | 2 |
| UK | Manchester | 3 |
| USA | New York | 1 |
| USA | Houston | 2 |
| USA | Dallas | 3 |

  

SELECT Country, City ,

'City '+CAST( (ROW\_NUMBER() OVER ( PARTITION BY Country ORDER BY Country )) AS VARCHAR(10)) AS CitySequenceNumber

FROM #CountriesTemp

  

| Country | City | CitySequenceNumber |
| ---| ---| --- |
| India | Hyderabad | City 1 |
| India | Bangalore | City 2 |
| India | New Delhi | City 3 |
| UK | London | City 1 |
| UK | Birmingham | City 2 |
| UK | Manchester | City 3 |
| USA | New York | City 1 |
| USA | Houston | City 2 |
| USA | Dallas | City 3 |

  

SELECT Country, City1,City2,City3

FROM

(

SELECT Country, City ,

'City'+CAST( (ROW\_NUMBER() OVER ( PARTITION BY Country ORDER BY Country )) AS VARCHAR(10)) AS CitySequenceNumber

FROM #CountriesTemp

  

) Temp

PIVOT

(

MAX(City)

For CitySequenceNumber IN(City1,City2,City3)

) PIV

  

| Country | City1 | City2 | City3 |
| ---| ---| ---| --- |
| India | Hyderabad | Bangalore | New Delhi |
| UK | London | Birmingham | Manchester |
| USA | New York | Houston | Dallas |

  

**What happens if we add on more city in INDIA**

  

INSERT INTO #CountriesTemp VALUES ('India','Patna')

SELECT \* FROM #CountriesTemp

| Country | City |
| ---| --- |
| USA | New York |
| USA | Houston |
| USA | Dallas |
| India | Hyderabad |
| India | Bangalore |
| India | New Delhi |
| UK | London |
| UK | Birmingham |
| UK | Manchester |
| India | Patna |

SELECT Country, City1,City2,City3,City4

FROM

(

SELECT Country, City ,

'City'+CAST( (ROW\_NUMBER() OVER ( PARTITION BY Country ORDER BY Country )) AS VARCHAR(10)) AS CitySequenceNumber

FROM #CountriesTemp

  

) Temp

PIVOT

(

MAX(City)

For CitySequenceNumber IN(City1,City2,City3,City4)

) PIV

  

| Country | City1 | City2 | City3 | City4 |
| ---| ---| ---| ---| --- |
| India | Hyderabad | Bangalore | New Delhi | Patna |
| UK | London | Birmingham | Manchester | NULL |
| USA | New York | Houston | Dallas | NULL |

  

**5\. SQL Query to find rows that contain only numeric data.**

  

Create Table TestTable

(

     ID int identity primary key,

     Value nvarchar(50)

)

  

Insert into TestTable values ('123')

Insert into TestTable values ('ABC')

Insert into TestTable values ('DEF')

Insert into TestTable values ('901')

Insert into TestTable values ('JKL')

  
  

![](https://t9016373936.p.clickup-attachments.com/t9016373936/0cf33605-a35a-4b6a-a9c1-17b824224f94/image.png)

  

This is very easy to achieve. If you have used ISNUMERIC() function in SQL Server, then you already know the answer. Here is the query

**Select** **Value** **from** **TestTable** **Where** **ISNUMERIC****(****Value****)** **\=** **1**

  

ISNUMERIC function returns 1 when the input expression evaluates to a valid numeric data type, otherwise it returns 0.

  

![](https://t9016373936.p.clickup-attachments.com/t9016373936/bce65840-06dc-42de-be77-c67f9e64c4d0/image.png)

  

**For the list of all valid numeric data types in SQL Server please visit the following MSDN link.**

[http://technet.microsoft.com/en-us/library/ms186272(v=sql.110).aspx](http://technet.microsoft.com/en-us/library/ms186272(v=sql.110).aspx)

  

**6\. SQL Query to find Department With Highest Number Of Employee.**

  

Create Table #Departments

(

     DepartmentID int primary key,

     DepartmentName nvarchar(50)

)

GO

  

Create Table #Employees

(

     EmployeeID int primary key,

     EmployeeName nvarchar(50),

     DepartmentID int foreign key references Departments(DepartmentID)

)

GO

  

Insert into #Departments values (1, 'IT')

Insert into #Departments values (2, 'HR')

Insert into #Departments values (3, 'Payroll')

\--Insert into #Departments values (4, 'Admin')

GO

  

Insert into #Employees values (1, 'Mark', 1)

Insert into #Employees values (2, 'John', 1)

Insert into #Employees values (3, 'Mike', 1)

Insert into #Employees values (4, 'Mary', 2)

Insert into #Employees values (5, 'Stacy', 3)

GO

  

| DepartmentID | DepartmentName |
| ---| --- |
| 1 | IT |
| 2 | HR |
| 3 | Payroll |

| EmployeeID | EmployeeName | DepartmentID |
| ---| ---| --- |
| 1 | Mark | 1 |
| 2 | John | 1 |
| 3 | Mike | 1 |
| 4 | Mary | 2 |
| 5 | Stacy | 3 |

  

SELECT d.DepartmentName,COUNT(\*) AS EmployeeCount

FROM #Employees e

JOIN #Departments d ON d.DepartmentID=e.DepartmentID

GROUP BY DepartmentName

| DepartmentName | EmployeeCount |
| ---| --- |
| HR | 1 |
| IT | 3 |
| Payroll | 1 |

  

SELECT TOP 1 d.DepartmentName,COUNT(\*) AS EmployeeCount

FROM #Employees e

JOIN #Departments d ON d.DepartmentID=e.DepartmentID

GROUP BY DepartmentName

ORDER BY EmployeeCount

| DepartmentName | EmployeeCount |
| ---| --- |
| HR | 1 |

  

SELECT TOP 1 d.DepartmentName

FROM #Employees e

JOIN #Departments d ON d.DepartmentID=e.DepartmentID

GROUP BY DepartmentName

ORDER BY COUNT(\*)

| DepartmentName |
| --- |
| HR |

  

**7\. Difference Between Inner JOIN AND LEFT JOIN.**

  

| DepartmentID | DepartmentName |
| ---| --- |
| 1 | IT |
| 2 | HR |
| 3 | Payroll |
| 4 | Admin |

  

| EmployeeID | EmployeeName | DepartmentID |
| ---| ---| --- |
| 1 | Mark | 1 |
| 2 | John | 1 |
| 3 | Mike | 1 |
| 4 | Mary | 2 |
| 5 | Stacy | 3 |
| 6 | Pam | NULL |

![](https://t9016373936.p.clickup-attachments.com/t9016373936/f74f9cda-2b04-43ed-9cc9-56c75490bea8/image.png)

  
  

**\--Applying INNER JOIN OR JOIN**

SELECT e.EmployeeID, e.EmployeeName, e.DepartmentID,d.DepartmentName

FROM #Employees e

JOIN #Departments d ON d.DepartmentID = e.DepartmentID;

| EmployeeID | EmployeeName | DepartmentID | DepartmentName |
| ---| ---| ---| --- |
| 1 | Mark | 1 | IT |
| 2 | John | 1 | IT |
| 3 | Mike | 1 | IT |
| 4 | Mary | 2 | HR |
| 5 | Stacy | 3 | Payroll |

  
  

**\--Applying INNER JOIN OR JOIN**

SELECT e.EmployeeID, e.EmployeeName, e.DepartmentID,d.DepartmentName

FROM #Employees e

LEFT JOIN #Departments d ON d.DepartmentID = e.DepartmentID;

| EmployeeID | EmployeeName | DepartmentID | DepartmentName |
| ---| ---| ---| --- |
| 1 | Mark | 1 | IT |
| 2 | John | 1 | IT |
| 3 | Mike | 1 | IT |
| 4 | Mary | 2 | HR |
| 5 | Stacy | 3 | Payroll |
| 6 | Pam | NULL | NULL |

**In general there could be several questions on JOINS in a sql server interview. If we understand the basics of JOINS properly, then answering any JOINS related questions should be a cakewalk.**

**What is the difference between INNER JOIN and RIGHT JOIN** 

*   INNER JOIN returns only the matching rows between the tables involved in the JOIN, where as RIGHT JOIN returns all the rows from the right table including the NON-MATCHING rows.

  

**What is the difference between INNER JOIN and FULL JOIN** 

*   FULL JOIN returns all the rows from both the left and right tables including the NON-MATCHING rows.

  

**What is the Difference between INNER JOIN and JOIN**

*   There is no difference they are exactly the same. Similarly there is also no difference between 

**LEFT JOIN and LEFT OUTER JOIN**

**RIGHT JOIN and RIGHT OUTER JOIN**

**FULL JOIN and FULL OUTER JOIN**

  

**8\. Real Time Example OF RIGHT JOIN.**

  

SELECT d.DepartmentName, COUNT(e.EmployeeID) AS EmployeeCount

FROM #Employees e

JOIN #Departments d ON d.DepartmentID=e.DepartmentID

GROUP BY d.DepartmentName

ORDER BY EmployeeCount

| DepartmentName | EmployeeCount |
| ---| --- |
| HR | 1 |
| Payroll | 1 |
| IT | 3 |

  

**\--Applying Right Join**

SELECT d.DepartmentName, COUNT(e.EmployeeID) AS EmployeeCount

FROM #Employees e

RIGHT JOIN #Departments d ON d.DepartmentID=e.DepartmentID

GROUP BY d.DepartmentName

ORDER BY EmployeeCount

| DepartmentName | EmployeeCount |
| ---| --- |
| Admin | 0 |
| HR | 1 |
| Payroll | 1 |
| IT | 3 |

###   

### 9\. Can we join two tables without primary foreign key relation

*   Yes, we can join two tables without primary foreign key relation as long as the column values involved in the join can be converted to one type.

  

**The obious next question is, if primary foreign key relation is not mandatory for 2 tables to be joined then what is the use of these keys?**

  

*   **Primary key** enforces uniqueness of values over one or more columns. Since ID is not a primary key in Departments table, 2 or more departments may end up having same ID value, which makes it impossible to distinguish between them based on the ID column value.

  

*   **Foreign key** enforces referential integrity. Without foreign key constraint on DepartmentId column in Employees table, it is possible to insert a row into Employees table with a value for DepartmentId column that does not exist in Departments table.

  
### 10\. Difference between blocking and deadlocking
  

**Blocking** : Occurs if a transaction tries to acquire an incompatible lock on a resource that another transaction has already locked. The blocked transaction remains blocked until the blocking transaction releases the lock. The following diagram explains this.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhWnCJpeATK0BEiqSQBjzi7SHaXnewBSeO00tLoyggKbsushEiaeRJX_1FY_8k_VKX7MToquZ-0uCK6GH6YTRp8Vq3e1JDa6_eBoxocwl6mVTzxxV6dCmEwiiHUoxx8-Y3v6mb7gzcPOGI/s1600/blocking+in+sql+server.png)

  

Example : Open 2 instances of SQL Server Management studio. From the first window execute Transaction 1 code and from the second window execute Transaction 2 code. Notice that Transaction 2 is blocked by Transaction 1. Transaction 2 is allowed to move forward only when Transaction 1 completes.

  

\--Transaction 1

Begin Tran

Update TableA set Name\='Mark Transaction 1' where Id \= 1

  

\--NOT COMMITED YET

Commit Transaction

  

\--Transaction 2

\--Withing for transaction 1 to be complete

Begin Tran

Update TableA set Name\='Mark Transaction 2' where Id \= 1

Commit Transaction

  

**Deadlock** : Occurs when two or more transactions have a resource locked, and each transaction requests a lock on the resource that another transaction has already locked. Neither of the transactions here can move forward, as each one is waiting for the other to release the lock. So in this case, SQL Server intervenes and ends the deadlock by cancelling one of the transactions, so the other transaction can move forward. The following diagram explains this.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgFD71hJEiVzBSwsFI87-sRWBcBIqpIKcXkYVX-QEaGJRQMcmvS6bFqmj2SEDkLFT0C4GOC6W17Ze5QhOvOSUHThtZNB_eOUbCxdpy0NBH-Sx3ilkYkzhww6XzpqD2Rvc0UrWf_dYnkiqE/s1600/blocking+vs+deadlock+in+sql+server.png)

  

Example : Open 2 instances of SQL Server Management studio. From the first window execute Transaction 1 code and from the second window execute Transaction 2 code. Notice that there is a deadlock between Transaction 1 and Transaction 2.

  

\-- Transaction 1

Begin Tran

Update TableA Set Name \= 'Mark Transaction 1' where Id \= 1

 \-- From Transaction 2 window execute the first update statement

 pdate TableB Set Name \= 'Mary Transaction 1' where Id \= 1

 \-- From Transaction 2 window execute the second update statement

Commit Transaction

  

\-- Transaction 2

Begin Tran

Update TableB Set Name \= 'Mark Transaction 2' where Id \= 1

\-- From Transaction 1 window execute the second update statement

Update TableA Set Name \= 'Mary Transaction 2' where Id \= 1

\-- After a few seconds notice that one of the transactions complete

\-- successfully while the other transaction is made the deadlock victim

Commit Transaction

  
### **11\. Sql query to select all names that start with a given letter without like operator**
  

If the interviewer has not mentioned not to use LIKE operator, we would have written the query using the LIKE operator as shown below.

  

SELECT \* FROM Students WHERE Name LIKE 'M%'

  

We can use any one of the following 3 SQL Server functions, to achieve exactly the same thing

CHARINDEX

LEFT

SUBSTRING

  

The following 3 queries retrieve all student rows whose Name starts with letter 'M'. Notice none of the queries are using the LIKE operator.

  

SELECT \* FROM Students WHERE CHARINDEX('M',Name) \= 1

SELECT \* FROM Students WHERE LEFT(Name, 1) \= 'M'

SELECT \* FROM Students WHERE SUBSTRING(Name, 1, 1) \= 'M'

  
### **12\. SQL script to insert into many to many table**
  

Create table #Students

(

     Id int primary key identity,

     StudentName nvarchar(50)

)

Go

  

Create table #Courses

(

     Id int primary key identity,

     CourseName nvarchar(50)

)

Go

  

\--Bridge Table

Create table #StudentCourses

(

     StudentId int not null foreign key references Students(Id),

     CourseId int not null foreign key references Courses(Id)

)

Go

  

Skipping FOREIGN KEY constraint '#StudentCourses' definition for temporary table. FOREIGN KEY constraints are not enforced on local or global temporary tables.

Skipping FOREIGN KEY constraint '#StudentCourses' definition for temporary table. FOREIGN KEY constraints are not enforced on local or global temporary tables.

  

Create table Students

(

     Id int primary key identity,

     StudentName nvarchar(50)

)

Go

  

Create table Courses

(

     Id int primary key identity,

     CourseName nvarchar(50)

)

Go

  

Create table StudentCourses

(

     StudentId int not null foreign key references Students(Id),

     CourseId int not null foreign key references Courses(Id)

)

Go

  

**Answer** : To avoid duplicate student course enrolments create a composite primary key on StudentId and CourseId columns in StudentCourses table. With this composite primary key in place, if someone tries to enroll the same student in the same course again we get violation of primary key constraint error.

  

**Alter** **table** **StudentCourses**

**Add** **Constraint** **PK\_StudentCourses**

**Primary** **Key** **Clustered** **(****CourseId****,** **StudentId****)**

  

CREATE PROCEDURE spInsertIntoStudentCourses

@StudentName NVARCHAR(50),

@CourseName NVARCHAR(50)

AS

BEGIN

DECLARE @StudentId INT;

DECLARE @CourseId INT;

DECLARE @ErrorMessage NVARCHAR(4000);

  

BEGIN TRY

\-- Start a transaction

BEGIN TRANSACTION;

  

\-- Validate that @StudentName and @CourseName are not null or empty

IF (@StudentName IS NULL OR LTRIM(RTRIM(@StudentName)) = '')

BEGIN

SET @ErrorMessage = 'Error: Student name cannot be null or empty.';

SELECT @ErrorMessage AS StatusMessage;

ROLLBACK TRANSACTION;

RETURN;

END

  

IF (@CourseName IS NULL OR LTRIM(RTRIM(@CourseName)) = '')

BEGIN

SET @ErrorMessage = 'Error: Course name cannot be null or empty.';

SELECT @ErrorMessage AS StatusMessage;

ROLLBACK TRANSACTION;

RETURN;

END

  

\-- Retrieve StudentId, insert if not found

SELECT @StudentId = Id FROM Students WHERE StudentName = @StudentName;

IF (@StudentId IS NULL)

BEGIN

INSERT INTO Students (StudentName) VALUES (@StudentName);

SET @StudentId = SCOPE\_IDENTITY();

END

\-- Retrieve CourseId, insert if not found

SELECT @CourseId = Id FROM Courses WHERE CourseName = @CourseName;

IF (@CourseId IS NULL)

BEGIN

INSERT INTO Courses (CourseName) VALUES (@CourseName);

SET @CourseId = SCOPE\_IDENTITY();

END

\-- Check if the student is already enrolled in the course

IF EXISTS (SELECT 1 FROM StudentCourses WHERE StudentId = @StudentId AND CourseId = @CourseId)

BEGIN

SET @ErrorMessage = 'Error: Student is already enrolled in this course.';

SELECT @ErrorMessage AS StatusMessage;

ROLLBACK TRANSACTION;

RETURN;

END

  

\-- Insert into StudentCourses

INSERT INTO StudentCourses (StudentId, CourseId) VALUES (@StudentId, @CourseId);

  

\-- Commit transaction if everything succeeded

COMMIT TRANSACTION;

  

\-- Success message

SELECT 'Success: Student enrolled in course successfully.' AS StatusMessage;

  

END TRY

  

BEGIN CATCH

\-- Rollback transaction if an error occurs

IF XACT\_STATE() <> 0

BEGIN

ROLLBACK TRANSACTION;

END

  

\-- Capture and return the error details directly

DECLARE @ErrorMessage NVARCHAR(4000) = 'Error: ' + ERROR\_MESSAGE();

SELECT @ErrorMessage AS StatusMessage;

END CATCH

END;

  

Use the following statement to execute the stored procedure

Execute spInsertIntoStudentCourses 'Tom','C#'

  
### **13\. Sql date interview questions .**
  

Create Table #Employees

(

       ID int identity primary key,

       Name nvarchar(50),

       DateOfBirth DateTime

)

Insert Into #Employees Values ('Tom', '2018-11-19 10:36:46.520')

Insert Into #Employees Values ('Sara', '2018-11-18 11:36:26.400')

Insert Into #Employees Values ('Bob', '2017-12-22 10:40:10.300')

Insert Into #Employees Values ('Alex', '2017-12-30 9:30:20.100')

Insert Into #Employees Values ('Charlie', '2017-11-25 7:25:14.700')

Insert Into #Employees Values ('David', '2017-10-09 8:26:14.800')

Insert Into #Employees Values ('Elsa', '2017-10-09 9:40:18.900')

Insert Into #Employees Values ('George', '2018-11-15 10:35:17.600')

Insert Into #Employees Values ('Mike', '2018-11-16 9:14:17.600')

Insert Into #Employees Values ('Nancy', '2018-11-17 11:16:18.600')

  

| ID | Name | DateOfBirth |
| ---| ---| --- |
| 1 | Tom | 11/19/2018 10:36 |
| 2 | Sara | 11/18/2018 11:36 |
| 3 | Bob | 12/22/2017 10:40 |
| 4 | Alex | 12/30/2017 9:30 |
| 5 | Charlie | 11/25/2017 7:25 |
| 6 | David | 10/9/2017 8:26 |
| 7 | Elsa | 10/9/2017 9:40 |
| 8 | George | 11/15/2018 10:35 |
| 9 | Mike | 11/16/2018 9:14 |
| 10 | Nancy | 11/17/2018 11:16 |

**Write a SQL query to retrieve all people who are born on a given date (For example, 9th October 2017)**

  

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM #Employees

WHERE CAST(DateOfBirth AS DATE)='2017-10-09'

  

| ID | Name | DOB |
| ---| ---| --- |
| 6 | David | 2017-10-09 |
| 7 | Elsa | 2017-10-09 |

  

**Write a SQL query to retrieve all people who are born between 2 given dates (For example, all people born between Nov 1, 2017 and Dec 31, 2017)**

  

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM #Employees

WHERE CAST(DateOfBirth AS DATE) BETWEEN '2017-11-01' AND '2017-12-31'

  

| ID | Name | DOB |
| ---| ---| --- |
| 3 | Bob | 2017-12-22 |
| 4 | Alex | 2017-12-30 |
| 5 | Charlie | 2017-11-25 |

**Write a SQL query to retrieve all people who are born on the same day and month excluding the year (For example, 9th October)**

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM #Employees

WHERE DAY(DateOfBirth) =9 AND MONTH(DateOfBirth) =10

  

| ID | Name | DOB |
| ---| ---| --- |
| 6 | David | 2017-10-09 |
| 7 | Elsa | 2017-10-09 |

**Write a SQL query to get all people who are born in a given year (Example, all people born in the year 2017)**

  

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM    #Employees

WHERE  YEAR(DateOfBirth) \= 2017

  

| ID | Name | DOB |
| ---| ---| --- |
| 3 | Bob | 2017-12-22 |
| 4 | Alex | 2017-12-30 |
| 5 | Charlie | 2017-11-25 |
| 6 | David | 2017-10-09 |
| 7 | Elsa | 2017-10-09 |

**Write a SQL query to retrieve all people who are born yesterday**

  

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM    #Employees

WHERE  CAST(DateOfBirth AS DATE) \= DATEADD(DAY, \-1, CAST(GETDATE() AS DATE))

  

**Write a SQL query to retrieve all people who are born tommorow**

  

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM    #Employees

WHERE  CAST(DateOfBirth AS DATE) \= DATEADD(DAY, 1, CAST(GETDATE() AS DATE))

  

**To get all people who are born since yesterday (i.e all the people who are born yesterday and today)**

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM    #Employees

WHERE  CAST(DateOfBirth AS DATE) \= BETWEEN DATEADD(DAY, -1, CAST(GETDATE() AS DATE)) and CAST(GETDATE() AS DATE)

  

**To get all people who are born in the last 7 days (excluding today)**

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM    #Employees

WHERE  CAST(DateOfBirth AS DATE) \= BETWEEN DATEADD(DAY, -7, CAST(GETDATE() AS DATE)) and DATEADD(DAY, -1, CAST(GETDATE() AS DATE))

  

**To get all people who are born today .**

SELECT ID,Name,CAST(DateOfBirth AS DATE) AS DOB

FROM    #Employees

WHERE  CAST(DateOfBirth AS DATE) \= CAST(GETDATE() AS DATE))

  

### 14\. How and why a sql inner left right full and even cross join returns the same row count
  

We have 2 tables - TableA and TableB. Both the tables have just one column each. TableA has 2 rows and TableB has 3 rows.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLGEZU32Ex6Wt4Ci94-3KLp38x1Qha30HYpNwXc7PcwztXM9Q9MOB8pqM_qrkDCywJmB1wASkCiXkxBR-Ty1xNGslnanOXTflAUF5vqO1_-AxZYO6RLjZI82-pDOxONMndJJ2OMIsNoc7R/s16000/sql-inner-join-cross-join-return-same-count-how.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLGEZU32Ex6Wt4Ci94-3KLp38x1Qha30HYpNwXc7PcwztXM9Q9MOB8pqM_qrkDCywJmB1wASkCiXkxBR-Ty1xNGslnanOXTflAUF5vqO1_-AxZYO6RLjZI82-pDOxONMndJJ2OMIsNoc7R/s368/sql-inner-join-cross-join-return-same-count-how.jpg)

  

To join both these tables, we are using ColumnA in TableA and ColumnB in TableB. The following is the SQL Server interview question.

No matter how you join these 2 tables, the query produces the same result i.e 6 rows - How and Why?

  

1. Inner Join
2. Left Outer Join
3. Right Outer Join
4. Full Outer Join OR
5. even Cross Join

  
### SQL Script to create and populate the tables with test data

Create Table TableA

(

       ColumnA int

)

Go

Create Table TableB

(

       ColumnB int

)

Go

Insert into TableA Values (1)

Insert into TableA Values (1)

Go 

Insert into TableB Values (1)

Insert into TableB Values (1)

Insert into TableB Values (1)

Go

Select ColumnA, ColumnB

from TableA

inner join TableB

on TableA.ColumnA \= TableB.ColumnB

Select ColumnA, ColumnB

from TableA

left outer join TableB

on TableA.ColumnA \= TableB.ColumnB

Select ColumnA, ColumnB

from TableA

right outer join TableB

on TableA.ColumnA \= TableB.ColumnB

Select ColumnA, ColumnB

from TableA

full outer join TableB

on TableA.ColumnA \= TableB.ColumnB 

Select ColumnA, ColumnB

from TableA

cross join TableB

All the above queries return the same row count - 6 rows. How and why all the different types of joins return the same count of rows.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4zG-aGTBC-RhRsTvFEfdcRRmos7XlugDO_vSrN_fztxw9c3A_JTIeBrB8-Uh3CTTlVN_H_aDp789fgHD8Xai_-g5888o5gJMs6Wvu3y2htNAb-vdrHNb_li6OiYl2VCaZM_m09-se5XYS/s16000/sql-inner-left-right-join-same-results.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4zG-aGTBC-RhRsTvFEfdcRRmos7XlugDO_vSrN_fztxw9c3A_JTIeBrB8-Uh3CTTlVN_H_aDp789fgHD8Xai_-g5888o5gJMs6Wvu3y2htNAb-vdrHNb_li6OiYl2VCaZM_m09-se5XYS/s337/sql-inner-left-right-join-same-results.png)

  

Every row in TableA matches with every row in TableB, so what we get back is a cartesian product i.e the number of rows in TableA multiplied by the number of rows in TableB. So, in essence it's like a cross join. 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjMXJboZd_ODnZO60GfNg-zDtiGdyRjOKVLhGrO2l0wdpurODUq5KiQhYFsdvGuuermAluVlPE6GE8lmJvTCp9w0E3g-ZkVbnfKs7NNMwafG3ik5DxbWtqefxWLFS7EvVx8eVg4Dk_x_JIk/s16000/sql-inner-left-right-join-result-same-how.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjMXJboZd_ODnZO60GfNg-zDtiGdyRjOKVLhGrO2l0wdpurODUq5KiQhYFsdvGuuermAluVlPE6GE8lmJvTCp9w0E3g-ZkVbnfKs7NNMwafG3ik5DxbWtqefxWLFS7EvVx8eVg4Dk_x_JIk/s364/sql-inner-left-right-join-result-same-how.jpg)

  

TableA has 2 rows and TableB 3 rows. Every row in TableA matches with every row in TableB. So irrespective of the type of join we get the cartesian product 6, i.e 2 rows in TableA multiplied by 3 rows in TableB.

What do you think is the result going to be if we add one more row with a value of 1 to TableB.

Well, the same logic, Cartesian product. 2 rows in TableA multiplied by 4 rows in TableB. So, the answer is 8.

Insert into TableA Values (1)

Go

  

Execute all the 5 select queries again and you will get 8 rows as the result.

Are you still confused? Let's look at another example.

Drop both the tables

Drop table TableA

Drop table TableB

  

Recreate the tables. We now have a second column called SomeValue in both the tables.

Create Table TableA

(

       ColumnA int,

       SomeValue nvarchar(2)

)

Go

Create Table TableB

(

       ColumnB int,

       SomeValue nvarchar(2)

)

Go

\--Insert test data.

Insert into TableA Values (1, 'A1')

Insert into TableA Values (1, 'A2')

Go 

Insert into TableB Values (1, 'B1')

Insert into TableB Values (1, 'B2')

Insert into TableB Values (1, 'B3')

Go

  

Now, the select queries. In addition to ColumnA and ColumnB we also want to select SomeValue From TableA. Let's give it an alias TableASomeValue. Similarly SomeValue column from TableB as well. Let's call it TableBSomeValue.

Let's include the same select list on the rest of the 4 queries - that is left join, right join, full join, and cross join.

Select ColumnA, ColumnB, TableA.SomeValue as \[TableASomeValue\],

TableB.SomeValue as \[TableBSomeValue\]

from TableA inner join

TableB on TableA.ColumnA \= TableB.ColumnB 

Select ColumnA, ColumnB, TableA.SomeValue as \[TableASomeValue\],

TableB.SomeValue as \[TableBSomeValue\]

from TableA left outer join

TableB on TableA.ColumnA \= TableB.ColumnB 

Select ColumnA, ColumnB, TableA.SomeValue as \[TableASomeValue\],

TableB.SomeValue as \[TableBSomeValue\]

from TableA right outer join

TableB on TableA.ColumnA \= TableB.ColumnB 

Select ColumnA, ColumnB, TableA.SomeValue as \[TableASomeValue\],

TableB.SomeValue as \[TableBSomeValue\]

from TableA full outer join

TableB on TableA.ColumnA \= TableB.ColumnB

Select ColumnA, ColumnB, TableA.SomeValue as \[TableASomeValue\],

TableB.SomeValue as \[TableBSomeValue\]

from TableA cross join TableB

  

Execute all the queries. Notice the output. 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBANWaED2fu36wy2EsPxyN8Vu97cfDpNoq0VShpldnRjMI6YyuOXUzRSEYgqemqFzn9MjDcg9mGx2IZx7M8V0B60N1rRTvMxhC5YhU0ovrFqmbHjam1GhHQYHmqTZ8Hgf5Tq0iG8v6qqfS/w320-h103/all-sql-join-types-produce-same-result.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBANWaED2fu36wy2EsPxyN8Vu97cfDpNoq0VShpldnRjMI6YyuOXUzRSEYgqemqFzn9MjDcg9mGx2IZx7M8V0B60N1rRTvMxhC5YhU0ovrFqmbHjam1GhHQYHmqTZ8Hgf5Tq0iG8v6qqfS/s895/all-sql-join-types-produce-same-result.jpg)