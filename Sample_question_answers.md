## Question 1
 
You are working on optimizing BigQuery for a query that is run repeatedly on a single table. The data queried is about 1 GB, and some rows are expected to change about 10 times every hour. You have optimized the SQL statements as much as possible. You want to further optimize the query's performance. What should you do?
A. Create a materialized view based on the table, and query that view.
B. Enable caching of the queried data so that subsequent queries are faster.
C. Create a scheduled query, and run it a few minutes before the report has to be created.
D. Reserve a larger number of slots in advance so that you have maximum compute power to execute the query.
 
Correct answer
A. Create a materialized view based on the table, and query that view.
Feedback
A: Option A is correct because materialized views periodically cache the results of a query for increased performance. Materialized views are suited to small datasets that are frequently queried. When underlying table data changes, the materialized view invalidates the affected portions and re-reads them.

B: Option B is not correct because caching is automatically enabled but is not performant when the underlying data changes.

C: Option C is not correct because scheduled queries let you schedule recurring queries but do not provide specific performance
optimizations. Also, running a query too early could use old/stale data.

D: Option D is not correct because reserving more slots guarantees the availability of BigQuery slots but does not improve performance.
 
https://cloud.google.com/bigquery/docs/materialized-views-intro
 
https://cloud.google.com/bigquery/docs/materialized-views-best-practices
 
https://cloud.google.com/bigquery/docs/materialized-views
## Question 2
 
Several years ago, you built a machine learning model for an ecommerce company. Your model made good predictions. Then a global pandemic occurred, lockdowns were imposed, and many people started working from home. Now the quality of your model has degraded. You want to improve the quality of your model and prevent future performance degradation. What should you do?

A. Retrain the model with data from the first 30 days of the lockdown.
B. Monitor data until usage patterns normalize, and then retrain the model.
C. Retrain the model with data from the last 30 days. After one year, return to the older model.
D. Retrain the model with data from the last 30 days. Add a step to continuously monitor model input data for changes, and retrain the model.
 
Feedback
A: Option A is not correct because retraining based on the data from the first 30 days of the lockdown might only be useful for predictions during similar lockdowns and not for regular periods.
B: Option B is not correct because usage patterns might have changed permanently and might continue to change in the future.
C: Option C is not correct because the older model might not be indicative of user behavior after a year.
D: Option D is correct because the data used to build the original model is no longer relevant. Retraining the model with recent data from the last 30 days will improve the predictions. To keep a watch on future data drifts, monitor the incoming data.
 
https://cloud.google.com/blog/topics/developers-practitioners/monitor-models-training-serving-skew-vertex-ai
## Question 3
 
A new member of your development team works remotely. The developer will write code locally on their laptop, which will connect to a MySQL instance on Cloud SQL. The instance has an external (public) IP address. You want to follow Google-recommended practices when you give access to Cloud SQL to the new team member. What should you do?
A. Ask the developer for their laptop's IP address, and add it to the authorized networks list.
B. Remove the external IP address, and replace it with an internal IP address. Add only the IP address for the remote developer's laptop to the authorized list.
C. Give instance access permissions in Identity and Access Management (IAM), and have the developer run Cloud SQL Auth proxy to connect to a MySQL instance.
D. Give instance access permissions in Identity and Access Management (IAM), change the access to "private service access" for security, and allow the developer to access Cloud SQL from their laptop.
 
Correct answer
C. Give instance access permissions in Identity and Access Management (IAM), and have the developer run Cloud SQL Auth proxy to connect to a MySQL instance.
Feedback
A: Option A is not correct, because although adding an authorized networks list is possible, it is more effort to track it and also less secure.
B: Option B is not correct, because if you remove the external IP address, access for those who work remotely will be more complicated because they also have to be within private RFC 1918 address space.
C: Option C is correct because the recommended approach is to use Cloud SQL Auth proxy. Permissions can be controlled by IAM. You don't need to track authorization lists for changing user IP addresses.
D: Option D is not correct because private service access will require access from a private RFC 1918 address space, which might not be available to developers who work remotely.
 
https://cloud.google.com/sql/docs/mysql/sql-proxy
 
https://cloud.google.com/sql/docs/mysql/connect-admin-proxy
 
