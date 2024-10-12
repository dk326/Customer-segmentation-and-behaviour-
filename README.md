# Customer-segmentation-and-behaviour-

**About the project**

This project aimed to understand the behavior of various customers of an online news provider. The analysis focused on identifying behavioral differences between gold and non-gold members. Additionally, new customer segmentations, not originally present in the data, were created using SQL queries.

Data cleaning was conducted and missing values were either removed or imputed. 

Analysis mainly focused on areas such as comparing gold and non gold members in areas such as communication preferences and spending patterns. Further, preferences in communication channels and technology used among customer segments such as age, tenure, gender and state are also compared.

**Data Cleaning**

#**Inspect and cleaning data from customer table**
SELECT * FROM `assignment-2-433807.customer.customer`;

#Null value count by each column 
##Gender column has 7214 null values, affluene level has 888 null values, state has 283 null values. 
 SELECT 
  SUM(CASE WHEN CostID IS NULL THEN 1 ELSE 0 END) AS Null_CostID,
  SUM(CASE WHEN Affluence_Level IS NULL THEN 1 ELSE 0 END) AS Null_Affluence_Level,
  SUM(CASE WHEN Tenure IS NULL THEN 1 ELSE 0 END) AS Null_Tenure,
  SUM(CASE WHEN Age IS NULL THEN 1 ELSE 0 END) AS Null_Age,
  SUM(CASE WHEN Gender IS NULL THEN 1 ELSE 0 END) AS Null_Gender,
  SUM(CASE WHEN State IS NULL THEN 1 ELSE 0 END) AS Null_State,
  SUM(CASE WHEN HasValidEmailAddress IS NULL THEN 1 ELSE 0 END) AS Null_HasValidEmailAddress,
  SUM(CASE WHEN HasValidMobileNumber IS NULL THEN 1 ELSE 0 END) AS Null_HasValidMobileNumber,
  SUM(CASE WHEN Number_Of_Inbound_Communication_In_24_MTH IS NULL THEN 1 ELSE 0 END) AS Null_Number_Of_Inbound_Communication_In_24_MTH,
  SUM(CASE WHEN Number_Of_Inbound_Communication_In_60_MTH IS NULL THEN 1 ELSE 0 END) AS Null_Number_Of_Inbound_Communication_In_60_MTH,
  SUM(CASE WHEN Number_Of_Outbound_Communication_In_24_MTH IS NULL THEN 1 ELSE 0 END) AS Null_Number_Of_Outbound_Communication_In_24_MTH,
  SUM(CASE WHEN Number_Of_Outbound_Communication_In_60_MTH IS NULL THEN 1 ELSE 0 END) AS Null_Number_Of_Outbound_Communication_In_60_MTH,
  SUM(CASE WHEN Pathway_Arrears_In_24_MTH IS NULL THEN 1 ELSE 0 END) AS Null_Pathway_Arrears_In_24_MTH,
  SUM(CASE WHEN Pathway_Arrears_In_60_MTH IS NULL THEN 1 ELSE 0 END) AS Null_Pathway_Arrears_In_60_MTH,
  SUM(CASE WHEN Inter_Mailing_Preferances_Update IS NULL THEN 1 ELSE 0 END) AS Null_Inter_Mailing_Preferances_Update,
  SUM(CASE WHEN Inter_Personal_Details_Update IS NULL THEN 1 ELSE 0 END) AS Null_Inter_Personal_Details_Update,
  SUM(CASE WHEN Operating_System IS NULL THEN 1 ELSE 0 END) AS Null_Operating_System,
  SUM(CASE WHEN Browser_type IS NULL THEN 1 ELSE 0 END) AS Null_Browser_type
FROM `assignment-2-433807.customer.customer`;  

#Delete Nulls from State 283.
DELETE
FROM `assignment-2-433807.customer.customer`
WHERE State IS NULL;

#Impute missing values in Affluence level as unknown
UPDATE `assignment-2-433807.customer.customer`
SET Affluence_Level = 'Unknown'
WHERE Affluence_Level IS NULL;

#Impute missing values in Gender as unknown
UPDATE `assignment-2-433807.customer.customer`
SET Gender = 'Unknown'
WHERE Gender IS NULL;

