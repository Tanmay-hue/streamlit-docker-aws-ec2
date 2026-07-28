# Deployment Guide

This guide explains how to deploy the **Streamlit Machine Learning Application** on an **Amazon EC2 instance** using **Docker**.

---

# Table of Contents

- Prerequisites
- AWS Infrastructure Setup
- Launch EC2 Instance
- Connect to EC2
- Install Docker
- Transfer Project Files
- Build Docker Image
- Run Docker Container
- Access the Application
- Verify Deployment
- Stop and Remove Container
- Troubleshooting
- Useful Commands

---

# Prerequisites

Before starting, ensure you have:

- AWS Account
- Amazon EC2 Instance (Amazon Linux 2023)
- SSH Key Pair (.pem)
- Docker Desktop (optional for local testing)
- Git
- Python 3.9+
- Streamlit Application Source Code

---

# AWS Infrastructure Setup

Create the following AWS resources:

- Custom VPC
- Public Subnet
- Internet Gateway
- Route Table
- Security Group
- EC2 Instance

The Security Group should allow:

| Protocol | Port | Purpose |
|----------|------|---------|
| SSH | 22 | Remote Login |
| HTTP | 80 | Optional |
| Custom TCP | 8501 | Streamlit Application |

---

# Launch EC2 Instance

Launch an EC2 instance with:

- Amazon Linux 2023 AMI
- t3.micro Instance Type
- Existing Key Pair
- Public IPv4 Enabled
- Security Group allowing ports 22 and 8501

Verify the instance status is:

```text
Running
```

---

# Connect to EC2

Connect using SSH.

```bash
ssh -i my-ec2-key.pem ec2-user@<EC2-PUBLIC-IP>
```

Example

```bash
ssh -i my-ec2-key.pem ec2-user@13.200.xxx.xxx
```

---

# Update the System

```bash
sudo yum update -y
```

---

# Install Docker

Install Docker.

```bash
sudo yum install -y docker
```

Enable Docker.

```bash
sudo systemctl enable docker
```

Start Docker.

```bash
sudo systemctl start docker
```

Verify installation.

```bash
docker --version
```

Expected Output

```text
Docker version xx.x.x
```

---

# Transfer Project Files

Copy project files from your local machine to the EC2 instance.

```bash
scp -i my-ec2-key.pem app.py Dockerfile requirements.txt mushrooms.csv ec2-user@<EC2-PUBLIC-IP>:/home/ec2-user/
```

Verify files.

```bash
ls
```

Expected Output

```text
app.py
Dockerfile
requirements.txt
mushrooms.csv
```

---

# Build Docker Image

Navigate to the project directory.

```bash
cd /home/ec2-user
```

Build the Docker image.

```bash
sudo docker build -t streamlit-app .
```

Verify the image.

```bash
sudo docker images
```

Expected Output

```text
REPOSITORY       TAG       IMAGE ID
streamlit-app    latest    xxxxxxxxx
```

---

# Run Docker Container

Run the container.

```bash
sudo docker run -d \
-p 8501:8501 \
--name streamlit-container \
streamlit-app
```

Verify the container.

```bash
sudo docker ps
```

Expected Output

```text
CONTAINER ID
IMAGE
STATUS
PORTS
streamlit-app
```

---

# Access the Application

Open your browser.

```text
http://<EC2-PUBLIC-IP>:8501
```

Example

```text
http://13.200.xxx.xxx:8501
```

The Streamlit application should now be accessible.

---

# Verify Deployment

Check running containers.

```bash
sudo docker ps
```

Check container logs.

```bash
sudo docker logs streamlit-container
```

Inspect the container.

```bash
sudo docker inspect streamlit-container
```

---

# Stop the Container

```bash
sudo docker stop streamlit-container
```

---

# Start the Container

```bash
sudo docker start streamlit-container
```

---

# Restart the Container

```bash
sudo docker restart streamlit-container
```

---

# Remove the Container

```bash
sudo docker rm -f streamlit-container
```

---

# Remove the Docker Image

```bash
sudo docker rmi streamlit-app
```

---

# Troubleshooting

## EC2 Connection Refused

Verify that:

- Security Group allows port **22**
- EC2 instance is running
- Correct `.pem` file is used

---

## Streamlit Not Accessible

Verify:

```bash
sudo docker ps
```

Ensure port **8501** is exposed.

Check Security Group:

- Custom TCP
- Port 8501
- Source: 0.0.0.0/0 (for testing)

---

## Docker Service Not Running

Start Docker.

```bash
sudo systemctl start docker
```

Check status.

```bash
sudo systemctl status docker
```

---

## Docker Build Failed

Verify that the following files exist.

```bash
ls
```

Expected

```text
Dockerfile
app.py
requirements.txt
mushrooms.csv
```

---

## Container Exited Immediately

Check logs.

```bash
sudo docker logs streamlit-container
```

---

# Useful Docker Commands

List images.

```bash
sudo docker images
```

List containers.

```bash
sudo docker ps
```

List all containers.

```bash
sudo docker ps -a
```

Stop container.

```bash
sudo docker stop streamlit-container
```

Restart container.

```bash
sudo docker restart streamlit-container
```

Remove container.

```bash
sudo docker rm streamlit-container
```

Remove image.

```bash
sudo docker rmi streamlit-app
```

---

# Useful EC2 Commands

Current directory.

```bash
pwd
```

List files.

```bash
ls -l
```

Check memory.

```bash
free -h
```

Check disk usage.

```bash
df -h
```

Check running processes.

```bash
top
```

Exit SSH.

```bash
exit
```

---

# Deployment Summary

The deployment process followed these steps:

1. Created AWS networking resources (VPC, Subnet, Internet Gateway, Route Table).
2. Launched an Amazon EC2 instance.
3. Connected to the instance using SSH.
4. Installed Docker on Amazon Linux.
5. Transferred the Streamlit application files.
6. Built a Docker image.
7. Started the Docker container.
8. Exposed the application on port **8501**.
9. Accessed the application through the EC2 public IP.

---

**Project:** Deploying a Streamlit App in Docker on AWS EC2

**Platform:** Amazon Web Services (AWS)

**Operating System:** Amazon Linux 2023

**Container Runtime:** Docker