https://cloud.google.com/sql/docs/mysql/connect-overview
 
https://cloud.google.com/sql/docs/postgres/configure-ip
 
https://codelabs.developers.google.com/codelabs/cloud-sql-connectivity-gce-private
## Question 4
 
Your Cloud Spanner database stores customer address information that is frequently accessed by the marketing team. When a customer enters the country and the state where they live, this information is stored in different tables connected by a foreign key. The current architecture has performance issues. You want to follow Google-recommended practices to improve performance. What should you do?

A. Create interleaved tables, and store states under the countries.
    -- good choice for many parent-child relationships. With interleaving, Spanner physically co-locates child rows with parent rows in storage.
    -- Co-location can significantly improve performance.
B. Denormalize the data, and have a row for each state with its corresponding country.
C. Retain the existing architecture, but use short, two-letter codes for the countries and states.
D. Combine the countries in a single cell's text, for example "country:state1,state2, …" and when required, split the data.
Correct answer
A. Create interleaved tables, and store states under the countries.
Feedback
A: Option A is correct because Cloud Spanner supports interleaving that guarantees data being stored in the same split, which is performant when you need a strong data locality relationship.
B: Option B is not correct because denormalizing is not a preferred approach in relational databases. It leads to multiple rows with repeated data.
C: Option C is not correct because reducing the size of the fields to short names will have lower impact because the data access and joins will be a bigger performance issue.
D: Option D is not correct because packing multiple types of data into the same cell is not recommended for relational databases.
 
https://cloud.google.com/spanner/docs/schema-and-data-model#creating-interleaved-tables
## Question 5
 
Your company runs its business-critical system on PostgreSQL. The system is accessed simultaneously from many locations around the world and supports millions of customers. Your database administration team manages the redundancy and scaling manually. You want to migrate the database to Google Cloud. You need a solution that will provide global scale and availability and require minimal maintenance. What should you do?
A. Migrate to BigQuery.
B. Migrate to Cloud Spanner.
C. Migrate to a Cloud SQL for PostgreSQL instance.
D. Migrate to bare metal machines with PostgreSQL installed.
Feedback
A: Option A is not correct because BigQuery doesn’t support global scale. BigQuery also isn’t the best option for migrating a transactional database like PostgreSQL because it is more analytics-focused.
B: Option B is correct because Cloud Spanner provides a global-scale, highly available database that supports relational data.
C: Option C is not correct because Cloud SQL options are regional and have less scalability compared to Cloud Spanner.
D: Option D is not correct because running PostgreSQL on bare metal machines requires a greater maintenance effort.
 
https://cloud.google.com/spanner/docs/migrating-postgres-spanner
## Question 6
 
Your company collects data about customers to regularly check their health vitals. You have millions of customers around the world. Data is ingested at an average rate of two events per 10 seconds per user. You need to be able to visualize data in Bigtable on a per user basis. You need to construct the Bigtable key so that the operations are performant. What should you do?
A. Construct the key as user-id#device-id#activity-id#timestamp.
B. Construct the key as timestamp#user-id#device-id#activity-id.
    -- Row keys that start with a timestamp. This pattern causes sequential writes to be pushed onto a single node, creating a hotspot. 
    -- If you put a timestamp in a row key, precede it with a high-cardinality value like a user ID to avoid hotspots.
C. Construct the key as timestamp#device-id#activity-id#user-id.
D. Construct the key as user-id#timestamp#device-id#activity-id.
 
Correct answer
A. Construct the key as user-id#device-id#activity-id#timestamp.
Feedback
A: Option A is correct because the design does not monotonically increase, thus avoiding hotspots.
B: Option B is not correct because it monotonically increases, thus causing hotspots.
C: Option C is not correct because it monotonically increases, thus causing hotspots.
D: Option D is not correct because it monotonically increases, thus causing hotspots.
 
https://cloud.google.com/bigtable/docs/schema-design
## Question 7
 
Your company is hiring several business analysts who are new to BigQuery. The analysts will use BigQuery to analyze large quantities of data. You need to control costs in BigQuery and ensure that there is no budget overrun while you maintain  the quality of query results. What should you do?

