# Docker-project
Docker-project for jupiter website
# README for Docker-Project

## Overview

This project demonstrates how to set up and deploy a simple web application using Docker and a three-tier architecture on AWS. It involves creating a Docker image for a web application (`Jupiter-website`), running the application locally in a container, and deploying the architecture on AWS.

---

## Project Structure

- `Docker-project/`
  - `Jupiter-website/`
    - `Dockerfile`: Instructions to build the Docker image for the `Jupiter-website`.

---

## Prerequisites

- **GitHub account**: To create and manage the project repository.
- **Docker Desktop**: To build and run Docker containers.
- **AWS Account**: To deploy the application using a three-tier architecture.
- **AWS CLI**: For managing AWS resources from the command line.

---

## Steps to Follow

### 1. Create and Set Up the Repository
1. Create a repository on GitHub named `Docker-project`.
2. Clone the repository and create the folder structure as mentioned above.

### 2. Write and Build the Dockerfile
1. Navigate to the `Jupiter-website` folder.
2. Create a `Dockerfile` and include commands to:
   - Use the Amazon Linux base image.
   - Install dependencies (`httpd`, `wget`, `unzip`).
   - Download and set up the application files.
   - Expose port `80`.
3. Build and run the Docker image:
   ```bash
   docker build -t jupiter .
   docker run -dp 80:80 jupiter
   ```

### 3. Test the Application Locally
- Open your browser and go to `http://localhost:80` to verify that the application is running.

### 4. Deploy on AWS (Three-Tier Architecture)
1. Create an IAM user with `AdministratorAccess` and generate an access key.
2. Configure the AWS CLI:
   ```bash
   aws configure
   ```
3. Set up the architecture with the following layers:
   - **Frontend (Web Application)**: Hosted in a Docker container.
   - **Backend (Application Layer)**: Managed using AWS services.
   - **Database Layer**: Connect to a managed database instance.

---

## Key Commands

### Git Commands
```bash
git add .
git commit -m "Initial commit"
git push
```

### Docker Commands
```bash
docker build -t jupiter .
docker run -dp 80:80 jupiter
```

### AWS CLI Commands
```bash
aws configure
```

---

## Notes
- Ensure that all required dependencies are installed on your local machine.
- Secure your AWS access keys to prevent unauthorized access.

---

## License
This project is licensed under the MIT License.
