# Lab Dataproc

## Create cluster

gcloud config set dataproc/region us-east1
gcloud dataproc clusters create example-cluster --worker-boot-disk-size 500

## Submit a job

gcloud dataproc jobs submit spark --cluster example-cluster \
  --class org.apache.spark.examples.SparkPi \ 
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar \
  -- 1000

### --class containing the main method for the job's 
### --jars location of the jar file containing your job's code
### parameters you want to pass to the job—in this case, the number of tasks, which is 1000

## Update a cluster

gcloud dataproc clusters update example-cluster --num-workers 4
gcloud dataproc clusters update example-cluster --num-workers 2
