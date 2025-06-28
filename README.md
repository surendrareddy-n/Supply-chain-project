# Supply-chain-project
 Evaluate and improve the overall efficiency of the supply chain operations for a logistics and distribution company that manages procurement, inventory, and transportation across multiple regions. The company collaborates with several suppliers and operates multiple warehouses to serve clients in different parts of the country. This project aims to provide deeper visibility into supplier performance, warehouse utilization, shipment timelines, and stock movement trends, enabling the leadership team to make informed strategic and operational decisions.

## Problem Statement
The company is currently facing several operational challenges across its supply chain network. Frequent stockouts in some warehouses are causing delivery delays and customer dissatisfaction, while certain warehouses remain underutilized. Supplier delivery performance is inconsistent, with some shipments arriving later than scheduled, disrupting inventory planning. Additionally, there is no centralized system to monitor key performance indicators such as stock movements, lead times, or delivery reliability. Due to a lack of real-time insights and fragmented data, the supply chain team is unable to proactively identify issues or optimize resource allocation. There is an urgent need to analyze existing operational data to uncover inefficiencies, identify performance gaps, and support timely, data-driven decision-making.

## My Tasks
As a hypothetical applicant for this role, I was tasked with:

Writing and executing SQL queries to answer these requests. Creating a presentation to showcase these insights, targeting top-level management.

## Sample SQL Queries

1. Identify the top 5 suppliers whose average delivery delay is the highest.

with delivery_delay as (

select supplier_id, avg(datediff(delivery_date, ship_Date)) as avg_delay

from shipments group by supplier_id 

)

select * from (

select *,

rank() over(order by avg_delay desc) as delay_rank 

from delivery_delay

) ranked

where delay_rank <= 5

2. Using the `warehouses` table, calculate the utilization percentage of each warehouse as `(current_utilization / capacity) * 100`. Then rank all warehouses in descending order of utilization percentage to identify which ones are most heavily used.

select warehouse_id,

round((current_utilization * 1.0 / capacity) * 100, 2) as utilization_pct,

rank() over(order by current_utilization * 1.0/ capacity desc) as rank_by_utilization

from warehouses

order by rank_by_utilization limit 10;

3.  calculate the number of total shipments and the number of delayed shipments (delay > 5 days) per month. Group the results by each month (use `ship_date`) to show the monthly trend of shipping performance.
   
with monthly_shipments as (

select date_format(ship_Date, "%Y-%M") as month, count(*) as total_shipments,

sum(case when datediff(delivery_date, ship_date) > 5 then 1 else 0 end) as delayed_shipments

from shipments

group by month

)

select * from monthly_shipments;

5.  Identify the top 3 transport modes from the `shipments` table that incurred the highest total `transport_cost` inthe last 6 months. Filter records using the `ship_date`, group them by `transport_mode`, sum up the cost, and then rank them in descending order.
   
WITH recent_transport AS (

SELECT * FROM shipments

WHERE ship_date >= CURRENT_DATE() - INTERVAL 6 MONTH

), 

mode_cost AS (

SELECT transport_mode, SUM(transport_cost) AS total_cost

FROM recent_transport

GROUP BY transport_mode

)

SELECT * FROM (

SELECT *, 

RANK() OVER (ORDER BY total_cost DESC) AS rnk

FROM mode_cost

) ranked

WHERE rnk <= 3;

## My Approach
Data Extraction with SQL:

-Used MySQL to write queries and fetch the needed data.

Presentation Design:

-Created a clear and professional presentation in Microsoft PowerPoint to share the insights.

Actionable Insights:

-Provided actionable insights and recommendations to assist the management team in making informed decisions.

## Outcome
This project showed my ability to work with complex data queries and share findings in a clear and engaging way. It helped me improve both my technical skills and my ability to explain insights effectively.

## Folder Structure
Air Cargo Requests: Document containing the 10 Air Cargo business requests. 

SQL Queries: Folder containing SQL scripts used to extract data.  

Presentation: PowerPoint file showcasing insights and recommendations.

## How to Use
SQL Queries:

Navigate to the SQL Queries folder.

Run the SQL scripts in your MySQL Workbench to extract the necessary data.

Presentation:

Open the PowerPoint file to view the presentation designed for top-level management.

This project highlights my skills in data analysis, SQL querying, data visualization, and presentation design in a business setting. It demonstrates my ability to extract and present actionable insights from data, supporting data-driven decision-making in a corporate environment.



