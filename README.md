
# 🛍️ Implementation of an E-Commerce System on AWS in an automated way using Terraform and Ansible

![PORTFOLIO PROJECTS_AWS - MODULE 7_ARCHITECTURE](https://github.com/user-attachments/assets/312f7c4a-db6a-42ce-b174-1c91fc6006e6)


---
Pre-requisites

Before proceeding with the deployment steps, ensure the following:

GitHub Repository Access

Clone or download the  project repository containing the required assets and image files.

Public S3 Bucket Setup

Upload the necessary e-commerce images (logo, product images, etc.) from the repository to your own public S3 bucket.


## **Part 1: Deploying the E-commerce MVP with Terraform**

### 1. Create a Free Magento Account
- Go to: [https://marketplace.magento.com/](https://marketplace.magento.com/)
- Generate and **save your Public and Private Keys**:
  - **Your User** → **My Profile** → **Marketplace** → **Access Key** → **Magento 2**

  ```
  Example:
  Public Key: b2041f02509880fbbb96312b29995cb6
  Private Key: 7be7c69e79bd798a2a8a6b00988a5b72
  ```

### 2. Install Terraform on AWS CloudShell

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform
```

### 3. Download Terraform Files in AWS CloudShell

```bash
mkdir final_project
cd final_project
wget https://yourS3bucketname.s3.amazonaws.com/bootcamp-aws/en/final-project-terraform.zip
unzip final-project-terraform.zip
cd terraform
```

### 4. Edit Terraform Configuration

```bash
vi main.tf
```

Update the following values:

- `vpc_id = "your-default-vpc-id"`
- `key_name = "ssh-aws-bootcamp"`
- `instance_type = "t3a.large"`

Save and exit (`Esc` → `:x`)

### 5. Deploy EC2 Instance with Terraform

```bash
terraform init
terraform plan
terraform apply
```

A file named `terraform.tfstate` will be created — it stores the current state of your infrastructure. Do **not** modify it manually.

---

## **Part 2: Installing Ansible and Magento**

### 1. SSH into EC2

```bash
ssh -i ssh-aws-bootcamp.pem ec2-user@<EC2_PUBLIC_IP>
```

### 2. Install Ansible

```bash
sudo yum-config-manager --enable epel
sudo yum install ansible -y
```

### 3. Download and Prepare Ansible Playbooks

```bash
wget https://yourS3bucketname.s3.amazonaws.com/bootcamp-aws/en/final-project-ansible-magento2.zip
unzip final-project-ansible-magento2.zip
cd ansible-magento2
```

### 4. Edit Ansible Variables

```bash
vi group_vars/all.yml
```

Update the following:

```yaml
magento_domain: <EC2_PUBLIC_IP>
server_hostname: <EC2_PUBLIC_IP>
repo_api_key: <Your Public Key>
repo_secret_key: <Your Private Key>
```

Save and exit (`Esc` → `:x`)

### 5. Run Ansible Playbook

```bash
ansible-playbook -i hosts.yml ansible-magento2.yml -k -vvv --become
```

---

## **Part 3: Configuring Magento Store**

- Access your store at: `http://<EC2_PUBLIC_IP>/securelocation`
- Login:
  - **Username:** `Admin`
  - **Password:** `Strong123Password#`

### 1. Personalize the Store

🖼️ **Download the ecommerce images from your GitHub repository and upload them to your own public S3 bucket. Use those files for customization.**

### 2. Update Store Details

- **Content > Configuration > Default Store View > Edit**
  - `HTML Head > Default page title`: **Your company Store**
  - `Header > Logo image`: Upload your company logo
  - `Header > Welcome text`: **Welcome to The "your company" Store!**

✅ Save Configuration

If prompted to refresh the cache:

- Go to **Cache Management**
- Select all with status `INVALIDATED`
- Click **Flush Magento Cache**

### 3. Add Sample Product

- **Catalog > Products > Add Product**
  - Name: `Your company T-Shirt`
  - Price: `80`
  - Quantity: `100`
  - Add your product image

✅ Save

### 4. Update Homepage Content

- **Content > Pages > Home Page > Edit**
  - Clear existing content
  - Click `Insert Widget` > Choose `Catalog Products List`
  - ✅ Insert > ✅ Save

### 5. Preview Store

- Click **Admin** > **Customer View** to see the updated store.

---

## ✅ E-commerce Deployment Complete!

---

## **Cleanup**

### 1. Destroy Resources

```bash
cd ~/final_project/terraform
terraform destroy
```

---

## 🔗 Resources

- [Ansible Documentation](https://docs.ansible.com/ansible-core/devel/user_guide/index.html)

---

