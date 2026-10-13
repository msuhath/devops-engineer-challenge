
# DevOps Engineer Challenge — React Application Deployment

A hands-on DevOps project demonstrating how to build, deploy, containerize, and automate a React application using AWS and common DevOps tools.

> **Source attribution:** The original React application was obtained from [Riyanuddin17's DevOps Challenge repository](https://github.com/riyanuddin17/DevOps-Challenge). The AWS deployment, Jenkins pipeline, S3 upload, and Docker configuration documented here reflect my hands-on implementation of the challenge.

## Project Overview

This project deploys a React application on an AWS EC2 Ubuntu instance. It includes a traditional deployment using PM2, an Nginx reverse proxy, a Jenkins CI/CD pipeline, automated build uploads to Amazon S3, and a Docker Compose deployment.

## Technologies Used

| Technology | Purpose |
|---|---|
| AWS EC2 | Hosts the application and DevOps tools |
| AWS IAM | Grants the EC2 instance access to S3 |
| Amazon S3 | Stores built React application files |
| Linux / Ubuntu | Server administration |
| Git and GitHub | Source control and project hosting |
| Node.js and npm | Install dependencies and build React |
| Nginx | Reverse proxy and static web server |
| PM2 | Runs the non-containerized application |
| Jenkins | Automates checkout, build, deployment, and upload |
| Docker | Packages the built application with Nginx |
| Docker Compose | Builds and runs the container |

## Architecture

1. Source code is checked out from GitHub.
2. Jenkins installs npm dependencies and builds the React application.
3. The generated `dist/` files are deployed to the EC2 host and served by PM2.
4. Nginx proxies HTTP traffic to the non-containerized application.
5. Jenkins uploads the build files to a private S3 bucket using the EC2 IAM role.
6. Docker packages the built static files into an Nginx-based image.
7. Docker Compose runs the container and maps host port `3001` to container port `3000`.

## Deployment Endpoints

Replace `<EC2-PUBLIC-IP>` with your current instance address when testing.

| Deployment | Endpoint |
|---|---|
| Nginx reverse proxy | `http://<EC2-PUBLIC-IP>` |
| PM2 deployment | `http://<EC2-PUBLIC-IP>:3000` |
| Docker deployment | `http://<EC2-PUBLIC-IP>:3001` |
| Jenkins | `http://<EC2-PUBLIC-IP>:8080` |

These endpoints require the corresponding services to be running and the necessary network access to be configured. Do not expose Jenkins publicly without appropriate access restrictions.

## Run the React Application

Install dependencies and build the application:

```bash
npm install
npm run build
```

The production build is generated in the `dist/` directory.

## Docker Deployment

Build the Docker image:

```bash
sudo docker compose build
```

Start the container:

```bash
sudo docker compose up -d
```

Check the running container:

```bash
sudo docker compose ps
```

Test the application locally on the EC2 instance:

```bash
curl -I http://127.0.0.1:3001/
```

Stop the container:

```bash
sudo docker compose down
```

## Jenkins CI/CD Pipeline

The pipeline performs the following stages:

1. Checkout source code from GitHub.
2. Check Node.js, npm, Git, and AWS CLI.
3. Install application dependencies.
4. Build the React application.
5. Prepare the deployment files.
6. Deploy the build using PM2.
7. Verify the application response.
8. Upload the build files to Amazon S3.

The EC2 instance uses an IAM role for S3 access instead of storing AWS access keys in the pipeline.

## Security Practices

- Use IAM roles and least-privilege permissions for AWS access.
- Keep S3 private unless public access is explicitly required.
- Never commit AWS credentials, passwords, private keys, or secret environment files.
- Restrict access to Jenkins and other administrative ports.
- Keep dependencies and operating-system packages updated.

## Screenshots and Evidence

Project screenshots will be added under `docs/screenshots/`, including:

- Jenkins pipeline success
- S3 build upload
- Docker image and running container
- Application response checks

## Learning Outcomes

- Linux server administration
- React production builds
- Nginx reverse proxy configuration
- PM2 application management
- Jenkins pipeline automation
- AWS IAM role-based access to S3
- Docker image creation and Docker Compose
- Git version control and GitHub documentation

## License and Attribution

The original React application belongs to its respective author. Please refer to the original repository and its license for terms governing reuse. The deployment configuration and documentation in this repository describe my implementation of the DevOps challenge.

