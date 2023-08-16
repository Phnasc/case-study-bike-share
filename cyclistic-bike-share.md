# Cyclistic Bike Share
In this project, I conduct a analysis of Cyclistic bike share data, leveraging Google Cloud Platform's (GCP) BigQuery and API integration for robust data extraction. I dissect user behavior patterns, encompassing rideable classifications, trip distances, travel durations, and non-return scenarios. Through meticulous exploration of these dimensions, I aim to uncover substantiated trends that can serve as foundational insights for strategic decision-making in the context of the bike share system.
## Scenario
The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, the goal is to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, there will be designed a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve my recommendations, so they must be backed up with compelling data insights and visualizations.The datasets for this project are available at [Divvy](https://divvy-tripdata.s3.amazonaws.com/index.html). 
### Business Task
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?
## Prepare
In this project, we've gathered a comprehensive dataset comprising 12 CSV files. For the purpose of this analysis, our focus is exclusively on the data from the year 2022. This deliberate scope allows us to delve into user behavior patterns and glean insights that are specifically tied to this timeframe
<kbd>
  <pre>
   -- Create a new table named `tables_tripdata.tripdata2022_union` if it doesn't exist.
CREATE TABLE IF NOT EXISTS `tables_tripdata.tripdata2022_union` AS (
    -- Combine data from multiple tables for the year 2022 using UNION ALL.
    -- Each SELECT statement fetches data from a specific monthly table.
    
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202201`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202202`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202203`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202204`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202205`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202206`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202207`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202208`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202209`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202210`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202211`
    UNION ALL
    SELECT * FROM `project-analysis-bikes.tables_tripdata.202212`
);
  </pre>
  <pre>
    -- Count the total number of records in the consolidated table `tables_tripdata.tripdata2022_union`.
    
  SELECT COUNT(*) AS total_records
  FROM `project-analysis-bikes.tables_tripdata.tripdata2022_union`;
  </pre>
  Counting the records in the consolidated table, tripdata2022_union, which returned 5,667,717 rows.
  <pre>
    SELECT * FROM `tables_tripdata.tripdata2022_union` LIMIT 10;

    SELECT DISTINCT member_casual
    FROM `tables_tripdata.tripdata2022_union`;

    SELECT DISTINCT rideable_type
    FROM `tables_tripdata.tripdata2022_union`;
    
  </pre>
  In this step, the values ​​available in the columns rideable_type and member_casual were checked.
  - eletric_bike, classic bike, docked bike
  - casual and member
  <pre>
    SELECT column_name, data_type
    FROM `project-analysis-bikes.tables_tripdata`.INFORMATION_SCHEMA.COLUMNS
    WHERE table_name = 'tripdata2022_union';
  </pre>
  This query, which retrieves the column names and data types, holds critical importance as it provides a concise overview of the table's structure, facilitating a clear understanding of the available data fields and their corresponding data types. This knowledge is pivotal for effective data manipulation, transformation, and analysis, enabling accurate interpretation and appropriate handling of the dataset's attributes within subsequent analytical processes.
  
  ![column_types](https://github.com/Phnasc/case-study-bike-share/blob/main/images_cyclists_analysis/column_types.png)

  <pre>
    SELECT
    COUNTIF(ride_id IS NULL) AS ride_id_null_count,
    COUNTIF(rideable_type IS NULL) AS rideable_type_null_count,
    COUNTIF(started_at IS NULL) AS started_at_null_count,
    COUNTIF(ended_at IS NULL) AS ended_at_null_count,
    COUNTIF(start_station_name IS NULL) AS start_station_name_null_count,
    COUNTIF(start_station_id IS NULL) AS start_station_id_null_count,
    COUNTIF(end_station_name IS NULL) AS end_station_name_null_count,
    COUNTIF(end_station_id IS NULL) AS end_station_id_null_count,
    COUNTIF(start_lat IS NULL) AS start_lat_null_count,
    COUNTIF(start_lng IS NULL) AS start_lng_null_count,
    COUNTIF(end_lat IS NULL) AS end_lat_null_count,
    COUNTIF(end_lng IS NULL) AS end_lng_null_count,
    COUNTIF(member_casual IS NULL) AS member_casual_null_count
    FROM `tables_tripdata.tripdata2022_union`;
  </pre>
  The decision not to drop null values in the column related to the bikes' return is based on the absence of null values in the columns associated with bike withdrawal and return, implying that the rental transactions were successfully executed. This approach ensures that data integrity is maintained while examining non-return cases. Additionally, the observation that 80% of these incidents involve casual members raises the need for deeper analysis. A strategic exploration could involve geographic analysis, aiming to identify locations with higher non-return rates and assessing factors like accessibility and signage that might contribute. Investigating time of day, rush hours, late-night hours, day of the week variations, seasonal trends, and potential impacts of special events or festivals can provide insights into user behaviors and operational dynamics, facilitating effective strategies for minimizing non-return incidents.
</kbd>

