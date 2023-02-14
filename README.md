# GCP_DE_certification

https://cloud.google.com/certification/guides/data-engineer/

## How did I prepare it

3 months before the exam:

* Book the exam
* [Cloudskillsboost courses](https://www.cloudskillsboost.google/course_templates/72)
* [Official Study guide](https://www.amazon.com/Official-Google-Certified-Professional-Engineer/dp/1119618436/ref=asc_df_1119618436/?tag=hyprod-20&linkCode=df0&hvadid=459440273404&hvpos=&hvnetw=g&hvrand=8733177887651538298&hvpone=&hvptwo=&hvqmt=&hvdev=m&hvdvcmdl=&hvlocint=&hvlocphy=9007372&hvtargid=pla-912616327829&psc=1) : questions at the end of each chapter are useful

1 week before the exam:

* get the 1-week free trial tier in acloudguru for practice exams
* Google official documentation

Complementary reads (not necessary for the exam):

* [Fundamentals of Data Engineering](https://www.amazon.com/Fundamentals-Data-Engineering-Robust-Systems/dp/1098108302/ref=sr_1_1?keywords=data+engineering&qid=1673798311&s=books&sprefix=data+en%2Cstripbooks-intl-ship%2C215&sr=1-1)
* [The Data Warehouse Toolkit: The Definitive Guide to Dimensional Modeling](https://www.amazon.com/Data-Warehouse-Toolkit-Definitive-Dimensional/dp/1118530802/ref=d_pd_sbs_vft_none_sccl_3_6/143-7068543-9046039?pd_rd_w=wrPPW&content-id=amzn1.sym.3676f086-9496-4fd7-8490-77cf7f43f846&pf_rd_p=3676f086-9496-4fd7-8490-77cf7f43f846&pf_rd_r=2AFX4NCZPC67JNJXQCDH&pd_rd_wg=F7nby&pd_rd_r=0279bab2-6beb-41d3-a853-1a6755f697bf&pd_rd_i=1118530802&psc=1)

## Exam topics

Section 1: Designing data processing systems

1.1 Selecting the appropriate storage technologies. Considerations include:

    ●  Mapping storage systems to business requirements
    ●  Data modeling
    ●  Trade-offs involving latency, throughput, transactions
    ●  Distributed systems
    ●  Schema design

1.2 Designing data pipelines. Considerations include:

    ●  Data publishing and visualization (e.g., BigQuery)
    ●  Batch and streaming data (e.g., Dataflow, Dataproc, Apache Beam, Apache Spark and Hadoop ecosystem, Pub/Sub, Apache Kafka)
    ●  Online (interactive) vs. batch predictions
    ●  Job automation and orchestration (e.g., Cloud Composer)

1.3 Designing a data processing solution. Considerations include:

    ●  Choice of infrastructure
    ●  System availability and fault tolerance
    ●  Use of distributed systems
    ●  Capacity planning
    ●  Hybrid cloud and edge computing
    ●  Architecture options (e.g., message brokers, message queues, middleware, service-oriented architecture, serverless functions)
    ●  At least once, in-order, and exactly once, etc., event processing

1.4 Migrating data warehousing and data processing. Considerations include:

    ●  Awareness of current state and how to migrate a design to a future state
    ●  Migrating from on-premises to cloud (Data Transfer Service, Transfer Appliance, Cloud Networking)
    ●  Validating a migration

Section 2: Building and operationalizing data processing systems

2.1 Building and operationalizing storage systems. Considerations include:

    ●  Effective use of managed services (Cloud Bigtable, Cloud Spanner, Cloud SQL, BigQuery, Cloud Storage, Datastore, Memorystore)
    ●  Storage costs and performance
    ●  Life cycle management of data

2.2 Building and operationalizing pipelines. Considerations include:

    ●  Data cleansing
    ●  Batch and streaming
    ●  Transformation
    ●  Data acquisition and import
    ●  Integrating with new data sources

2.3 Building and operationalizing processing infrastructure. Considerations include:

    ●  Provisioning resources
    ●  Monitoring pipelines
    ●  Adjusting pipelines
    ●  Testing and quality control

Section 3: Operationalizing machine learning models

3.1 Leveraging pre-built ML models as a service. Considerations include:

    ●  ML APIs (e.g., Vision API, Speech API)
    ●  Customizing ML APIs (e.g., AutoML Vision, Auto ML text)
    ●  Conversational experiences (e.g., Dialogflow)

3.2 Deploying an ML pipeline. Considerations include:

    ●  Ingesting appropriate data
    ●  Retraining of machine learning models (AI Platform Prediction and Training, BigQuery ML, Kubeflow, Spark ML)
    ●  Continuous evaluation

3.3 Choosing the appropriate training and serving infrastructure. Considerations include:

    ●  Distributed vs. single machine
    ●  Use of edge compute
    ●  Hardware accelerators (e.g., GPU, TPU)

3.4 Measuring, monitoring, and troubleshooting machine learning models. Considerations include:

    ●  Machine learning terminology (e.g., features, labels, models, regression, classification, recommendation, supervised and unsupervised learning, evaluation metrics)
    ●  Impact of dependencies of machine learning models
    ●  Common sources of error (e.g., assumptions about data)

Section 4: Ensuring solution quality

4.1 Designing for security and compliance. Considerations include:

    ●  Identity and access management (e.g., Cloud IAM)
    ●  Data security (encryption, key management)
    ●  Ensuring privacy (e.g., Data Loss Prevention API)
    ●  Legal compliance (e.g., Health Insurance Portability and Accountability Act (HIPAA), Children's Online Privacy Protection Act (COPPA), FedRAMP, General Data Protection Regulation (GDPR))

4.2 Ensuring scalability and efficiency. Considerations include:

    ●  Building and running test suites
    ●  Pipeline monitoring (e.g., Cloud Monitoring)
    ●  Assessing, troubleshooting, and improving data representations and data processing infrastructure
    ●  Resizing and autoscaling resources

4.3 Ensuring reliability and fidelity. Considerations include:

    ●  Performing data preparation and quality control (e.g., Dataprep)
    ●  Verification and monitoring
    ●  Planning, executing, and stress testing data recovery (fault tolerance, rerunning failed jobs, performing retrospective re-analysis)
    ●  Choosing between ACID, idempotent, eventually consistent requirements

4.4 Ensuring flexibility and portability. Considerations include:

    ●  Mapping to current and future business requirements
    ●  Designing for data and application portability (e.g., multicloud, data residency requirements)
    ●  Data staging, cataloging, and discovery
