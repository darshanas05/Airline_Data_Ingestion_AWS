# Airline_Data_Ingestion_AWS

Greetings!

Let's delve into the intricate web of technologies and services we've employed for this data processing pipeline:

Tech Stack Utilized:

**Amazon S3:** Our foundational storage solution for both storing raw data and facilitating communication between various components of our pipeline.

**S3 CloudTrail Notification:** Enables us to monitor S3 bucket activity and trigger events based on file additions.

**EventBridge Pattern Rule:** Allows us to detect specific patterns in S3 events and trigger subsequent actions accordingly.

**AWS Glue:** A key player in our pipeline, with its Crawler and ETL capabilities for data discovery, transformation, and loading.

**Amazon SNS:** Facilitates real-time notification delivery, crucial for monitoring the status of our pipeline operations.

**Amazon Redshift:** Our data warehouse solution, where transformed data is stored and made available for analytical queries.

**AWS Step Functions:** Orchestrates the flow of our pipeline, ensuring seamless execution of tasks in a defined sequence.


****Pipeline Workflow:****

**Data Preparation:**

We kick off by utilizing Glue Crawler to discover and catalog our airports CSV data, loading it into Redshift as a dimension table.
Additionally, we set up an S3 bucket to receive daily flights data, employing another Glue Crawler to capture metadata stored externally.

**Data Transformation:**

Leveraging Glue ETL, we craft a job to transform our raw flight data, starting with setting up data sources for both the dimension table and daily flights data.
A critical step involves performing inner joins to enrich our data, followed by necessary transformations and column pruning.
Further enrichment is achieved by performing joins with destination airport data before storing the transformed dataset back into Redshift.

**Pipeline Orchestration:**

With the Glue job ready, Step Functions takes the reins, orchestrating the entire pipeline flow.
It begins by triggering the Glue Crawler to scan for new files and waits for its completion before proceeding.
Depending on the crawler's status, Step Functions initiates the Glue job or awaits readiness before execution.
Upon completion, SNS comes into play, sending out notifications regarding the success or failure of the Glue job.

**Automation and Event-Driven Workflow:**

We achieve full automation by leveraging AWS CloudTrail to monitor S3 bucket activity and set up event notifications.
Through EventBridge, we identify specific patterns in S3 events, triggering the initiation of our Step Functions workflow.
To ensure seamless operation, we configure IAM roles and attach relevant policies for secure access and permissions management.

By intricately weaving together these services and technologies, we've crafted a robust and efficient data processing pipeline, capable of handling large volumes of data while maintaining scalability, reliability, and real-time monitoring capabilities.
