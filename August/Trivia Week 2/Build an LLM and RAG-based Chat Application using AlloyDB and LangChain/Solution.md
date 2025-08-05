---

# 📚 Solution References

The following links point to the official `gcloud` command documentation used in this solution.

- [`gcloud compute ssh`](https://cloud.google.com/sdk/gcloud/reference/compute/ssh)
- [`gcloud compute networks peerings create`](https://cloud.google.com/sdk/gcloud/reference/compute/networks/peerings/create)

---

> ### ⚠️ Important Notice
>
> **English:** Only for educational purposes.
>
> **Peringatan:** Hanya untuk tujuan edukasi.

---

## 📝 Lab Instructions

Before running the commands, you need to set the variables and ssh into the VM


1. **Define Variables & SSH into the Virtual Machine** 

   Use this command to set required variables, securely connect to your lab instance's command line and install the `PostgreSQL client` software on the deployed VM.

    ```bash
    export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")
    export PROJECT_ID=$(gcloud config get-value project)
    gcloud config set project $PROJECT_ID
    gcloud config set compute/zone "$ZONE"
    gcloud compute ssh instance-1 --zone=$ZONE
    ```

    ```bash
    sudo apt-get update
    sudo apt-get install --yes postgresql-client
    ```
    
2.  **After succesfully logs into SSH, You Should Install the `PostgreSQL client` software on the deployed VM**

    ```bash
    sudo apt-get update
    sudo apt-get install --yes postgresql-client
    ```
    Use the previously noted $PGASSWORD and the cluster name to connect to AlloyDB from the Compute Engine VM:
    ```bash
    export PGPASSWORD=PG_PASSWORD
    ```
    ```bash
    export REGION=${ZONE%-*}
    export ADBCLUSTER=CLUSTER
    export INSTANCE_IP=$(gcloud alloydb instances describe $ADBCLUSTER-pr --cluster=$ADBCLUSTER --region=$REGION --format="value(ipAddress)")
    psql "host=$INSTANCE_IP user=postgres sslmode=require" 
    ```
## 🎉 Congratulations, you've finished the lab! 🎊
