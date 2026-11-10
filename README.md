
# Production-Style Cloud DevOps Platform

An automated CI/CD platform that builds, publishes, and deploys a containerized Node.js application to AWS EC2 using Jenkins, Docker, GitHub, and Terraform.

## Project Overview

This project demonstrates an end-to-end DevOps workflow, from source code management to automated application deployment.

The application is containerized using Docker and deployed to an AWS EC2 instance through a Jenkins CI/CD pipeline.

## Technology Stack

- AWS EC2 - Cloud infrastructure
- Terraform - Infrastructure as Code
- Jenkins - CI/CD automation
- GitHub - Source code management
- Docker - Application containerization
- Docker Hub - Container image registry
- Node.js - Application runtime
- Linux Ubuntu - Server environment
- Bash - Automation and scripting

## Architecture

Developer pushes code to GitHub.

GitHub triggers Jenkins through a webhook.

Jenkins performs the following operations:

1. Validates application files.
2. Builds the Docker image.
3. Tags the image with the build number and latest.
4. Pushes images to Docker Hub.
5. Pulls the versioned image.
6. Deploys the application to AWS EC2.
7. Performs an HTTP health check.
8. Cleans unused Docker images.

## CI/CD Pipeline Stages

| Stage       |           Description         |
|-------------|-------------------------------|
| Test        | Validates application files   |
| Build       | Builds Docker images          |
| Push        | Publishes images to Docker Hub|
| Deploy      | Runs the application container|
| Health Check| Verifies HTTP availability    |
| Cleanup     | Removes unused Docker images  |

## Application Deployment

The application runs inside a Docker container.

- Container name: devops-web
- Container port: 3000
- EC2 exposed port: 80
- Restart policy: unless-stopped

## Docker Image Versioning

Docker images are tagged using Jenkins build numbers.

Example:

bashry/dockerapp:46
bashry/dockerapp:latest

## Security Practices

- Jenkins credentials are used for Docker Hub authentication.
- Docker Hub access tokens are stored in Jenkins Credentials.
- Sensitive environment files are excluded from Git.
- AWS Security Groups control inbound traffic.

## Key DevOps Concepts Demonstrated

- Infrastructure as Code
- Continuous Integration
- Continuous Deployment
- Docker Containerization
- Automated Build and Deployment
- Container Image Versioning
- Application Health Checks
- Cloud Infrastructure Management

## Future Improvements

- Automated rollback on deployment failure
- Unit and integration testing
- Monitoring and logging
- HTTPS configuration with SSL
- Container health monitoring
- Improved deployment strategy

## Author

Mohamed Bashry

GitHub: https://github.com/Mohamedbashry
