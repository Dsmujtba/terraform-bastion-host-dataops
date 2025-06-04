# Implementing DataOps with Terraform: Building a Secure and Automated Data Environment on AWS

---

This repository contains the Terraform configuration I developed to deploy a bastion host architecture on AWS, ensuring secure access to an RDS database housed within a private subnet. This project was an opportunity for me to apply DataOps principles and Infrastructure as Code (IaC) best practices using Terraform, focusing on automation, security (Principle of Least Privilege), and collaborative infrastructure state management.

## My Role & Key Achievements:

As the engineer for this project, I was responsible for the end-to-end design and implementation of the infrastructure using Terraform. My key achievements and the skills I applied include:

* **Infrastructure as Code (IaC) Expertise with Terraform:**
    * Authored **modular Terraform configurations** to define and provision all AWS resources (VPC networking components, EC2 bastion host, RDS PostgreSQL instance, Security Groups).
    * Developed a reusable **Terraform module (`bastion_host`)** encapsulating the bastion and RDS setup for better organization and maintainability.
* **Secure Cloud Architecture Design:**
    * Implemented a **bastion host (jump box) architecture** to provide secure, controlled SSH access to an RDS database instance located in a private subnet, thereby minimizing its direct exposure.
    * Configured **AWS Security Groups** with granular ingress/egress rules to enforce the Principle of Least Privilege, allowing traffic only from necessary sources (e.g., bastion to RDS, public internet to bastion on specific ports).
* **Robust Terraform State Management:**
    * Configured a **Terraform S3 backend with DynamoDB for state locking**, enabling secure, centralized state file storage and preventing conflicts in collaborative or automated environments.
* **Automation of Data Environments:**
    * Demonstrated how Terraform can fully automate the creation, modification, and destruction of a common data access pattern, significantly reducing manual effort and potential for human error.
* **DataOps Principles in Practice:**
    * **Automation:** Leveraged IaC for repeatable and reliable environment provisioning.
    * **Version Control:** Managed infrastructure configurations in Git, enabling tracking of changes and collaboration.
    * **Collaboration:** Utilized a remote backend for shared state, a cornerstone for team-based IaC development.
* **AWS Cloud Services Proficiency:**
    * Gained hands-on experience with AWS EC2, RDS (PostgreSQL), VPC, Subnets (public/private), Security Groups, S3, and DynamoDB.

## Architecture Deployed:

I designed and implemented the following architecture:

* An **AWS RDS PostgreSQL database instance** provisioned within a **private subnet** of a pre-existing VPC, ensuring it is not directly accessible from the public internet.
* An **EC2 instance acting as a bastion host** (jump server) deployed in a **public subnet** within the same VPC. This host is configured to accept SSH connections from authorized IP addresses.
* **Security Groups** meticulously configured to:
    * Allow SSH traffic to the bastion host from the internet (ideally restricted to specific IPs).
    * Allow database traffic (e.g., PostgreSQL port 5432) from the bastion host's security group to the RDS instance's security group.
    * Deny all other unnecessary inbound/outbound traffic.

![Architecture Diagram](./images/bastion_host.drawio.png)

## Key Terraform Features Implemented:

* **Modular Design:** The core infrastructure (bastion EC2, RDS, networking rules) is encapsulated within the `modules/bastion_host` directory.
* **Parameterized Configuration:** Utilized `variables.tf` to define input variables (VPC ID, subnet IDs, region, database name, etc.), allowing for flexible deployments. Default and example values are managed via `terraform.tfvars` (kept out of version control for sensitive data).
* **Exposed Outputs:** Defined `outputs.tf` to conveniently display critical information post-deployment, such as the RDS endpoint and the bastion host's public DNS/IP.
* **Remote State Backend:** Implemented in `backend.tf` using AWS S3 for persistent state storage and DynamoDB for state locking, crucial for reliability and teamwork.

## Technologies Used :

* **Infrastructure as Code:** Terraform
* **Cloud Platform:** Amazon Web Services (AWS)
    * **Compute:** EC2 (for bastion host)
    * **Database:** RDS for PostgreSQL
    * **Networking:** VPC, Public/Private Subnets, Security Groups
    * **Storage & State Management:** S3, DynamoDB
* **Configuration Language:** HCL (HashiCorp Configuration Language)
* **Operating System (Bastion - Example):** Amazon Linux 2 (or as configured)

## Setup & Deployment:


1.  **Prerequisites:**
    * An AWS account with necessary permissions.
    * Terraform installed locally.
    * AWS CLI installed and configured.
    * A pre-existing VPC with public and private subnets across different Availability Zones.
2.  **Clone & Initialize:**
    ```bash
    git clone <your-repository-url>
    cd terraform-bastion-host-dataops 
    terraform init
    ```
3.  **Configure Variables:**
    * Create a `terraform.tfvars` file (from the provided example structure).
    * **Crucially, update it with your specific VPC, subnet IDs, region, etc.**
    * **DO NOT commit `terraform.tfvars` if it contains sensitive information.** Add it to `.gitignore`.
4.  **Plan & Apply:**
    ```bash
    terraform plan  # Review the execution plan
    terraform apply # Deploy the infrastructure (confirm with 'yes')
    ```
5.  **Accessing Outputs:** After a successful `apply`, Terraform will display defined outputs like the database endpoint and bastion host DNS.
6.  **Destroy (When Done):**
    ```bash
    terraform destroy # Tear down all deployed resources
    ```

## Value & Relevance to Data Engineering:

This project is more than just deploying VMs and databases; it's about establishing the **secure and automated foundation** upon which data pipelines and analytical systems are built. As a Data Engineer, understanding how to:

* Provision data infrastructure reliably and repeatedly.
* Implement security best practices for data access.
* Manage infrastructure changes through code and version control.
* Collaborate effectively on infrastructure projects using shared state.

...are critical skills. This project provided me with direct experience in these areas, aligning with modern DataOps methodologies.

---
