# Chapter 3: Designing Data Pipelines

## Question 1

**A large enterprise using GCP has recently acquired a startup that has an IoT platform. The enterprise wants to migrate the IoT platform from an on-premises data center to GCP and intends to use Google Cloud managed services whenever possible. What GCP service would you recommend for ingesting IoT data?**

A. Google Cloud Storage  
B. Cloud SQL  
C. Cloud Pub/Sub  
D. BigQuery streaming inserts  

**Correct Answer: C**  
_Explanation: Cloud Pub/Sub is ideal for ingesting real-time data on a large scale, such as data from IoT devices, making it suitable for this scenario._

## Question 2

**You are designing a data pipeline to populate a sales data mart. The sponsor of the project has had quality control problems in the past and has defined a set of rules for filtering out bad data before it gets into the data mart. At which stage of the data pipeline would you implement those rules?**

A. Ingestion  
B. Transformation  
C. Storage  
D. Analysis  

**Correct Answer: B**  
_Explanation: The transformation stage of a data pipeline is the most appropriate point to implement data quality rules, as it allows for the data to be cleaned and transformed before storage and analysis._
