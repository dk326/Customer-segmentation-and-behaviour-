# Customer-segmentation-and-behaviour-

**About the project**

This project aimed to understand the behavior of various customers of an online news provider. The analysis focused on identifying behavioral differences between gold and non-gold members. Additionally, new customer segmentations, not originally present in the data, were created using SQL queries.

Analysis mainly focused on areas such as comparing gold and non gold members in areas such as communication preferences and spending patterns. Further, preferences in communication channels and technology used among customer segments such as age, tenure, gender and state are also compared.

Data cleaning was conducted and missing values were either removed or imputed. Since the gold membership is a subscription, the dataset consists of missing values for fullfilment end date for active memberships. Therefore, the missing values for these were handled accordingly.

**Behavioual Differences between gold vs non gold members**

**A CTE was used to join customer data and the contract data with the primary key of cost id to calculate metrics like purchase frequency, total spending, average spending, and average inbound/outbound communications over 24 and 60 months, as well as total arrears in the last 2 and 5 years for each customer.*

**Then using two CTEs gold member and non gold member data is queried. The final query uses an union all statement to combine gold and non gold segments and aggreates certain metrics for both groups for comparison.*

<img width="547" alt="Screenshot 2024-10-12 at 12 04 10 PM" src="https://github.com/user-attachments/assets/f222932c-2978-452d-9d7b-2e88b0e97d02">
<br>

<img width="413" alt="Screenshot 2024-10-12 at 12 04 41 PM" src="https://github.com/user-attachments/assets/a68aba68-f149-4539-ad1f-d852e65fbdf9">

**Result**

<img width="1506" alt="Screenshot 2024-10-12 at 12 15 51 PM" src="https://github.com/user-attachments/assets/9620fb55-3574-46c5-8421-3548f498dec7">


**Creating customer segments by Age, Tenure, Gender and State to understand differences in communication frequency and technology** 

**In the following queries, CTE is first used to create the customer segment from the data. Then The final query will aggregate the results from that CTE for different other metrics.*

**Creating customer segments by Age** 

<img width="588" alt="Screenshot 2024-10-12 at 12 08 19 PM" src="https://github.com/user-attachments/assets/cdc478d4-24f4-4a65-a90d-68a265b0b0a5">

**Result**

<img width="1566" alt="Screenshot 2024-10-12 at 12 16 29 PM" src="https://github.com/user-attachments/assets/2d1ff826-40af-40a0-889c-5f233a4938ac">


**Creating customer segments by Customer Tenure** 

<img width="546" alt="Screenshot 2024-10-12 at 12 06 54 PM" src="https://github.com/user-attachments/assets/7db50d42-364d-4817-994e-bb50129c42d5">

**Result**

<img width="1561" alt="Screenshot 2024-10-12 at 12 16 58 PM" src="https://github.com/user-attachments/assets/f16fd816-d8cf-4432-8e0c-026d120f843d">


**Creating customer segments by Customer gender** 

<img width="576" alt="Screenshot 2024-10-12 at 12 07 20 PM" src="https://github.com/user-attachments/assets/7f312c8f-3143-4a6b-a996-a985c3735f45">

**Result**

<img width="1563" alt="Screenshot 2024-10-12 at 12 17 23 PM" src="https://github.com/user-attachments/assets/270165d0-22ec-4cfa-a388-85f684d33491">

**Customer segmentation by states**

<img width="570" alt="Screenshot 2024-10-12 at 12 07 34 PM" src="https://github.com/user-attachments/assets/8916384b-7452-4d18-9887-742ef0728bc6">

**Result**

<img width="1575" alt="Screenshot 2024-10-12 at 12 17 44 PM" src="https://github.com/user-attachments/assets/e80b3242-8ec5-4dd4-ac62-91162972a368">

**Data Cleaning**

<img width="553" alt="Screenshot 2024-10-12 at 12 00 37 PM" src="https://github.com/user-attachments/assets/725ea43b-32bb-4f86-b713-35f2ee05ce29">

<br>

<img width="543" alt="Screenshot 2024-10-12 at 12 00 59 PM" src="https://github.com/user-attachments/assets/975e8313-57cf-4c26-ac55-03eafea613f3">


