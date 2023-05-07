---
title: "Setup and Install Jenkins on GCP VM"
datePublished: Sun May 07 2023 16:05:48 GMT+0000 (Coordinated Universal Time)
cuid: clhdlvigh000309l8d2zvg60g
slug: setup-and-install-jenkins-on-gcp-vm
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1683474802057/6777b6c9-a33f-4e48-8fd3-ea4fc89cb84a.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1683475474512/56e8fd52-2ed7-4ab2-b4ea-a616d97f7205.webp
tags: aws, devops, jenkins, gcp, ci-cd

---

## Configure Default VPC Firewall rules

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683475436020/4ab1edcc-f5af-4cfd-8e0d-261cc9dfbfbc.png align="center")

## Create a GCP VM

![](https://miro.medium.com/v2/resize:fit:700/1*ukrI9Xbv5-rznIRZpqly7w.png align="left")

On the VM instances creation page, Make sure you select the CentoOS Image and check the checkbox to allow HTTP traffic under the firewall section as below.

![](https://miro.medium.com/v2/resize:fit:700/1*np1qJB2Yq7x8lrUJkrxuHQ.png align="left")

In the advanced section

* Add this script in the startup script of the VM
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683475063972/8fe6c80d-4c4d-4870-b9b7-ccb3142aaa42.png align="center")
    
* ```bash
    sudo apt update
    sudo apt install openjdk-11-jre -y
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
      /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
      https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt-get install jenkins -y
    ```
    

---

## Using gcloud command

```bash
gcloud compute instances create jenkins-server-template-1 --project=$GCP_PROJECT_ID --zone=us-central1-a --machine-type=e2-medium --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default --metadata=startup-script=sudo\ apt\ update$'\n'sudo\ apt\ install\ git\ -y$'\n'sudo\ apt\ install\ openjdk-11-jre\ -y$'\n'curl\ -fsSL\ https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key\ \|\ sudo\ tee\ \\$'\n'\ \ /usr/share/keyrings/jenkins-keyring.asc\ \>\ /dev/null$'\n'echo\ deb\ \[signed-by=/usr/share/keyrings/jenkins-keyring.asc\]\ \\$'\n'\ \ https://pkg.jenkins.io/debian-stable\ binary/\ \|\ sudo\ tee\ \\$'\n'\ \ /etc/apt/sources.list.d/jenkins.list\ \>\ /dev/null$'\n'sudo\ apt-get\ update$'\n'sudo\ apt-get\ install\ jenkins\ -y --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=1008566890267-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=jenkins,http-server,https-server --create-disk=auto-delete=yes,boot=yes,device-name=instance-template-1,image=projects/debian-cloud/global/images/debian-11-bullseye-v20230411,mode=rw,size=10,type=projects/$GCP_PROJECT_ID/zones/us-central1-a/diskTypes/pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --labels=ec-src=vm_add-gcloud --reservation-affinity=any
```

---

## Using Terraform

```json
# This code is compatible with Terraform 4.25.0 and versions that are backward compatible to 4.25.0.
# For information about validating this Terraform code, see https://developer.hashicorp.com/terraform/tutorials/gcp-get-started/google-cloud-platform-build#format-and-validate-the-configuration

resource "google_compute_instance" "jenkins-server-1" {
  boot_disk {
    auto_delete = true
    device_name = "instance-template-1"

    initialize_params {
      image = "projects/debian-cloud/global/images/debian-11-bullseye-v20230411"
      size  = 10
      type  = "pd-balanced"
    }

    mode = "READ_WRITE"
  }

  can_ip_forward      = false
  deletion_protection = false
  enable_display      = false

  labels = {
    ec-src = "vm_add-tf"
  }

  machine_type = "e2-medium"

  metadata = {
    startup-script = "sudo apt update\nsudo apt install git -y\nsudo apt install openjdk-11-jre -y\ncurl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \\\n  /usr/share/keyrings/jenkins-keyring.asc > /dev/null\necho deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \\\n  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \\\n  /etc/apt/sources.list.d/jenkins.list > /dev/null\nsudo apt-get update\nsudo apt-get install jenkins -y"
  }

  name = "jenkins-server-1"

  network_interface {
    access_config {
      network_tier = "PREMIUM"
    }

    subnetwork = "projects/${var.project_id}/regions/us-central1/subnetworks/default"
  }

  scheduling {
    automatic_restart   = true
    on_host_maintenance = "MIGRATE"
    preemptible         = false
    provisioning_model  = "STANDARD"
  }

  service_account {
    email  = "1008566890267-compute@developer.gserviceaccount.com"
    scopes = ["https://www.googleapis.com/auth/devstorage.read_only", "https://www.googleapis.com/auth/logging.write", "https://www.googleapis.com/auth/monitoring.write", "https://www.googleapis.com/auth/service.management.readonly", "https://www.googleapis.com/auth/servicecontrol", "https://www.googleapis.com/auth/trace.append"]
  }

  shielded_instance_config {
    enable_integrity_monitoring = true
    enable_secure_boot          = false
    enable_vtpm                 = true
  }

  tags = ["http-server", "https-server", "jenkins"]
  zone = "us-central1-a"
}
```

---