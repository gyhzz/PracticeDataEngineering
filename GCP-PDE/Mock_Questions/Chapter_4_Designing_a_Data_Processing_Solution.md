# Chapter 4: Designing a Data Processing Solution

Correct answers for each question can be found below.

## Question 1

**A startup is designing a data processing pipeline for its IoT platform. Data from sensors will stream into a pipeline running in GCP. As soon as data arrives, a validation process, written in Python, is run to verify data integrity. If the data passes the validation, it is ingested; otherwise, it is discarded. What services would you use to implement the validation check and ingestion?**

A. Cloud Storage and Cloud Pub/Sub  
B. Cloud Functions and Cloud Pub/Sub  
C. Cloud Functions and BigQuery  
D. Cloud Storage and BigQuery  


## Question 2

**Your finance department is migrating a third-party application from an on-premises physical server. The system was written in C, but only the executable binary is available. After the migration, data will be extracted from the application database, transformed, and stored in a BigQuery data warehouse. The application is no longer actively supported by the original developer, and it must run on an Ubuntu 14.04 operating system that has been configured with several required packages. Which compute platform would you use?**

A. Compute Engine  
B. Kubernetes Engine  
C. App Engine Standard  
D. Cloud Functions  


## Question 3

**A team of developers has been tasked with rewriting the ETL process that populates an enterprise data warehouse. They plan to use a microservices architecture. Each microservice will run in its own Docker container. The amount of data processed during a run can vary, but the ETL process must always finish within one hour of starting. You want to minimize the amount of DevOps tasks the team needs to perform, but you do not want to sacrifice efficient utilization of compute resources. What GCP compute service would you recommend?**

A. Compute Engine  
B. Kubernetes Engine  
C. App Engine Standard  
D. Cloud Functions  


---


**Question 1 Correct Answer: B**  
_Explanation: Cloud Functions is ideal for running the Python validation script as it allows for executing code in response to events, such as incoming data streams, without needing to manage a server or runtime environment. Cloud Pub/Sub supports this process by facilitating the ingestion and distribution of streaming data efficiently. This combination ensures real-time data validation and ingestion without unnecessary storage or complex configurations._

**Question 2 Correct Answer: A**  
_Explanation: Compute Engine provides the flexibility to configure virtual machines extensively, including the choice of the operating system. This capability makes it ideal for running legacy applications that rely on specific OS versions and configurations, such as Ubuntu 14.04 with custom packages. It allows for full control over the virtual machine environment, which is necessary for hosting an application that is not supported by its original developers and for which only the binary is available._

**Question 3 Correct Answer: B**  
_Explanation: Kubernetes Engine is ideal for this scenario because it provides a managed environment for deploying, managing, and scaling containerized applications using Kubernetes. The service automatically handles the scaling and management of the infrastructure, which minimizes DevOps tasks. Kubernetes Engine's ability to handle variable workloads efficiently and ensure that resources are optimally used makes it suitable for the ETL process described, which needs to adapt to varying data volumes and complete within a fixed time frame.

- **Why not A, Compute Engine?** Compute Engine would require significant management of virtual machines and scaling, which increases the DevOps burden.
- **Why not C, App Engine Standard?** App Engine Standard is not well-suited for containerized applications with specific resource requirements, as it abstracts away much of the infrastructure management and limits control over the environment.
- **Why not D, Cloud Functions?** Cloud Functions is designed for lighter, event-driven processes and may not efficiently handle the heavy and variable data loads typical in ETL processes within the strict time constraints specified.**
