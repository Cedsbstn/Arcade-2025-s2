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

   Use this command to set required variables and securely connect to your lab instance's command line.

    ```bash
    export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")
    export PROJECT_ID=$(gcloud config get-value project)
    export VM=$(gcloud compute instances list --filter="name=workspace-vm" --format="value(name)")
    gcloud config set project $PROJECT_ID
    gcloud config set compute/zone "$ZONE"
    gcloud compute ssh $VM --project=$PROJECT_ID --zone=$ZONE
    ```

2.  **After succesfully logs into SSH, You Should Create Peering from `workspace-vpc` to `private-vpc` also from `private-vpc` back to `workspace-vpc`**

    ```bash
    gcloud compute networks peerings create peering-to-private --network=workspace-vpc --peer-network=private-vpc
    gcloud compute networks peerings create peering-to-workspace --network=private-vpc --peer-network=workspace-vpc
    ```

## 🎉 Congratulations, you've finished the lab! 🎊
