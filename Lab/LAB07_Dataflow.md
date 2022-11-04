# Lab : create vm in compute engine and retrieve lab code

git clone https://github.com/GoogleCloudPlatform/training-data-analyst

## create gcs bucket
BUCKET="qwiklabs-gcp-00-7998c3160645"
echo $BUCKET

cd ~/training-data-analyst/courses/data_analysis/lab2/python
nano grep.py

# local run 
python3 grep.py

# visualize res
ls -al /tmp
cat /tmp/output-*

# run in cloud 
gsutil cp ../javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/*.java gs://$BUCKET/javahelp
# add the bucket name created in the script
python3 grepc.py

# visualize progress in Web UI Dataflow

# see res
#gsutil cp gs://$BUCKET/javahelp/output* .
cat output*