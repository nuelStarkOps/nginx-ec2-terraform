# EC2 NGINX Setup with Terraform

This project automates the creation of an EC2 instance on AWS, installs NGINX, and enables SSH access using a designated key pair. All configurations are managed with Terraform.

## 🚀 Features

- **EC2 Instance Creation:** Launches an Ubuntu EC2 instance.
- **NGINX Installation:** Automatically installs and starts the NGINX web server.
- **SSH Access:** Secure SSH access using the `nuel-ansible.pem` key pair.

---

## 🗂️ Project Structure

```
.
├── ec2.tf         # EC2 instance configuration
├── input.tf       # Input variables
├── main.tf        # Terraform provider configuration
├── output.tf      # Outputs for EC2 details
├── vpc.tf         # VPC, Subnet, Security Group setup
└── README.md      # Project documentation
```

---

## ⚙️ Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) installed
- AWS CLI configured with appropriate credentials
- `nuel-ansible.pem` key pair present on your local machine

---

## 🌐 Deployment Steps

1. **Clone the Repository:**
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Initialize Terraform:**
   ```bash
   terraform init
   ```

3. **Apply the Configuration:**
   ```bash
   terraform apply -auto-approve
   ```

4. **Retrieve the Public IP:**
   ```bash
   terraform output ec2_public_ip
   ```

5. **SSH into the EC2 Instance:**
   ```bash
   chmod 400 /path/to/nuel-ansible.pem
   ssh -i /path/to/nuel-ansible.pem ubuntu@<public-ip>
   ```

6. **Verify NGINX Installation:**
   Open `http://<public-ip>` in your browser. You should see the NGINX welcome page.

---

## 🔐 Security Group Configuration

- **Inbound Rules:**
  - HTTP (80): Open to all (`0.0.0.0/0`)
  - HTTPS (443): Open to all (`0.0.0.0/0`)
  - SSH (22): Open to all (`0.0.0.0/0`)

- **Outbound Rules:**
  - All traffic allowed

> **Note:** For production, restrict SSH access to specific IPs for better security.

---

## ⚡ Terraform Configuration Highlights

- **Public IP Association:**
  ```hcl
  associate_public_ip_address = true
  ```

- **Key Pair for SSH:**
  ```hcl
  key_name = "<keypair-name>" 
  ```

- **NGINX Installation via User Data:**
  ```bash
  #!/bin/bash
  sudo apt update -y
  sudo apt install nginx -y
  sudo systemctl start nginx
  sudo systemctl enable nginx
  ```

---

## 🗑️ Teardown

To destroy all resources:
```bash
terraform destroy -auto-approve
```

---

## 🙋‍♂️ Troubleshooting

- **SSH Issues:** Ensure port 22 is open in the security group.
- **Public IP Issues:** Confirm `associate_public_ip_address = true` is set.
- **Permission Denied:** Use `chmod 400` for the `.pem` file.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

