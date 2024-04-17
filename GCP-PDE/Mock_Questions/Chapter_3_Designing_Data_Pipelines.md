# Chapter 3: Designing Data Pipelines

Correct answers for each question can be found below.

## Question 1

**A large enterprise using GCP has recently acquired a startup that has an IoT platform. The enterprise wants to migrate the IoT platform from an on-premises data center to GCP and intends to use Google Cloud managed services whenever possible. What GCP service would you recommend for ingesting IoT data?**

A. Google Cloud Storage  
B. Cloud SQL  
C. Cloud Pub/Sub  
D. BigQuery streaming inserts  


## Question 2

**You are designing a data pipeline to populate a sales data mart. The sponsor of the project has had quality control problems in the past and has defined a set of rules for filtering out bad data before it gets into the data mart. At which stage of the data pipeline would you implement those rules?**

A. Ingestion  
B. Transformation  
C. Storage  
D. Analysis  


## Question 3

**A team of data warehouse developers is migrating a set of legacy Python scripts that have been used to transform data as part of an ETL process. They would like to use a service that allows them to use Python and requires minimal administration and operations support. Which GCP service would you recommend?**

A. Cloud Dataproc  
B. Cloud Dataflow  
C. Cloud Spanner  
D. Cloud Dataprep  


## Question 4

**You are using Cloud Pub/Sub to buffer records from an application that generates a stream of data based on user interactions with a website. The messages are read by another service that transforms the data and sends it to a machine learning model that will use it for training. A developer has just released some new code, and you notice that messages are sent repeatedly at 10-minute intervals. What might be the cause of this problem?**

A. The new code release changed the subscription ID  
B. The new code release changed the topic ID  
C. The new code disabled acknowledgments from the consumer  
D. The new code changed the subscription from pull to push  


## Question 5

**It is considered a good practice to make your processing logic idempotent when consuming messages from a Cloud Pub/Sub topic. Why is that?**

A. Messages may be delivered multiple times.  
B. Messages may be received out of order.  
C. Messages may be delivered out of order.  
D. A consumer service may need to wait extended periods of time between the delivery of messages.



---


**Question 1 Correct Answer: C**  
_Explanation: Cloud Pub/Sub is ideal for ingesting real-time data on a large scale, such as data from IoT devices, making it suitable for this scenario._

**Question 2 Correct Answer: B**  
_Explanation: The transformation stage of a data pipeline is the most appropriate point to implement data quality rules, as it allows for the data to be cleaned and transformed before storage and analysis._

**Question 3 Correct Answer: B**  
_Explanation: Cloud Dataflow is an ideal choice for this scenario as it provides a managed service for processing and transforming large datasets using Python. It requires minimal administration, scales automatically, and integrates seamlessly with other GCP services._

**Question 4 Correct Answer: C**  
_Explanation: In Google Cloud Pub/Sub, if the consumer does not acknowledge the messages after processing them, the service considers the messages unprocessed and will redeliver them. This issue can occur if acknowledgments are disabled, leading to repeated delivery of the same messages._

**Question 5 Correct Answer: A**  
_Explanation: Making processing logic idempotent is crucial because Cloud Pub/Sub does not guarantee exactly-once delivery; it operates with at-least-once delivery semantics, where a message might be delivered more than once. Idempotent processing ensures that the repeated handling of the same message does not affect the final outcome, thereby preventing data inconsistencies or duplicates. This makes it particularly important to handle potentially duplicated messages appropriately._

