# Lab Video Intelligence

## Set up authorization

gcloud iam service-accounts create quickstart
gcloud iam service-accounts keys create key.json --iam-account quickstart@qwiklabs-gcp-01-1edf261b2fd3.iam.gserviceaccount.com
### authenticate your service account, passing the location of your service account key file:
gcloud auth activate-service-account --key-file key.json
### Obtain an authorization token using your service account:
gcloud auth print-access-token

## Make an annotate video request

### create a JSON request file
cat > request.json <<EOF
{
   "inputUri":"gs://spls/gsp154/video/train.mp4",
   "features": [
       "LABEL_DETECTION"
   ]
}
EOF

### make a videos:annotate request passing the filename of the entity request

curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/videos:annotate' \
    -d @request.json

returns 
```
{
  "name": "projects/885501676302/locations/europe-west1/operations/12917982148912986247"
}
```

### request information on the operation by calling the v1.operations endpoint

curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/projects/885501676302/locations/europe-west1/operations/12917982148912986247'
