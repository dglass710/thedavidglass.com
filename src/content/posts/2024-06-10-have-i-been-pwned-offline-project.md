---
title: "Have I Been Pwned? Offline Project"
date: 2024-06-10
slug: "have-i-been-pwned-offline-project"
type: post
categories: ["projects"]
---

## **Project Overview:**

For my final project in the Northwestern Cybersecurity Bootcamp, I developed an offline version of the popular "Have I Been Pwned?" service. This tool allows users to check if their passwords have been compromised without needing an internet connection.

## **Project Components:**

### **Docker Integration**

- **Purpose:** Containerize the application for portability and ease of deployment.
- **Details:** The Dockerfile defines the environment, copying all necessary project files and setting up dependencies. This makes the tool easy to run on any system with Docker installed.

### **Database Management**

- **Purpose:** Efficiently manage and query a large dataset of compromised passwords.
- **Details:** An SQLite database stores SHA-1 hashes of compromised passwords. A Python script (`txt_to_db.py`) processes the raw data into a structured database with 4096 tables.

### **Automated Updates**

- **Purpose:** Keep the database current with the latest compromised passwords and ensure the base image includes recent security patches.
- **Details:** The `Update` script automates downloading the latest data, building the database, and creating a new Docker image to ensure users always have up-to-date information. This process includes:
  - **Downloading Latest Data:** Fetches the most recent list of compromised passwords from Have I Been Pwned.
  - **Building Database:** Converts the raw data into an SQLite database, organized into 4096 tables for efficient querying.
  - **Creating Docker Image:** Builds a new Docker image using the latest version of the `bitnami/minideb` base image, which is regularly updated with security patches. This ensures the application environment is secure.
  - **Pushing to Docker Hub:** The updated Docker image is pushed to Docker Hub for easy deployment.

### **User Interaction**

- **Purpose:** Provide a simple interface for users to check if their passwords have been compromised.
- **Details:** Users interact with the tool via a command-line interface within the Docker container. They can input passwords and receive immediate feedback on whether their passwords are compromised.

## **Security and Efficiency**

This project emphasizes security at every step. By using Docker containers, the application runs in an isolated environment, reducing the risk of interference with the host system. The main security measure stems from the user's ability to enter their passwords in a container that is not connected to the internet and can be deleted after use. This provides a secure method for checking passwords without risking exposure to online threats. The tradeoff of downloading the entire database is weighed against the security risk of entering passwords on yet another website, which might pose a greater risk. The automated update process ensures that the data remains current without manual intervention.

## ****Outcome****

The "Have I Been Pwned? Offline" project demonstrates my ability to integrate various cybersecurity practices and tools into a cohesive solution. It highlights my proficiency in scripting, database management, and Docker containerization. This project not only provides a valuable tool for checking compromised passwords but also showcases my skills in developing and maintaining secure, scalable applications.

## ****Conclusion****

Completing this project was a rewarding experience, reinforcing the importance of automation, security, and efficiency in cybersecurity solutions. For more information on how to use the tool or to explore the documentation and code, please visit my [GitHub repository](https://github.com/dglass710/pwned).