##**Inspect and cleaning data from contract table**
SELECT * FROM `assignment-2-433807.Contract.contact`;

#Null value count by each column 
##Null_contract_ID, Null_IsInitialCommitment, Null_Product,Null_TotalPaid,Null_FulfilmentDate have 4409 missing values and  Null_FulfilmentEndedDate has 12869 null values.
 SELECT 
  SUM(CASE WHEN CostID IS NULL THEN 1 ELSE 0 END) AS Null_CostID,
  SUM(CASE WHEN contract_ID IS NULL THEN 1 ELSE 0 END) AS Null_contract_ID,
  SUM(CASE WHEN IsInitialCommitment IS NULL THEN 1 ELSE 0 END) AS Null_IsInitialCommitment,
  SUM(CASE WHEN Product IS NULL THEN 1 ELSE 0 END) AS Null_Product,
  SUM(CASE WHEN TotalPaid IS NULL THEN 1 ELSE 0 END) AS Null_TotalPaid,
  SUM(CASE WHEN FulfilmentDate IS NULL THEN 1 ELSE 0 END) AS Null_FulfilmentDate,
  SUM(CASE WHEN FulfilmentEndedDate IS NULL THEN 1 ELSE 0 END) AS Null_FulfilmentEndedDate,
FROM `assignment-2-433807.Contract.contact`;  

#Delete Nulls
DELETE
FROM `assignment-2-433807.Contract.contact`
WHERE contract_ID IS NULL OR IsInitialCommitment IS NULL OR Product IS NULL OR TotalPaid IS NULL OR FulfilmentDate IS NULL;

#checking active gold memberships 
#there are 6045 null values for gold memberships meaning that there are 6045 active gold memberships
SELECT COUNT (*)
FROM `assignment-2-433807.Contract.contact`
WHERE Product = 'Gold_Membership' 
AND FulfilmentEndedDate IS NULL;

#Removing null values for Fullfilment end date except for active gold memberships
DELETE FROM `assignment-2-433807.Contract.contact`
WHERE Product <> 'Gold_Membership'
AND FulfilmentEndedDate IS NULL;

#Updating Null values of Gold to current date to show active status
UPDATE `assignment-2-433807.Contract.contact`
SET FulfilmentEndedDate = CURRENT_DATE()
WHERE FulfilmentEndedDate IS NULL;

**Behavioual Differences between gold vs non gold members**

**A CTE was used to join customer data and the contract data with the primary key of cost id to calculate metrics like purchase frequency, total spending, average spending, and average inbound/outbound communications over 24 and 60 months, as well as total arrears in the last 2 and 5 years for each customer.*

**Then using two CTEs gold member and non gold member data is queried. The final query uses an union all statement to combine gold and non gold segments and aggreates certain metrics for both groups for comparison.*


#Q1 Gold members vs non gold members

