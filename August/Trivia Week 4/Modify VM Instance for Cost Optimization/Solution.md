---

# 📚 Solution References

The following links point to the official `gcloud` command documentation used in this solution.

- [`gcloud compute instances stop`](https://cloud.google.com/sdk/gcloud/reference/compute/instances/stop)
- [`gcloud compute instances set-machine-type`](https://cloud.google.com/sdk/gcloud/reference/compute/instances/set-machine-type)
- [`gcloud compute instances start`](https://cloud.google.com/sdk/gcloud/reference/compute/instances/start)

---

> ### ⚠️ Important Notice
>
> **English:** Only for educational purposes.
>
> **Peringatan:** Hanya untuk tujuan edukasi.

---

## 📝 Lab Instructions

Before running the commands, you need to set the variables


1. **Define Variables** 

    Use this command to set required variables
    ```bash
    export VM_NAME=$(gcloud compute instances list --filter="name=lab-vm" --format="value(name)")
    export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")
    gcloud config set compute/zone "$ZONE"
    ```
   
2.  **Stop the Virtual Machine, Set the New Machine Type, and Start the Virtual Machine**
    
    This command safely shuts down the instance, resizes the stopped instance to the `e2-medium` machine type, and starts the instance again with its new configuration.
    ```bash
    gcloud compute instances stop lab-vm --zone=$ZONE
    gcloud compute instances set-machine-type lab-vm --zone=$ZONE --machine-type=e2-medium
    gcloud compute instances start lab-vm --zone=$ZONE
    ```

## 🎉 Congratulations, you've finished the lab! 🎊
