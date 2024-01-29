# Airline_Data_Ingestion_AWS

Hello!

Tech Stack Used: S3, S3 Cloud Trail notification, Event Bridge pattern rule, Glue Crawler, Glue ETL, SNS for Notification, Redshift warehouse, Step function for Orchestration

First we create a glue crawler and load our airports csv data into our Redshift warehouse which will be used as a dimention table.

Next we create a S3 bucket where our daily flights data wil be added and create a glue crawler and store the metadata in a external s3 location.

Next we create Glue ETL job for and then chnage it to script format and make changes if required.

In the glue job we are first setup 2 data sources to read the data from the dimention table and our daily raw flights data from S3 bucket. Then we perform a inner join to join the departure_id and airport_id between our data sources and and then transform the data as per the right data types and remove any unwanted columns.

Next we again we perform join to between arrival_id and airport_id for the destination airport names and store the transformed data into Redshift.

For the above job to run we have to setup connections between redshift and S3 and setup end end point in VPC and provide the right access policy and IAM roles for it.



After the Glue job prepation we use Stepfunction to setup the flow of our job.

First we run our crawler to check for any new files that have been added and gather the metadata. After which we get the status of our crawler to check if it has complete and not in runnind state.

Depending on the status of the crawler we then start the glue job or else wait for the crawler to move to stopped state.

Once the Glue job is run we then setup SNS notification to check if the status of the job and send a mail notification if the glue job has failed or succeed.



To automate it completely we use CloudTrail service to check and create event notification for our S3 bucket whenever a new file is added to our S3 bucket. We create a new IAM role for our cloudwatch and attach policies for S3 bucket.

Next we use Event Bridge service to start our step function whenever a event notification is generaed by our cloud trail.