WITH CustomerData AS(
   SELECT
        a.CostID,
        b.Product,
        COUNT(DISTINCT b.FulfilmentEndedDate) AS PurchaseFrequency,
        SUM(b.TotalPaid) AS TotalSpending,
        AVG(b.TotalPaid) AS AverageSpending,
        AVG(a.Number_Of_Inbound_Communication_In_24_MTH) AS AverageInboundComm24,
        AVG(a.Number_Of_Outbound_Communication_In_24_MTH) AS AverageOutboundComm24,
        AVG(a.Number_Of_Inbound_Communication_In_60_MTH) AS AverageInboundComm60,
        AVG(a.Number_Of_Outbound_Communication_In_60_MTH) AS AverageOutboundComm60,
        SUM(a.Pathway_Arrears_In_24_MTH) AS TotalArrears2yrs,
        SUM(a.Pathway_Arrears_In_60_MTH) AS TotalArrears5yrs
    FROM
        `assignment-2-433807.customer.customer` a
    LEFT JOIN `assignment-2-433807.Contract.contact` b ON a.CostID = b.CostID
    GROUP BY
        a.CostID, b.Product
),
GoldMembers AS (
    SELECT
        CostID,
        PurchaseFrequency,
        TotalSpending,
        AverageSpending,
        AverageInboundComm24,
        AverageOutboundComm24,
        AverageInboundComm60,
        AverageOutboundComm60,
        TotalArrears2yrs,
        TotalArrears5yrs,
    FROM
        CustomerData
    WHERE
        Product = 'Gold_Membership'
),
NonGoldMembers AS (
    SELECT
        CostID,
        PurchaseFrequency,
        TotalSpending,
        AverageSpending,
        AverageInboundComm24,
        AverageOutboundComm24,
        AverageInboundComm60,
        AverageOutboundComm60,
        TotalArrears2yrs,
        TotalArrears5yrs,
    FROM
        CustomerData
    WHERE
        Product != 'Gold_Membership'
)
SELECT
    'Gold Members' AS CustomerGroup,
    COUNT(DISTINCT(CostID)) AS CountOfMemebers,
    AVG(PurchaseFrequency) AS ThePurchaseFrequency,
    SUM(TotalSpending) AS TheTotalSpending,
    AVG(AverageSpending) AS TheAverageSpending,
    AVG(AverageInboundComm24) AS TheAverageInboundComm24,
    AVG(AverageOutboundComm24) AS TheAverageOutboundComm24,
    AVG(AverageInboundComm60) AS TheAverageInboundComm60,
    AVG(AverageOutboundComm60) AS TheAverageOutboundComm60,
    SUM(TotalArrears2yrs) AS TotalArrears2yrs,
    SUM(TotalArrears5yrs) AS TotalArrears5yrs,
FROM
    GoldMembers
UNION ALL
SELECT
    'Non-Gold Members' AS CustomerGroup,
    COUNT(DISTINCT(CostID)) AS CountOfMemebers,
    AVG(PurchaseFrequency) AS ThePurchaseFrequency,
    SUM(TotalSpending) AS TheTotalSpending,
    AVG(AverageSpending) AS TheAverageSpending,
    AVG(AverageInboundComm24) AS TheAverageInboundComm24,
    AVG(AverageOutboundComm24) AS TheAverageOutboundComm24,
    AVG(AverageInboundComm60) AS TheAverageInboundComm60,
    AVG(AverageOutboundComm60) AS TheAverageOutboundComm60,
    SUM(TotalArrears2yrs) AS TotalArrears2yrs,
    SUM(TotalArrears5yrs) AS TotalArrears5yrs,
FROM
    NonGoldMembers;



**Creating customer segments by Age, Tenure, Gender and State to understand differences in communication frequency and technology** 

**In the following queries, CTE is first used to create the customer segment from the data. Then The final query will aggregate the results from that CTE for different other metrics.*

**Creating customer segments by Age** 

#Q2 
#How customers in different age categories differ in terms of communication frequency and technology
WITH CustomerAge AS(
  SELECT
    CostID,
    CASE
        WHEN AGE BETWEEN 7 AND 14 THEN 'Children'
        WHEN AGE BETWEEN 15 AND 24 THEN 'Young Adults'
        WHEN AGE BETWEEN 25 AND 44 THEN 'Adults'
        WHEN AGE BETWEEN 45 AND 64 THEN 'Established Adults'
        WHEN AGE BETWEEN 65 AND 100 THEN 'Seniors'
    ELSE 'Unknown'
    END AS Age_Group,
    Number_Of_Inbound_Communication_In_24_MTH,
    Number_Of_Outbound_Communication_In_24_MTH,
    Number_Of_Inbound_Communication_In_60_MTH,
    Number_Of_Outbound_Communication_In_60_MTH,
    Operating_System,
    Browser_type,
    HasValidEmailAddress,
    HasValidMobileNumber,
    Inter_Personal_Details_Update,
  FROM `assignment-2-433807.customer.customer`
)
SELECT
    Age_Group,
    COUNT(*) AS Member_Count,
    AVG(Number_Of_Inbound_Communication_In_24_MTH) AS Avg_Inbound_Comm,
    AVG(Number_Of_Outbound_Communication_In_24_MTH) AS Avg_Outbound_Comm,
    SUM(CASE WHEN Operating_System = 1 THEN 1 ELSE 0 END) AS Windows_Users,
    SUM(CASE WHEN Operating_System = 2 THEN 1 ELSE 0 END) AS macOS_Users,
    SUM(CASE WHEN Operating_System = 3 THEN 1 ELSE 0 END) AS Linux_Users,
    SUM(CASE WHEN Operating_System = 4 THEN 1 ELSE 0 END) AS Android_Users,
    SUM(CASE WHEN Browser_type = 1 THEN 1 ELSE 0 END) AS Chrome_Users,
    SUM(CASE WHEN Browser_type = 2 THEN 1 ELSE 0 END) AS Firefox_Users,
    SUM(CASE WHEN Browser_type = 3 THEN 1 ELSE 0 END) AS Safari_Users,
    SUM(CASE WHEN Browser_type = 4 THEN 1 ELSE 0 END) AS Edge_Users,
    SUM(CASE WHEN Browser_type = 5 THEN 1 ELSE 0 END) AS Opera_Users,
    SUM(CASE WHEN HasValidEmailAddress = true THEN 1 ELSE 0 END) AS Valid_Email,
    SUM(CASE WHEN HasValidMobileNumber = true THEN 1 ELSE 0 END) AS Valid_Mobile,
