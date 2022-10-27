# LAB : Apache Spark on Cloud Dataproc

Migrer des tâches Spark existantes vers Cloud Dataproc
Modifier des tâches Spark afin qu'elles utilisent Cloud Storage au lieu de HDFS
Optimiser des tâches Spark pour leur exécution sur des clusters spécifiques à la tâche

active account list

```bash
gcloud auth list
```

Project list

```bash
gcloud config list project
gcloud projects list | grep PROJECT_NUMBER
```

List all Users and Service accounts in a project with their IAM roles and check that a editor role is linked to a compute develop service accounts

```bash
export PROJECT_ID=$(gcloud info --format='value(config.project)')
gcloud projects get-iam-policy $PROJECT_ID
gcloud projects get-iam-policy $PROJECT_ID | grep compute@developer.gserviceaccount.com -A 2 -B 2 # show 2 rows after and before matching reg pattern
```

Create a cluster instance on dataproc

```bash
gcloud dataproc clusters create sparktodp --region=us-central1 --enable-component-gateway --optional-components=JUPYTER --image-version=2.0
```

Locate GCS bucket used by default by dataproc

```bash
export DP_STORAGE="gs://$(gcloud dataproc clusters describe sparktodp --region=us-central1 --format=json | jq -r '.config.configBucket')"
```

clone repo and copy notebooks to GCS where we can use the Jupyter web interface to launch those notebooks

```bash
git -C ~ clone https://github.com/GoogleCloudPlatform/training-data-analyst
gsutil -m cp ~/training-data-analyst/quests/sparktobq/*.ipynb $DP_STORAGE/notebooks/jupyter
#-m : multi-threading and multi-processing
```

Create a bucket

```bash
export PROJECT_ID=$(gcloud info --format='value(config.project)')
gsutil mb gs://$PROJECT_ID
```

Copy source data in that bucket

```bash
wget https://archive.ics.uci.edu/ml/machine-learning-databases/kddcup99-mld/kddcup.data_10_percent.gz
gsutil cp kddcup.data_10_percent.gz gs://$PROJECT_ID/
```

Adapt current scripts by decoupling storage and compute + create scripts from notebook spark_analysis.py
...

Retrieve script to execute it as a job

```bash
gsutil cp gs://$PROJECT_ID/sparktodp/spark_analysis.py spark_analysis.py
```

Create script

```bash
nano submit_onejob.sh
```

by putting the following code

```bash
#!/bin/bash
gcloud dataproc jobs submit pyspark \
       --cluster sparktodp \
       --region us-central1 \
       spark_analysis.py \
       -- --bucket=$1
```

make script executable

```bash
chmod +x submit_onejob.sh
```

lauch Pyspark job

```bash
./submit_onejob.sh $PROJECT_ID
```