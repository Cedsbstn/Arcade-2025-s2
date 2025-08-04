---

# 📚 Solution References

The following links point to the official `gcloud` command documentation used in this solution.

- [`gcloud compute ssh`](https://cloud.google.com/sdk/gcloud/reference/compute/ssh)
- [`gcloud compute networks peerings create`](https://cloud.google.com/sdk/gcloud/reference/compute/networks/peerings/create)

---

> ### ⚠️ Important Notice
>
> **English:** Ensure you use the exact names for the VPC networks, instance, project, and zone as provided by the lab environment. Typos are a common source of errors.
>
> **Peringatan:** Pastikan Anda menggunakan nama yang tepat untuk jaringan VPC, instance, proyek, dan zona sesuai dengan yang disediakan di lingkungan lab. Kesalahan pengetikan adalah sumber galat yang umum.

---

## 📝 Lab Instructions

Before running the commands, replace the following placeholders with the actual values provided in your lab's details panel:
- `INSTANCE_NAME`: The name of your virtual machine.
- `PROJECT_ID`: Your Google Cloud project ID.
- `INSTANCE_ZONE`: The zone where your instance is located.
- `workspace-vpc` & `private-vpc`: The names of the VPC networks you need to peer.

1.  **SSH into the Virtual Machine**

    Use this command to securely connect to your lab instance's command line.

    ```bash
    gcloud compute ssh INSTANCE_NAME --project=PROJECT_ID --zone=INSTANCE_ZONE
    ```

2.  **Create Peering from `workspace-vpc` to `private-vpc`**

    This command creates a one-way peering connection from `workspace-vpc` to `private-vpc`. Note that peering is not yet bidirectional.

    ```bash
    gcloud compute networks peerings create peering-to-private --network=workspace-vpc --peer-network=private-vpc
    ```

3.  **Create the Return Peering Connection**

    Finally, this command establishes the peering connection from `private-vpc` back to `workspace-vpc`, allowing for bidirectional communication between the networks.

    ```bash
    gcloud compute networks peerings create peering-to-workspace --network=private-vpc --peer-network=workspace-vpc
    ```

---

## 🎉 Congratulations, you've finished the lab! 🎊

---