FROM
    CustomerAge
GROUP BY
    Age_Group;

**Creating customer segments by Customer Tenure** 

#How customers in different Tenure categories differ in terms of communication frequency and technology
WITH CustomerTenure AS(
  SELECT 
    CostID,
    CASE
      WHEN Tenure BETWEEN 0 AND 5 THEN 'New to Early Stage'
      WHEN Tenure BETWEEN 6 AND 10 THEN 'Mid Stage'
      WHEN Tenure BETWEEN 11 AND 15 THEN 'Established'
      WHEN Tenure BETWEEN 16 AND 25 THEN 'Long-Term'
      WHEN Tenure BETWEEN 26 AND 44 THEN 'Loyal'
      ELSE 'Unknown'
    END AS Tenure_Group,
    Number_Of_Inbound_Communication_In_24_MTH,
    Number_Of_Outbound_Communication_In_24_MTH,
    Number_Of_Inbound_Communication_In_60_MTH,
    Number_Of_Outbound_Communication_In_60_MTH,
    Operating_System,
    Browser_type,
    HasValidEmailAddress,
    HasValidMobileNumber,

  FROM `assignment-2-433807.customer.customer`
)
SELECT
    Tenure_Group,
    COUNT(*) AS Member_Count,
    AVG(Number_Of_Inbound_Communication_In_24_MTH) AS Avg_Inbound_Comm,
    AVG(Number_Of_Outbound_Communication_In_24_MTH) AS Avg_Outbound_Comm,
    SUM(CASE WHEN Operating_System = 1 THEN 1 ELSE 0 END) AS Windows_Users,
    SUM(CASE WHEN Operating_System = 2 THEN 1 ELSE 0 END) AS macOS_Users,
    SUM(CASE WHEN Operating_System = 3 THEN 1 ELSE 0 END) AS Linux_Users,
    SUM(CASE WHEN Operating_System = 4 THEN 1 ELSE 0 END) AS Android_Users,
    SUM(CASE WHEN Browser_type = 1 THEN 1 ELSE 0 END) AS Chrome_Users,
    SUM(CASE WHEN Browser_type = 2 THEN 1 ELSE 0 END) AS Firefox_Users,
    SUM(CASE WHEN Browser_type = 3 THEN 1 ELSE 0 END) AS Safari_Users,
    SUM(CASE WHEN Browser_type = 4 THEN 1 ELSE 0 END) AS Edge_Users,
    SUM(CASE WHEN Browser_type = 5 THEN 1 ELSE 0 END) AS Opera_Users,
    SUM(CASE WHEN HasValidEmailAddress = true THEN 1 ELSE 0 END) AS Valid_Email,
    SUM(CASE WHEN HasValidMobileNumber = true THEN 1 ELSE 0 END) AS Valid_Mobile,
FROM
    CustomerTenure
GROUP BY
    Tenure_Group;

**Creating customer segments by Customer gender** 

#How customers in different Genders differ in terms of communication frequency and technology

