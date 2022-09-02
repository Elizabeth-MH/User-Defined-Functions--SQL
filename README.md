# User-Defined-Functions--SQL
User Defined Functions -SQL

-- Use the HumanResources.Employee Table in the Adventureworks Database

--1. Create a UDF that accepts EmployeeID (2012: BusinessEntityID) and returns UserLoginID. The UserLoginID is the last part of the LogID column. It’s only the part that comes after the \

--Hardcode Component Test

SELECT   SUBSTRING(LoginID,CHARINDEX('\',LoginID,0) + 1, LEN(LoginID)) AS UserLoginID
FROM	 HumanResources.Employee
WHERE    BusinessEntityID=1 --@BusinessEntity
--Creating the UDF
CREATE FUNCTION fx_getLoginID

			   (@businessEntityID INT)


RETURNS  VARCHAR(25)

AS

BEGIN

   DECLARE @LoginID VARCHAR(25) =
                                (
								SELECT   SUBSTRING(LoginID,CHARINDEX('\',LoginID,0) + 1, LEN(LoginID)) AS UserLoginID
                                FROM	 HumanResources.Employee 
								WHERE    BusinessEntityID = @businessEntityID
								)

   RETURN @LoginID

END
--2.Create a UDF that accepts EmployeeID (2012: BusinessEntityID) and returns their age.

--Hardcode component test

SELECT   DATEDIFF(year, BirthDate,GETDATE())as Age
FROM     HumanResources.Employee
WHERE    BusinessEntityID =2 --@BusinessEntityID
--Creating UDF
CREATE FUNCTION fx_getAge

				(@businessEntityID INT)

RETURNS INT

AS

BEGIN

   DECLARE @Age INT =
                    ( 
					SELECT   DATEDIFF(year, BirthDate,GETDATE())
	                FROM	 [HumanResources].[Employee] 
					WHERE	 BusinessEntityID = @businessEntityID
			         )
   RETURN @Age

END
---3. Create a UDF that accepts the Gender and returns the avg VacationHours

--Hardcode Component Test

SELECT   AVG (VacationHours)
FROM     HumanResources.Employee
WHERE    Gender ='M'-- @ Gender
--Creating UDF
CREATE FUNCTION fx_getAverageVacationHoursByGender

				(@Gender NCHAR (1))

RETURNS INT

AS

BEGIN

   DECLARE @AvgVacationHours INT =
							  ( 
							  SELECT    AVG (VacationHours)
							  FROM		HumanResources.Employee
							  WHERE     Gender = @Gender
							  )
   RETURN @AvgVacationHours

END
--4.Create a UDF that accepts ManagerID (2012: JobTitle) and returns all of that Managers (2012: JobTitle) Employee Information.
--a.LoginID b.Gender c. Hiredate

---HardCode Component test
SELECT LoginID,Gender, HireDate
FROM HumanResources.Employee
WHERE JobTitle ='Senior Tool Designer' ---@jobtitle

--Creating the UDF Table

CREATE FUNCTION  fx_getLoginIDGenderandHireDate

				(@JobTitle VARCHAR(50))

RETURNS TABLE

AS

	  RETURN                         
			  ( 
			   SELECT LoginID,Gender,HireDate
			   FROM   HumanResources.Employee
			   WHERE  JobTitle = @JobTitle
				)

END
