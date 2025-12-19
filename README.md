# 3-tier-project-with-aws
calable and Highly Available 3-Tier Architecture on AWS Cloud
# AWS 3-Tier Web Application Architecture

A production-style implementation of a **secure, scalable, and highly available AWS 3-Tier Web Application Architecture**. This project follows AWS best practices by separating the application into **Web, Application, and Database layers** and deploying them across multiple Availability Zones.

---

## 🏗️ Architecture Overview

The architecture is divided into three logical tiers:

* **Web Layer (Public Subnets)** – Handles client requests and serves frontend content
* **Application Layer (Private Subnets)** – Processes business logic using a Flask backend
* **Database Layer (Private Subnets)** – Stores application data using Amazon RDS (MySQL)

All components are deployed inside a **custom VPC** with proper network isolation, security controls, and high availability.

---

## 🚀 Project Implementation Steps

### 1️⃣ Create a VPC

* Created a custom VPC with DNS hostnames and DNS resolution enabled
* Used private CIDR blocks for secure internal communication

---

### 2️⃣ Create Subnets (6 Total)

* **2 Public Subnets** for Web Servers (Multi-AZ)
* **2 Private Subnets** for Application Servers (Multi-AZ)
* **2 Private Subnets** for Database Servers (Multi-AZ)

---

### 3️⃣ Configure Route Tables

* **Public Route Table** connected to an Internet Gateway and associated with public subnets
* **Private Route Tables** created per AZ and mapped to NAT Gateways for outbound access
* **Database Route Table** without internet access (optional NAT for patching)

---

### 4️⃣ Configure Security Groups (5 Total)

* **ALB-Web-SG**: Allows HTTP (80) and HTTPS (443) from the internet
* **WebServer-SG**: Allows traffic only from ALB-Web-SG and restricted SSH access
* **ALB-App-SG**: Allows port 5000 from WebServer-SG
* **AppServer-SG**: Allows application traffic from ALB-App-SG and SSH from WebServer
* **DB-SG**: Allows MySQL (3306) access only from AppServer-SG

---

### 5️⃣ Create Route 53 Hosted Zone

* Created a public hosted zone for the domain
* Updated Name Server records with the domain provider

---

### 6️⃣ Validate ACM Certificate

* Requested SSL/TLS certificate using AWS Certificate Manager
* Completed DNS validation using Route 53 CNAME records

---

### 7️⃣ Create Amazon RDS (MySQL)

* Created a DB subnet group using private DB subnets
* Launched RDS MySQL with public access disabled
* Attached DB-SG for secure access

---

### 8️⃣ Launch Web Server EC2

* Launched EC2 instances in public subnets
* Installed and configured Apache (httpd)
* Hosted frontend files (HTML, CSS, JavaScript)

---

### 9️⃣ Launch Application Server EC2

* Launched EC2 instances in private subnets
* Installed Python, Flask, and required dependencies
* Deployed Flask backend application

---

### 🔟 Access Application Server Securely

```bash
chmod 400 AWS3TierProject.pem
ssh -i AWS3TierProject.pem ec2-user@<private-ip>
```

---

### 1️⃣1️⃣ Database Setup

* Installed MySQL client on application server
* Connected to RDS endpoint
* Executed SQL scripts to create database, tables, and sample data

---

### 1️⃣2️⃣ Backend Application Setup

```bash
sudo yum install python3 python3-pip -y
pip3 install flask flask-mysql-connector flask-cors
nohup python3 app.py > output.log 2>&1 &
```

---

### 1️⃣3️⃣ Web Server Setup

```bash
sudo yum install httpd -y
sudo service httpd start
```

Configured frontend files to communicate with backend APIs.

---

### 1️⃣4️⃣ Create Application Load Balancers

* **Backend ALB**: Routes traffic to application servers (port 5000, health check `/login`)
* **Frontend ALB**: Routes user traffic to web servers (port 80/443)

---

### 1️⃣5️⃣ Configure Route 53

* Created an Alias A-record pointing the domain to the frontend ALB

---

### 1️⃣6️⃣ Attach ACM Certificate

* Attached ACM certificate to the frontend ALB
* Enabled HTTPS for secure client communication

---

## 🛠️ Tech Stack

* **Cloud**: AWS
* **Compute**: EC2, Auto Scaling
* **Networking**: VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway
* **Load Balancing**: Application Load Balancer
* **Database**: Amazon RDS (MySQL)
* **Security**: IAM, Security Groups, ACM
* **DNS**: Route 53
* **Monitoring**: CloudWatch
* **Web Server**: Apache (httpd)
* **Backend**: Python, Flask
* **OS**: Amazon Linux

---

## ✅ Final Outcome

This project delivers a **production-ready AWS 3-Tier Architecture** with strong emphasis on **security, scalability, high availability, and best practices**, making it suitable for real-world cloud and DevOps environments.

---

## 📌 Notes

* This project is implemented for learning and demonstration purposes
* Architecture and security follow AWS recommended best practices
