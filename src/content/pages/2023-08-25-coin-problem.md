---
title: "Coin Problem"
date: 2023-08-25
slug: "coin-problem"
type: page
---

# **Coin Problem**

## **Welcome to the Coin Problem Database Page**

The Frobenius Coin Problem, a fascinating challenge in number theory and combinatorics, has puzzled mathematicians for over 150 years. It examines the largest monetary amount that cannot be obtained using specific denominations of coins—a concept known as the Frobenius number. Our research focuses on Frobenius symmetry, which explores the distribution of attainable and unattainable values between 0 and the Frobenius number. For sets of two denominations, this symmetry is always present, while for sets of three, it's possible to predict when symmetry will occur. However, for sets of four, no general solution exists yet. Our database includes data on roughly 60 million sets, specifically those where all four denominations are less than or equal to 200, to identify whether each set is symmetric or not. This extensive data provides valuable insights into the structural properties of this intriguing problem.

## **About This Resource**

This page offers a collection of resources in a ZIP archive (`resources.zip`), which includes an SQLite3 database (`frob.db`) and a Python script (`query.py`). These resources are designed for those interested in exploring the computational aspects of the Frobenius coin problem. The database interfaces seamlessly with Python, providing an efficient means for data queries.

## **Streamlined Access via Docker Container**

For those unfamiliar with database setup or seeking a more streamlined experience, we offer access to the database via a Docker container hosted on the [Google Cloud Console](https://console.cloud.google.com/). This setup allows direct interaction with the database through a command-line interface in the Cloud Console, bypassing the need for local downloads and minimizing site traffic usage.

## **How to Access the Database**

To access the database in the Docker container, use the following command in the Google Cloud Console’s Cloud Shell:

```
docker run -it --rm dglass710/frobenius-quad-database
```

![](/images/Screenshot-2024-01-24-at-4.00.09 PM.png)

The Cloud Shell Button is located on the top right of the screen

## **Understanding the Command**

- `docker run`: Initiates the running of a Docker container.
- `-it`: This flag ensures the container runs in interactive mode with a terminal, allowing command-line interactions.
- `--rm`: Automatically removes the container upon exit, ensuring no residual data is left on your system.
- `dglass710/frobenius-quad-database`: Specifies the Docker image containing the Frobenius coin problem database.

This command launches an interactive session where you can directly engage with our database without additional setups. It’s designed to be user-friendly, even for those who may not be familiar with Docker or cloud services.

## Google Cloud Console's Cloud Shell

The [Google Cloud Console’s](https://console.cloud.google.com/) Cloud Shell provides an integrated, browser-based command-line environment for managing Google Cloud Platform resources. This versatile tool comes pre-installed with essential tools and libraries, such as Docker, allowing you to execute containerized applications directly within the cloud. Cloud Shell facilitates immediate interaction with your projects without the need for any local setup or lengthy configurations. It features persistent storage and pre-authenticated access to your resources, enabling seamless transitions between planning, coding, and deployment tasks.

## Docker on Google Cloud Console's Cloud Shell

Docker is a powerful platform used for developing, shipping, and running applications inside lightweight, portable containers. These containers package an application with all its dependencies, ensuring that it runs consistently across any computing environment. In the context of [Google Cloud Console's](https://console.cloud.google.com/) Cloud Shell, Docker is pre-installed, which simplifies the process of deploying applications like the Frobenius Coin Problem database. By utilizing Docker containers, you can ensure that your application will run the same way in Cloud Shell as it would in any other environment, providing a robust and reliable tool for managing application deployments.

## **Explore and Contribute**

We encourage users to explore the database and contribute to the expanding research in this fascinating area of mathematics. Your insights and discoveries are valuable as we collectively push the boundaries of what is known about the Frobenius coin problem.