A. Set a customized project-level or user-level daily quota to acceptable values.
 
B. Reduce the data in the BigQuery table so that the analysts query less data, and then archive the remaining data.
C. Train the analysts to use the query validator or --dry_run to estimate costs so that the analysts can self-regulate usage.
D. Export the BigQuery daily costs, and visualize the data on Looker on a per-analyst basis so that the analysts can self-regulate usage.
Feedback
A: Option A is correct because if you have multiple BigQuery projects and users, you can manage costs by requesting a custom quota that specifies a limit on the amount of query data processed per day.
B: Option B is not correct because giving only partial data to the analysts might not produce accurate query results.
C: Option C is not correct because costs could still overrun budgets. This approach assumes that analysts always follow guidelines.
D: Option D is not correct because your costs could still overrun budgets. This approach also assumes that the analysts look at the charts daily and adjust their behavior.
 
https://cloud.google.com/bigquery/docs/custom-quotas
## Question 8
 
Your Bigtable database was recently deployed into production. The scale of data ingested and analyzed has increased significantly, but the performance has degraded. You want to identify the performance issue. What should you do?

A. Use Key Visualizer to analyze performance.
B. Use Cloud Trace to identify the performance issue.
 
C. Add logging statements into the code to see which inserts cause the delay.
D. Add more nodes to the cluster to see if that resolves the performance issue.
Correct answer
A. Use Key Visualizer to analyze performance.
Feedback
A: Option A is correct because Key Visualizer for Bigtable generates visual reports for your tables that detail your usage based on the row keys that you access, show you how Bigtable operates, and can help you troubleshoot performance issues.
B: Option B is not correct because Cloud Trace is used to debug latency in applications.
C: Option C is not correct because adding logging statements won't help you understand the performance issues within Bigtable.
D: Option D is not correct because adding more nodes might improve the performance, but the database could continue to have performance issues if the keys are not designed well.
 
https://cloud.google.com/bigtable/docs/keyvis-overview
## Question 9
 
Your company is moving your data analytics to BigQuery. Your other operations will remain on-premises. You need to transfer 800 TB of historic data. You also need to plan for 30 Gbps of daily data transfers that must be appended for analysis the next day. You want to follow Google-recommended practices to transfer your data. What should you do?

A. As early as possible every day, use Cloud VPN to transfer the existing data over the internet.
B. Use a Transfer Appliance to move the existing data to Google Cloud. Use Cloud VPN to transfer data daily.
C. Use a Transfer Appliance to move the existing data to Google Cloud.. Use VPC Network Peering to transfer data daily.
D. Use a Transfer Appliance to move the existing data to Google Cloud. Set up a Dedicated or Partner Interconnect for daily transfers.
 
Feedback
A: Option A is not correct because the internet in general will have less stability and much lower speed. Transferring large amounts of data is not viable.
B: Option B is not correct because Cloud VPN is useful for data transfers at a rate of a few Gbps (1.5 Gbps to 3 Gbps).
C: Option C is not correct because VPC Network Peering is used for data transfers within Google Cloud Organizations.
D: Option D is correct because using a Transfer Appliance is recommended to transfer hundreds of terabytes of data. For large data transfers that occur regularly, a dedicated, hybrid networking connection is recommended.
 
https://cloud.google.com/hybrid-connectivity
 
https://cloud.google.com/transfer-appliance/docs/4.0
 
https://cloud.google.com/network-connectivity/docs/interconnect/concepts/overview
## Question 10
 
Your team runs Dataproc workloads where the worker node takes about 45 minutes to process. You have been exploring various options to optimize the system for cost, including shutting down worker nodes aggressively. However, in your metrics you see that the entire job takes even longer. You want to optimize the system for cost without increasing job completion time. What should you do?

A. Set a graceful decommissioning timeout greater than 45 minutes.
B. Rewrite the processing in Cloud Data Fusion, and run the job automatically.
C. Rewrite the processing in Dataflow, and use stream processing of the same data.
D. Increase the number of vCPUs on each worker node so that the processing finishes sooner.
 
