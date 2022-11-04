# SKILL BADGE 1 Create and Manage Cloud Resources

## Task 1. Create a project jumphost instance

gcloud compute instances create nucleus-jumphost-561 \
          --machine-type f1-micro  \
          --image-family debian-11  \
          --image-project debian-cloud\
          --zone us-east1-b

## Task 2. Create a Kubernetes service cluster

```bash
gcloud container clusters create nucleus-backend \
          --num-nodes 1 \
          --zone us-east1-b

gcloud container clusters get-credentials nucleus-backend \
          --zone us-east1-b

kubectl create deployment hello-server \
          --image=gcr.io/google-samples/hello-app:2.0

kubectl expose deployment hello-server \
          --type=LoadBalancer \
          --port 8083

kubectl get service

## Task 3. Set up an HTTP load balancer

```bash
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```

### Creating an instance template

```bash
gcloud config set compute/zone us-east1-b

gcloud compute instance-templates create web-server-templates \
          --metadata-from-file startup-script=startup.sh \
          --region us-east1
```

### Create a target pool 

```bash
gcloud compute target-pools create nginx-pool \
          --region us-east1
```

### Create a managed instance group

```bash
gcloud compute instance-groups managed create web-server-group \
          --base-instance-name web-server \
          --size 2 \
          --template web-server-template \
          --region us-east1
          --target-pool nginx-pool
```

### Create a firewall rule named as accept-tcp-rule-276 to allow traffic (80/tcp)

```bash
gcloud compute firewall-rules create accept-tcp-rule-276 \
          --allow tcp:80 \
```

### Create a health check

```bash
gcloud compute http-health-checks create http-basic-check
gcloud compute instance-groups managed \
          set-named-ports web-server-group \
          --named-ports http:80 \
          --region us-east1
```

### Create a backend service, and attach the managed instance group with named port (http:80)

```bash
gcloud compute backend-services create web-server-backend \
          --protocol HTTP \
          --http-health-checks http-basic-check \
          --global

gcloud compute backend-services add-backend web-server-backend \
          --instance-group web-server-group \
          --instance-group-region us-east1 \
          --global
```

### Create a URL map, and target the HTTP proxy to route requests to your URL map

```bash
gcloud compute url-maps create web-server-map \
          --default-service web-server-backend

gcloud compute target-http-proxies create http-lb-proxy \
          --url-map web-server-map
```

### Create a forwarding rule

```bash
gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \
        --ports 80

gcloud compute forwarding-rules list
```
