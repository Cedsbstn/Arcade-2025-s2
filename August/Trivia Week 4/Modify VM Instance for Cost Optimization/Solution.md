---

# 📚 Solution References

The following links point to the official `gcloud` command documentation used in this solution.

- [`gcloud compute instances stop`](https://cloud.google.com/sdk/gcloud/reference/compute/instances/stop)
- [`gcloud compute instances set-machine-type`](https://cloud.google.com/sdk/gcloud/reference/compute/instances/set-machine-type)
- [`gcloud compute instances start`](https://cloud.google.com/sdk/gcloud/reference/compute/instances/start)

---

> ### ⚠️ Important Notice
>
> **English:** Do not change the `instance-name` or `zone` if your assigned instance is already named `lab-vm` and is located in the `us-central1-b` zone.
>
> **Peringatan:** Jangan ubah `instance-name` atau `zone` jika nama instance yang diberikan kepada Anda sudah `lab-vm` dan berada di zona `us-central1-b`.

---

## 📝 Lab Instructions

Before running the commands, you must replace `lab-vm` with the **Instance Name** provided in your lab's connection details panel. You may also need to replace the `us-central1-b` zone if a different one is specified.

1.  **Stop the Virtual Machine**
    
    This command safely shuts down the instance, which is required before changing its properties.
    ```bash
    gcloud compute instances stop lab-vm --zone=us-central1-b
    ```

2.  **Set the New Machine Type**
    
    This command resizes the stopped instance to the `e2-medium` machine type.
    ```bash
    gcloud compute instances set-machine-type lab-vm --zone=us-central1-b --machine-type=e2-medium
    ```

3.  **Start the Virtual Machine**
    
    Finally, this command starts the instance again with its new configuration.
    ```bash
    gcloud compute instances start lab-vm --zone=us-central1-b
    ```

## 🎉 Congratulations, you've finished the lab! 🎊
