## Overview

This project addresses the need for automatic documentation generation for backend development projects, specifically those built with Django. The proposal focuses on creating a tool that integrates with the GitHub API, allowing users to select a specific Django project and automatically generate reference guides. These technical guides aim to provide developers with a clear and quick understanding of how a particular API functions.

### Problem Statement

This project was initiated in response to three main issues:
1. **Lack of Documentation**: Often due to time constraints, developers do not document their projects adequately.
2. **Perception of Documentation**: Documentation is frequently viewed as a non-priority task, leading to its neglect within organizations.
3. **Organizational Culture**: Many organizations rely solely on developers' tacit knowledge without promoting the creation of technical documentation. This is risky as any developer might leave the project at any time.

### Project Objectives
- **Automate documentation generation** to minimize manual intervention.
- **Reduce maintenance and update time** in software projects.
- **Improve the quality and consistency of available documentation** at every project stage.
- **Accelerate project understanding** for new developers and architects.

### Project Architecture
**Architecture Diagram**
![Architecture Diagram](https://camo.githubusercontent.com/db9a4fc9f44fae42766c2791537f9e286ffdcb71cbbffac0198701bdfc426374/68747470733a2f2f666972656261736573746f726167652e676f6f676c65617069732e636f6d2f76302f622f6669722d646174616170702d63363034332e61707073706f742e636f6d2f6f2f4449414752414d412e706e673f616c743d6d6564696126746f6b656e3d38613763653732312d663265342d346638652d393565332d363735386163333232356437)

**Technical Description**
- **Presentation** The front-end is deployed in an S3 bucket and accessed through a web browser.
- **Business Logic** Uses Lambda functions, API Gateway, and a load balancer to handle requests and process data.
- **Gemini Integration** The Gemini AI model communicates with the business logic through asynchronous Lambda functions, using queues to manage the load.
- **Storage** MongoDB is used to store the generated results, while S3 is used to store the documentation.

# Credentials to Test the Platform

Use the following credentials to access the platform and perform tests. This account already has integrated projects that you can explore to see the platform's capabilities:

- **Email:** test@test.com
- **Password:** Gato123

> **Note:** This is a test account. Please do not activate the two-factor authentication (2FA) option or change the password. If you encounter any issues with the platform, please contact `support@codeia.pro`.

### Deployment
The deployment of both the web application and the backend services is fully automated through **GitHub Actions**, ensuring a seamless and continuous integration and deployment (CI/CD) process. This automation minimizes manual intervention, reduces the risk of errors, and accelerates the deployment process.

#### 1. **Web Application Deployment**
   - **Automation**: The deployment of the web application, hosted on an S3 bucket, is triggered automatically whenever changes are pushed to the `master` branch of the GitHub repository.
   - **Process**:
     - **Build**: GitHub Actions trigger the build process, which includes installing dependencies, running tests, and packaging the application.
     - **Deploy**: The build artifacts are then uploaded to the S3 bucket. The bucket is configured to serve static files, allowing the web application to be accessed via a web browser.

#### 2. **Backend Services Deployment**
   - **Automation**: Similar to the web application, backend services are automatically deployed through GitHub Actions. The backend consists of multiple AWS Lambda functions, an API Gateway, and supporting resources.
   - **Process**:
     - **Infrastructure as Code (IaC)**: The infrastructure is defined using tools like Terraform or AWS CloudFormation, ensuring that all resources are provisioned consistently.
     - **Lambda Functions**: Code for the Lambda functions is built, tested, and then deployed. The deployment process updates the Lambda functions with the latest code, ensuring that any new logic or bug fixes are immediately available.

#### 3. **Generation Logic**
   - **Hosted in AWS Lambda**: The core logic responsible for documentation generation is encapsulated within an AWS Lambda function. This function interacts with the Gemini AI API to analyze and generate the required documentation.
   - **Triggering**: The Lambda function is invoked asynchronously via API Gateway, ensuring that the logic can scale efficiently based on the incoming requests.
   - **Monitoring and Logs**: AWS CloudWatch is used to monitor the performance of the Lambda function, track errors, and generate logs, providing visibility into the operation of the generation process.

### Continuous Deployment (CD) Pipeline

- **GitHub Actions Workflow**:
  - **Triggered by Commits**: Any commit to the `master` branch initiates the deployment pipeline.
  - **Automated Tests**: Unit tests and integration tests are executed to validate the changes before deployment.
  - **Multi-Environment Deployments**: The pipeline supports deploying to different environments (e.g., development, staging, production) based on the branch or tags used.

By automating the documentation process, this project aims to improve the understanding and maintenance of APIs, thereby facilitating better project management and developer onboarding.
