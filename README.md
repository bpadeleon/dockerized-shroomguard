## Project Description 

This project features a ready-to-deploy deep learning model designed to identify the readiness of oyster mushrooms. The model’s training is guided by both local and international standards, specifically the **USDA 81 FR 51297** and **PNS/BAFS 195:2017**

## Deployment 

To run this application into your own server, simply clone the repository, run the **docker build** command, and start the container using **docker run**. The application can be accessed through your local IP (or EC2 IP) on port 5000. Do note that inbound rules must be configured to enable other users to access the container application.

## Project Demonstration
https://github.com/user-attachments/assets/426b3242-bcfb-40bf-a253-2c11edf96d17

## CI/CD Pipeline
<p align="center">
  <img width="755" height="203" src="https://github.com/user-attachments/assets/ecbc7e09-5b99-45ac-a1b9-ef83d6afd50c" alt="CI/CD Pipeline" />
</p>

Whenever code is committed to the repository, a Docker image is automatically built. Once the build is complete, the image is pushed to the Elastic Container Registry, making it ready for deployment.
