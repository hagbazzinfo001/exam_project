# Automated Website Deployment on AWS EC2

**Author:** Agbabiaka Owolabi  
**Role:** Cloud & DevOps Engineer | Frontend Developer  
**Date:** Sept 2025  

---

## Project Overview

This project demonstrates **automated deployment of a web application on AWS EC2 instances** using **Ansible**, **NGINX**, and an **Application Load Balancer (ALB)**.  
It dynamically displays EC2 instance metadata (IP address, hostname, availability zone) on a hosted web page using **IMDSv2**.  
Additionally, the same HTML page is deployed as a **static website on S3** to compare compute-based hosting vs storage-based hosting.

**Key Objectives:**

- Automate server setup and web deployment with Ansible.  
- Install and configure NGINX to serve a dynamic HTML page.  
- Configure ALB for load balancing across multiple EC2 instances.  
- Deploy and compare static hosting on AWS S3.  
- Gain hands-on experience with AWS, automation, and infrastructure as code.

---

## Architecture
          ┌─────────────────────────────┐
          │  Application Load Balancer  │
          │  (Distributes HTTP Traffic) │
          └─────────────┬──────────────┘
                        │
           ┌────────────┴─────────────┐
           │                          │
 ┌───────────────────┐      ┌───────────────────┐
 │  EC2 Instance 1   │      │  EC2 Instance 2   │
 │  Ubuntu + NGINX   │      │  Ubuntu + NGINX   │
 │  Dynamic HTML Page│      │  Dynamic HTML Page│
 │  (IP, Hostname,   │      │  (IP, Hostname,   │
 │   Availability AZ)│      │   Availability AZ)│
 └───────────────────┘      └───────────────────┘
                        │
                        │
              ┌───────────────────┐
              │       Users       │
              │  (Web Browser)    │
              └───────────────────┘
---

## Prerequisites

- **AWS Account** (Free Tier recommended)  
- **Ansible** installed on your local machine  
- **Python 3** and **pip**  
- **Git**  
- AWS CLI configured (`aws configure`) with proper IAM credentials  

---

## Setup Guide

### 1. Launch EC2 Instances

1. Navigate to AWS EC2 → Launch Instances.  
2. Select **Ubuntu 22.04 LTS** (Free Tier).  
3. Instance type: **t3.micro**.  
4. Configure security group to allow:
   - HTTP (port 80)  
   - SSH (port 22)  
5. Launch **2 EC2 instances** in the same VPC, different availability zones.  

---

### 2. Clone Repository

```bash
git clone https://github.com/hagbazzinfo001/Website-Deployment-using-Ansible-NGINX-S3-andALB
cd EC2-Web-Automation
```
### 3. Configure Ansible Inventory 
Create hosts.ini:
```bash
[webservers]
ec2-1-public-dns ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/your-key.pem
ec2-2-public-dns ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/your-key.pem
````
### 4. Run Ansible Playbook
Create hosts.ini:
```bash
ansible-playbook -i hosts.ini playbook.yaml
````
### 5. Tasks performed by the playbook:
  - Installs NGINX and updates the system.
  - Starts NGINX and enables it on boot.
  - Fetches EC2 instance metadata (IP, hostname, availability zone).
  - Deploys dynamic HTML page with the metadata.
  - Restarts NGINX to apply changes.

### 6. Application Load Balancer (ALB) Setup
  1. Navigate to EC2 → Load Balancers → Create Load Balancer → Application Load Balancer.
  2. Select Internet-facing with HTTP listener.
  3. Attach security groups allowing HTTP traffic.
  4. Register the 2 EC2 instances.
  5. Access the website via ALB DNS name (do not use EC2 public IPs).

### 7. Deploy to S3 (Static Website)
  1. Create a new S3 bucket (public access enabled).
  2. Upload index.html file.
  3. Enable Static Website Hosting under bucket properties.
  4. Access the website using the S3 endpoint URL.

| Feature         | EC2 + ALB        | S3 Static Website |
| --------------- | ---------------- | ----------------- |
| Hosting Type    | Compute-based    | Storage-based     |
| Auto Scaling    | Possible via ASG | N/A               |
| Cost            | Higher           | Lower             |
| Dynamic Content | Yes              | No                |

### 8. Technologies & Tools
  1. AWS: EC2, S3, ALB, VPC, Security Groups
  2. Automation: Ansible, Jinja2 Templates
  3. Web Server: NGINX
  4. Frontend: HTML, CSS
  5. Version Control: Git
