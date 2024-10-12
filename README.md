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

**Insights**

- Customer Overview: 7,049 gold members (6,045 active) versus 16,487 non-gold members.
- Purchase Frequency: Gold members purchase 1.09 times on average; non-gold members purchase 3.8 times.
- Total Spending: Gold members spent $559,182; non-gold members spent $7,745,525.03.
- Average Spending: Gold members average $67; non-gold members average $58, but non-gold members generate more revenue overall.
- Payment Rejections (Last 2 Years): Gold members had 3,035 rejections (1 per 2 members); non-gold members had 28,460 rejections (1.7 per member).
- Payment Rejections (Last 5 Years): Gold members had 5,691 rejections; non-members had 62,007.
- Communications (Last 2 Years): Gold members had 5.7 inbound messages; non-gold members had 9.8. NewsStream sent 45 messages to gold members and 60 to non-gold members.
- Conclusion: Non-gold members purchase more frequently and generate higher total revenue, while gold members have higher average spending and fewer payment issues, indicating stability.

**Creating customer segments by Age, Tenure, Gender and State to understand differences in communication frequency and technology** 

**In the following queries, CTE is first used to create the customer segment from the data. Then The final query will aggregate the results from that CTE for different other metrics.*

**Creating customer segments by Age** 

<img width="588" alt="Screenshot 2024-10-12 at 12 08 19 PM" src="https://github.com/user-attachments/assets/cdc478d4-24f4-4a65-a90d-68a265b0b0a5">

**Result**

<img width="1566" alt="Screenshot 2024-10-12 at 12 16 29 PM" src="https://github.com/user-attachments/assets/2d1ff826-40af-40a0-889c-5f233a4938ac">

**Insights**

Customer Segments:
- Children: Ages 7-14
- Young Adults: Ages 15-24
- Adults: Ages 25-44
- Established Adults: Ages 45-64
- Seniors: Ages 65-100
- Unknown: Age not available or outside the 7-100 range.

Summary of Findings

Established Adults: 
- Largest segment, comprising 41% (10,310 members).
- Second highest average inbound communications (8.2) over the last 2 years.

Adults:
- Second largest segment at 22% (5,531 members).
- Third highest average inbound communications.

Seniors:
- Third largest segment at 18% (4,465 members).
- Highest average inbound communications (8.6).

Children:
- Smallest segment at 0.009% (246 members).
- High average inbound communications (7.5).

Young Adults:
- Second smallest segment at 0.04% (1,011 members).
- Lowest average inbound communications (5.1).

Contact Information Validity:
- Most segments have over 80% valid emails and mobile numbers.
- Seniors: 74% with valid email; 61% with valid mobile numbers.

Technology Preferences:
- Majority (27%) use Linux; least popular is Windows (22%).
- Most popular browser is Safari (23%), followed by Edge (21%); least popular is Opera (17%).
- 13.5% of the sample have no age group.

**Creating customer segments by Customer Tenure** 

<img width="546" alt="Screenshot 2024-10-12 at 12 06 54 PM" src="https://github.com/user-attachments/assets/7db50d42-364d-4817-994e-bb50129c42d5">

**Result**

<img width="1561" alt="Screenshot 2024-10-12 at 12 16 58 PM" src="https://github.com/user-attachments/assets/f16fd816-d8cf-4432-8e0c-026d120f843d">

**Insights**

- New to Early Stage: 0 – 5 years
- Mid Stage: 6 – 10 years
- Established: 11 – 15 years
- Long Term: 16 – 25 years
- Loyal: 16 – 44 years

Key Insights

- Operating Systems: No significant differences in operating system preferences across segments; Safari is the most popular browser.
- Communication Trends: Average inbound communications increase with customer tenure, with loyal customers averaging 9.1 communications, indicating higher engagement.
- Contact Information Updates: All segments, except for loyal and long-term customers, have over 80% updated emails and mobile numbers. Notably, 25% of long-term and 40% of loyal customers lack updated mobile numbers, potentially leading to service issues. It’s recommended that NewsStream takes action to update contact details for these groups.

**Creating customer segments by Customer gender** 

<img width="576" alt="Screenshot 2024-10-12 at 12 07 20 PM" src="https://github.com/user-attachments/assets/7f312c8f-3143-4a6b-a996-a985c3735f45">

**Result**

<img width="1563" alt="Screenshot 2024-10-12 at 12 17 23 PM" src="https://github.com/user-attachments/assets/270165d0-22ec-4cfa-a388-85f684d33491">

**Insights**

- Customer Distribution: NewsStream has 6,860 male customers and 10,816 female customers, with 738 missing gender values categorized as unknown.
- Operating Systems: No significant differences in operating system preferences; Android is the most popular among both genders, while Safari is the leading web browser.
- Communication Patterns: Both male and female customers average 8 inbound communications.
- Contact Information: Over 80% of customers from both genders have valid mobile and email addresses.
- Overall Insights: Aside from the higher number of female customers, there are limited insights derived from this gender segmentation.
- Limitations: There were 738 missing values for gender, they have been categorised as unknown which might hinder the accuracy of this analysis. 

**Customer segmentation by states**

<img width="570" alt="Screenshot 2024-10-12 at 12 07 34 PM" src="https://github.com/user-attachments/assets/8916384b-7452-4d18-9887-742ef0728bc6">

**Result**

<img width="1575" alt="Screenshot 2024-10-12 at 12 17 44 PM" src="https://github.com/user-attachments/assets/e80b3242-8ec5-4dd4-ac62-91162972a368">

**Insights**

Customer Distribution:
- NSW: 33%
- VIC: 27%
- QLD: 14.9%
- WA: 10%
- SA: 0.7%
- ACT: 0.02%
- TAS: 0.02%
- International: 0.008%

- Operating Systems: No significant differences in operating system preferences across states; Safari remains the most popular web browser.
- Contact Information: Tasmania has the lowest valid mobile number rate at 72%, while ACT and international customers also have low rates at 74%.
- Communication Patterns: International customers report the highest average inbound communications at 8.7, indicating potential issues that may require further investigation.

**Data Cleaning**

<img width="553" alt="Screenshot 2024-10-12 at 12 00 37 PM" src="https://github.com/user-attachments/assets/725ea43b-32bb-4f86-b713-35f2ee05ce29">

<br>

<img width="543" alt="Screenshot 2024-10-12 at 12 00 59 PM" src="https://github.com/user-attachments/assets/975e8313-57cf-4c26-ac55-03eafea613f3">