Feedback
A: Option A is correct because graceful decommissioning will finish work in progress on a worker node before it is removed from the Dataproc cluster.
B: Option B is not correct because rebuilding the data pipeline in Cloud Data Fusion will increase effort, cost, and time.
C: Option C is not correct because rewriting the code in Dataflow will increase effort, cost, and time.
D: Option D is not correct because increasing the number of vCPUs will greatly increase the cost.
 
https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/scaling-clusters
 
https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/autoscaling#choosing_a_graceful_decommissioning_timeout
## Question 11
 
Your customer has a SQL Server database that contains about 5 TB of data in another public cloud. You expect the data to grow to a maximum of 25 TB. The database is the backend of an internal reporting application that is used once a week. You want to migrate the application to Google Cloud to reduce administrative effort while keeping costs the same or reducing them. What should you do?

A. Migrate the database to Bigtable.
B. Migrate the database to Cloud Spanner.
C. Install SQL Server on a Compute Engine VM.
D. Migrate the database to SQL Server in Cloud SQL.
 
Feedback
A: Option A is not correct because Bigtable is a NoSQL database and is not a suitable destination from a SQL Server source.
B: Option B is not correct because Cloud Spanner will be costlier than Cloud SQL. Although Spanner has global availability, it is unnecessary for this application requirement.
C: Option C is not correct because installing a custom SQL Server instance on a Compute Engine VM will require more administrative effort.
D: Option D is correct because Cloud SQL provides managed MySQL, PostgreSQL, and SQL Server databases, which will reduce administrative effort. Twenty-five TB can be accommodated efficiently on Cloud SQL.
 
https://cloud.google.com/sql/docs/sqlserver/quickstart
 
https://cloud.google.com/sql/docs/sqlserver/import-export/import-export-sql
 
https://cloud.google.com/products/databases
 
https://cloud.google.com/sql
## Question 12
 
Your IT team uses BigQuery for storing structured data. Your finance team recently moved to Google Workspace Enterprise edition from a standalone, desktop-based spreadsheet processor. When the finance team needs data insights, the IT team runs a query on BigQuery, exports the data to a CSV file, and sends the file as an email attachment to the finance team members. You want to improve the process while you retain familiar methods of data analysis for the finance team. What should you do?

A. Run the query in BigQuery, and give the finance team access to the results view, which can be analyzed.
 
B. Run the query in BigQuery, and give the finance team access to the data visualizations in Google Data Studio.
C. Run the query in BigQuery, export the data to CSV, upload the file to a Cloud Storage bucket, and share the file with the finance team.
D. Run the query in BigQuery, and save the results to a Google Sheets shared spreadsheet that can be accessed and analyzed by the finance team.
Correct answer
D. Run the query in BigQuery, and save the results to a Google Sheets shared spreadsheet that can be accessed and analyzed by the finance team.
Feedback
A: Option A is not correct because the finance team will have to be given Google Cloud access and be trained on using BigQuery, which is not a familiar method.
B: Option B is not correct because only giving the visualizations on Data Studio won't let the finance teams analyze the data.
C: Option C is not correct because the finance team will have to be given Google Cloud access and be trained on using Cloud Storage, which is not a familiar method.
D: Option D is correct because Connected Sheets gives you a direct and easy way to share BigQuery data through Google Sheets.
 
https://cloud.google.com/bigquery/docs/connected-sheets
 
https://cloud.google.com/bigquery/docs/writing-results#saving-query-results-to-sheets
 
https://www.youtube.com/watch?v=rkimIhnLKGI
## Question 13
 
Your scooter-sharing company collects information about their scooters, such as location, battery level, and speed. The company visualizes this data in real time. To guard against intermittent connectivity, each scooter sends repeats of certain messages within a short interval. Occasional data errors have been noticed. The messages are received in Pub/Sub and stored in BigQuery. You need to ensure that the data does not contain duplicates and that erroneous data with empty fields is rejected. What should you do?

A. Store the data in BigQuery, and run delete queries on erroneous and duplicate data.
B. Use Dataflow to subscribe to Pub/Sub, process the data, and store the data in BigQuery.
 
