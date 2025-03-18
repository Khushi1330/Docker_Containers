# Backery Foundation Example for Linux OS compaitble with only python 3.9

Here’s a simple working example for a **Bakery Foundation** that creates a **Linux-based machine image** compatible with **Python 3.9**. This example uses **HashiCorp Packer**, a widely used tool for baking immutable machine images.  

---

### **Prerequisites**
- **Linux or macOS**
- **Packer installed** (`brew install packer` or [Download Packer](https://developer.hashicorp.com/packer/downloads))
- **AWS Account** (if creating an AMI)

---

### **Step 1: Define a Packer Template (bakery.json)**  
Create a **Packer template file** (`bakery.json`) to bake a machine image with **Python 3.9**.

```json
{
  "variables": {
    "aws_region": "us-east-1"
  },
  "builders": [
    {
      "type": "amazon-ebs",
      "region": "{{user `aws_region`}}",
      "source_ami": "ami-0c55b159cbfafe1f0",
      "instance_type": "t2.micro",
      "ssh_username": "ubuntu",
      "ami_name": "bakery-foundation-python39-{{timestamp}}"
    }
  ],
  "provisioners": [
    {
      "type": "shell",
      "inline": [
        "sudo apt-get update",
        "sudo apt-get install -y python3.9 python3.9-venv python3.9-dev"
      ]
    }
  ]
}
```

---

### **Step 2: Initialize and Validate the Bakery**
Run the following commands to **initialize** and **validate** the bakery template:

```sh
packer init .
packer validate bakery.json
```

---

### **Step 3: Bake the Machine Image**
Now, build the image using:

```sh
packer build bakery.json
```

This will create a **Linux-based machine image (AMI)** with **Python 3.9 pre-installed**.

---

### **Step 4: Deploy and Use the Baked Image**
Once the image is baked:
- **For AWS:** Find the created AMI in **EC2 -> AMIs** and launch an instance.
- **For Local VM (Alternative):** You can use **VirtualBox or QEMU** to bake an image for local use.

---

### **Conclusion**
This **Bakery Foundation** approach ensures that all deployments start with a **pre-configured** Linux image containing Python 3.9. This minimizes configuration drift and enhances consistency across environments.

Would you like an example for **Docker-based baking** instead? 🚀