WITH CustomerGender AS(
  SELECT 
    CostID,
    Gender,
    Number_Of_Inbound_Communication_In_24_MTH,
    Number_Of_Outbound_Communication_In_24_MTH,
    Number_Of_Inbound_Communication_In_60_MTH,
    Number_Of_Outbound_Communication_In_60_MTH,
    Operating_System,
    Browser_type,
    HasValidEmailAddress,
    HasValidMobileNumber,
    Inter_Personal_Details_Update,
  FROM `assignment-2-433807.customer.customer`
)
SELECT
    Gender,
    COUNT(*) AS Member_Count,
    AVG(Number_Of_Inbound_Communication_In_24_MTH) AS Avg_Inbound_Comm,
    AVG(Number_Of_Outbound_Communication_In_24_MTH) AS Avg_Outbound_Comm,
    SUM(CASE WHEN Operating_System = 1 THEN 1 ELSE 0 END) AS Windows_Users,
    SUM(CASE WHEN Operating_System = 2 THEN 1 ELSE 0 END) AS macOS_Users,
    SUM(CASE WHEN Operating_System = 3 THEN 1 ELSE 0 END) AS Linux_Users,
    SUM(CASE WHEN Operating_System = 4 THEN 1 ELSE 0 END) AS Android_Users,
    SUM(CASE WHEN Browser_type = 1 THEN 1 ELSE 0 END) AS Chrome_Users,
    SUM(CASE WHEN Browser_type = 2 THEN 1 ELSE 0 END) AS Firefox_Users,
    SUM(CASE WHEN Browser_type = 3 THEN 1 ELSE 0 END) AS Safari_Users,
    SUM(CASE WHEN Browser_type = 4 THEN 1 ELSE 0 END) AS Edge_Users,
    SUM(CASE WHEN Browser_type = 5 THEN 1 ELSE 0 END) AS Opera_Users,
    SUM(CASE WHEN HasValidEmailAddress = true THEN 1 ELSE 0 END) AS Valid_Email,
    SUM(CASE WHEN HasValidMobileNumber = true THEN 1 ELSE 0 END) AS Valid_Mobile,
FROM
    CustomerGender
GROUP BY
    Gender;

**Customer segmentation by states**

#How customers in different States differ in terms of communication frequency and technology
WITH CustomerState AS(
  SELECT 
    CostID,
    State,
    Number_Of_Inbound_Communication_In_24_MTH,
    Number_Of_Outbound_Communication_In_24_MTH,
    Number_Of_Inbound_Communication_In_60_MTH,
    Number_Of_Outbound_Communication_In_60_MTH,
    Operating_System,
    Browser_type,
    HasValidEmailAddress,
    HasValidMobileNumber,
    Inter_Personal_Details_Update,
  FROM `assignment-2-433807.customer.customer`
)
SELECT
    State,
    COUNT(*) AS Member_Count,
    AVG(Number_Of_Inbound_Communication_In_24_MTH) AS Avg_Inbound_Comm,
    AVG(Number_Of_Outbound_Communication_In_24_MTH) AS Avg_Outbound_Comm,
    SUM(CASE WHEN Operating_System = 1 THEN 1 ELSE 0 END) AS Windows_Users,
    SUM(CASE WHEN Operating_System = 2 THEN 1 ELSE 0 END) AS macOS_Users,
    SUM(CASE WHEN Operating_System = 3 THEN 1 ELSE 0 END) AS Linux_Users,
    SUM(CASE WHEN Operating_System = 4 THEN 1 ELSE 0 END) AS Android_Users,
    SUM(CASE WHEN Browser_type = 1 THEN 1 ELSE 0 END) AS Chrome_Users,
    SUM(CASE WHEN Browser_type = 2 THEN 1 ELSE 0 END) AS Firefox_Users,
    SUM(CASE WHEN Browser_type = 3 THEN 1 ELSE 0 END) AS Safari_Users,
    SUM(CASE WHEN Browser_type = 4 THEN 1 ELSE 0 END) AS Edge_Users,
    SUM(CASE WHEN Browser_type = 5 THEN 1 ELSE 0 END) AS Opera_Users,
    SUM(CASE WHEN HasValidEmailAddress = true THEN 1 ELSE 0 END) AS Valid_Email,
    SUM(CASE WHEN HasValidMobileNumber = true THEN 1 ELSE 0 END) AS Valid_Mobile,
FROM
    CustomerState
GROUP BY
    State;