C. Use Kubernetes to create a microservices application that can remove duplicates and erroneous data. Then insert the data into BigQuery.
D. Create an application on Compute Engine with Managed Instance Groups that can remove duplicates and erroneous data. Then insert the data into BigQuery.
Feedback
A: Option A is not correct because directly storing data in BigQuery could cause data to be overwritten, and erroneous data could be present before it is deleted. Workarounds within BigQuery to circumvent these concerns would cost more effort, time, and money.
B: Option B is correct because Dataflow is the recommended data processing product for streaming data. Dataflow can be programmed to remove duplicates, delete empty fields, and perform other custom data processing.
C: Option C is not correct because creating a custom application for streaming processing on Kubernetes is a significant effort and is not recommended.
D: Option D is not correct because creating a custom application for streaming processing on Compute Engine is a significant effort and is not recommended.
 
https://cloud.google.com/architecture/building-production-ready-data-pipelines-using-dataflow-planning
## Question 14
 
Your cryptocurrency trading company visualizes prices to help your customers make trading decisions. Because different trades happen in real time, the price data is fed to a data pipeline that uses Dataflow for processing. You want to compute moving averages. What should you do?

A. Use hopping windows in Dataflow.
 
B. Use session windows in Dataflow.
C. Use tumbling windows in Dataflow.
D. Use Dataflow SQL, and compute averages grouped by time.
Feedback
A: Option A is correct because you use hopping windows to compute moving averages.
B: Option B is not correct because session windows are not used to calculate moving averages.
C: Option C is not correct because tumbling windows are not used to calculate moving averages.
D: Option D is not correct because grouping by time alone does not give you a moving average.
 
https://cloud.google.com/dataflow/docs/concepts/streaming-pipelines
## Question 15
 
You are building the trading platform for a stock exchange with millions of traders. Trading data is written rapidly. You need to retrieve data quickly to show visualizations to the traders, such as the changing price of a particular stock over time. You need to choose a storage solution in Google Cloud. What should you do?

A. Use Bigtable.
 
B. Use Firestore.
C. Use Cloud SQL.
D. Use Memorystore.
Feedback
A: Option A is correct because Bigtable is the recommended database for time series data that requires high throughput reads and writes.
B: Option B is not correct because Firestore does not have the high throughput capabilities that are suitable for time-series data.
C: Option C is not correct because Cloud SQL does not the have high throughput capabilities that are suitable for time-series data.
D: Option D is not correct because Memorystore is a fast in-memory database that is not suitable for persistently storing large amounts of data.
 
https://cloud.google.com/bigtable/docs/overview#what-its-good-for
## Question 16
 
Your customer uses Hadoop and Spark to run data analytics on-premises. The main data is stored in hard disks that are centrally accessed. Your customer needs to migrate their workloads to Google Cloud efficiently while considering scalability. You want to select an architecture that requires minimal effort. What should you do?

A. Use Dataproc to run Hadoop and Spark jobs. Move the data to Cloud Storage.
 
B. Use Dataflow to recreate the jobs in a serverless approach. Move the data to Cloud Storage.
C. Use Dataproc to run Hadoop and Spark jobs. Retain the data on a Compute Engine VM with an attached persistent disk.
D. Use Dataflow to recreate the jobs in a serverless approach. Retain the data on a Compute Engine VM with an attached persistent disk.
Feedback
A: Option A is correct because Dataproc is a fully managed service for hosting open source distributed processing platforms, such as Apache Spark, Presto, Apache Flink and Apache Hadoop on Google Cloud. Cloud Storage is the preferred storage option for all persistent storage needs.
B: Option B is not correct because using Dataflow requires you to rewrite all the jobs.
C: Option C is not correct because storing the centrally accessed data on persistent disks is not recommended.
D: Option D is not correct because using Dataflow requires you to rewrite all the jobs. Storing the centrally accessed data on persistent disks is not recommended.
 
https://cloud.google.com/architecture/hadoop/hadoop-gcp-migration-jobs
 
https://cloud.google.com/dataproc/docs/concepts/connectors/cloud-storage
 
https://cloud.google.com/blog/topics/developers-practitioners/dataproc-best-practices-guide
## Question 17
 
You are on a team of analysts who work with BigQuery and are already proficient in SQL. Your team needs to build a multi-label machine learning classification model that uses data in BigQuery. There are 6,000 rows of data in your training dataset. The inferences could be one of 200 possible labels. You want to create a high accuracy model. What should you do?

