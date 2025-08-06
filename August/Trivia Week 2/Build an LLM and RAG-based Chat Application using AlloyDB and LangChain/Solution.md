
---
# 📚 Solution References

This solution uses several Google Cloud services and open-source tools. Here are some helpful references for this lab.

-   [`gcloud compute ssh`](https://cloud.google.com/sdk/gcloud/reference/compute/ssh): Connects to virtual machines.
-   [AlloyDB for PostgreSQL Overview](https://cloud.google.com/alloydb/docs/overview): The managed database service used.
-   [Cloud Run Documentation](https://cloud.google.com/run/docs): For deploying and managing containerized applications.
-   [PostgreSQL Client (`psql`)](https://www.postgresql.org/docs/current/app-psql.html): The command-line tool for interacting with PostgreSQL.
-   [Python Virtual Environments (`venv`)](https://docs.python.org/3/library/venv.html): Isolates project dependencies.
---
   
> ### ⚠️ Important: Placeholders
>
> **English:** Throughout this lab, you will see placeholders like `PG_PASSWORD` and `CLUSTER`. You must replace these with the specific values provided in your lab environment.
>
> **Peringatan:** Di sepanjang lab ini, Anda akan melihat placeholder seperti `PG_PASSWORD` dan `CLUSTER`. Anda harus menggantinya dengan nilai spesifik yang disediakan di lingkungan lab Anda.
   
---
   
## 📝 Lab Instructions

This guide walks you through connecting to a Compute Engine VM, installing necessary software, and configuring a GenAI application to use an AlloyDB database.
   
### 1. Set Local Variables and Connect to the VM
   
First, run these commands in your local terminal or Google Cloud Shell to define environment variables and SSH into the `instance-1` virtual machine.
   
```bash
export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")
export PROJECT_ID=$(gcloud config get-value project)
gcloud config set project $PROJECT_ID
gcloud config set compute/zone "$ZONE"
gcloud compute ssh instance-1 --zone=$ZONE
```  
**Note**: The following steps should all be performed inside the SSH session on the `instance-1` VM.

### 2. Install Dependencies on the VM
   
Once connected, update the package manager and install necessary software `postgresql-client` for database interaction, and `python3-venv` and `git` for the application.

```bash
sudo apt-get update
sudo apt-get install --yes postgresql-client python3.11-venv git
```

Use the previously noted $PGASSWORD and the cluster name to connect to AlloyDB from the Compute Engine VM:

```bash
export PGPASSWORD=PG_PASSWORD
export REGION=${ZONE%-*}
export ADBCLUSTER=CLUSTER
export INSTANCE_IP=$(gcloud alloydb instances describe $ADBCLUSTER-pr --cluster=$ADBCLUSTER --region=$REGION --format="value(ipAddress)")
psql "host=$INSTANCE_IP user=postgres" -c "CREATE DATABASE assistantdemo"
psql "host=$INSTANCE_IP user=postgres dbname=assistantdemo" -c "CREATE EXTENSION vector"
```
   
### 3. Clone and Configure the Retrieval Application

With the database ready, clone the GenAI application repository, set up a Python virtual environment, and configure the application to connect to your AlloyDB instance.

```bash
git clone https://github.com/GoogleCloudPlatform/genai-databases-retrieval-app.git
cd genai-databases-retrieval-app/retrieval_service
    
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
    
cp example-config.yml config.yml
sed -i s/127.0.0.1/$INSTANCE_IP/g config.yml
sed -i s/my-password/$PGPASSWORD/g config.yml
sed -i s/my_database/assistantdemo/g config.yml
sed -i s/my-user/postgres/g config.yml
cat config.yml
pip install -r requirements.txt
python run_database_init.py
```

### 4. Deploy the Retrieval Service to Cloud Run

Open another Cloud Shell tab using the "+" at the top
```bash
export PROJECT_ID=$(gcloud config get-value project)
gcloud iam service-accounts create retrieval-identity
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:retrieval-identity@$PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/aiplatform.user"
```

### 5. Deploy the Retrieval Service
Return to the previous Cloud Shell and execute the code below:
```bash
cd ~/genai-databases-retrieval-app
gcloud alpha run deploy retrieval-service \
    --source=./retrieval_service/\
    --no-allow-unauthenticated \
    --service-account retrieval-identity \
    --region us-central1 \
    --network=default \
    --quiet
```
```bash
curl -H "Authorization: Bearer $(gcloud auth print-identity-token)" $(gcloud  run services list --filter="(retrieval-service)" --format="value(URL)")
```

### 6. Deploy sample application
Next you are going to deploy a sample application on the VM.
```bash
cd ~/genai-databases-retrieval-app/llm_demo
pip install -r requirements.txt
export BASE_URL=$(gcloud  run services list --filter="(retrieval-service)" --format="value(URL)")
```

### The next step you should following **Prepare Client ID** step.

## 🎉 Congratulations, you've finished the lab!