A. Use BigQuery ML to create a model.
 
B. Export the data to CSV files. Use TensorFlow to build a model.
C. Connect the data from BigQuery to AutoML, and build the model in AutoML.
D. Connect to the data through AI notebooks, and build your model interactively.
Correct answer
C. Connect the data from BigQuery to AutoML, and build the model in AutoML.
Feedback
A: Option A is not correct because BigQuery ML requires a lot of data to build an accurate model, and you don't have much data.
B: Option B is not correct because there isn't enough data to build a custom model in TensorFlow.
C: Option C is correct because the amount of data is relatively low and also varied. A model built using only this data wouldn't be accurate. AutoML is appropriate because it uses transfer learning based on other similar data.
D: Option D is not correct because there isn't enough data to build a custom model with AI Notebooks.
 
https://cloud.google.com/vertex-ai/docs/start/automl-model-types#tabular
## Question 18
 
You used a small amount of data to build a machine learning model that gives you good inferences during testing. However, the results show more errors when real-world data is used to run the model. No additional data can be collected for testing. You want to get a more accurate view of the model's capability. What should you do?

A. Reduce the amount of data to improve the model.
B. Cross-validate the data, and re-run the model building process.
 
C. Create feature crosses that will add new columns to increase the data.
D. Duplicate the data twice to increase the data, and re-run the model building process.
Feedback
A: Option A is not correct because this model is not underfitting.
B: Option B is correct because this model appears to be overfitting. Using cross-validation will run the validation on multiple folds of the data, which reduces the overfitting.
C: Option C is not correct because adding new columns will not reduce the overfitting.
D: Option D is not correct because duplicating the data will not reduce the overfitting.
 
https://developers.google.com/machine-learning/crash-course/generalization/peril-of-overfitting
## Question 19
 
Your organization has been collecting information for many years about your customers, including their address and credit card details. You plan to use this customer data to build machine learning models on Google Cloud. You are concerned about private data leaking into the machine learning model. Your management is also concerned that direct leaks of personal data could damage the company's reputation. You need to address these concerns about data security. What should you do?

A. Remove all the tables that contain sensitive data.
B. Use libraries like SciPy to build the ML models on your local computer.
C. Remove the sensitive data by using the Cloud Data Loss Prevention (DLP) API.
D. Identify the rows that contain sensitive data, and apply SQL queries to remove only those rows.
 
Correct answer
C. Remove the sensitive data by using the Cloud Data Loss Prevention (DLP) API.
Feedback
A: Option A is not correct because removing data, such as entire tables, could reduce the effectiveness of the resulting model.
B: Option B is not correct because building machine learning models on individual computers is not a viable approach when it involves large amounts of data.
C: Option C is correct because Cloud DLP is the recommended approach to redact, mask, tokenize, and transform text and images to help protect data privacy.
D: Option D is not correct because removing data, such as full rows, could reduce the effectiveness of the resulting model.
 
https://cloud.google.com/dlp/docs/concepts-de-identification
## Question 20
 
Your healthcare application has a backend system that accepts event data directly from IoT devices. Recent increases of the application's users and devices are causing a sudden influx of data that overwhelms the system. You need to redesign the data pipeline to ensure that all data is processed and that no events are lost. You want to follow Google-recommended practices. What should you do?  

A. Use Kafka with pull mode.
B. Use Pub/Sub with pull mode.
 
C. Use Pub/Sub with push mode.
D. Run Cloud Scheduler at fixed intervals.
Feedback
A: Option A is not correct because Kafka is not a managed solution in Google Cloud. The Google-recommended option is Pub/Sub, a fully managed, serverless solution.
B: Option B is correct because pull mode allows new event data to be pulled for processing on demand when the previous data is processed. Pub/Sub will absorb and retain new events in the interim without losing them.
C: Option C is not correct because Pub/Sub in push mode could continue to overwhelm the system.
D: Option D is not correct because new event data should be pulled for processing when the previous processing is completed, and that is not expected to be at fixed intervals.
 
https://cloud.google.com/pubsub/docs/pull
 
https://cloud.google.com/pubsub/docs/push
