# Web-App-DevOps-Project

Welcome to the Web App DevOps Project repo! This application allows you to efficiently manage and track orders for a potential business. It provides an intuitive user interface for viewing existing orders and adding new ones.

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Usage](#usage)
- [Technology Stack](#technology-stack)
- [Infrastructure Setup](#infrastructure-setup)
   - [Devops Pipeline Architecture](#devops-pipeline-architecture)
  - [Versioning and Docker Setup](#versioning-and-docker-setup)
  - [Networking Module (Terraform)](#networking-module-terraform)
  - [Provisioning AKS Cluster with Terraform](#provisioning-aks-cluster-with-terraform)
- [Deployment to AKS](#deployment-to-aks)
  - [Kubernetes Manifests](#kubernetes-manifests)
  - [Setting Up CI/CD Pipeline](#setting-up-cicd-pipeline)
- [Monitoring and Security Setup for AKS Cluster](#monitoring-and-security-setup-for-aks-cluster)
  - [Enable Container Insights](#enable-container-insights)
  - [Metrics Explorer Configuration](#metrics-explorer-configuration)
  - [Log Analytics Configuration](#log-analytics-configuration)
  - [Alerting Configuration](#alerting-configuration)
- [Securing Application Code with Azure Key Vault](#securing-application-code-with-azure-key-vault)
- [Conclusion](#conclusion)
- [Contributors](#contributors)
- [License](#license)

## Features

- **Order List:** View a comprehensive list of orders including details like date UUID, user ID, card number, store code, product code, product quantity, order date, and shipping date.
  
![Screenshot 2023-08-31 at 15 48 48](https://github.com/maya-a-iuga/Web-App-DevOps-Project/assets/104773240/3a3bae88-9224-4755-bf62-567beb7bf692)

- **Pagination:** Easily navigate through multiple pages of orders using the built-in pagination feature.
  
![Screenshot 2023-08-31 at 15 49 08](https://github.com/maya-a-iuga/Web-App-DevOps-Project/assets/104773240/d92a045d-b568-4695-b2b9-986874b4ed5a)

- **Add New Order:** Fill out a user-friendly form to add new orders to the system with necessary information.
  
![Screenshot 2023-08-31 at 15 49 26](https://github.com/maya-a-iuga/Web-App-DevOps-Project/assets/104773240/83236d79-6212-4fc3-afa3-3cee88354b1a)

- **Data Validation:** Ensure data accuracy and completeness with required fields, date restrictions, and card number validation.

## Getting Started

### Prerequisites

For the application to succesfully run, you need to install the following packages:

- flask (version 2.2.2)
- pyodbc (version 4.0.39)
- SQLAlchemy (version 2.0.21)
- werkzeug (version 2.2.3)

### Usage

To run the application, you simply need to run the `app.py` script in this repository. Once the application starts you should be able to access it locally at `http://127.0.0.1:5000`. Here you will be meet with the following two pages:

1. **Order List Page:** Navigate to the "Order List" page to view all existing orders. Use the pagination controls to navigate between pages.

2. **Add New Order Page:** Click on the "Add New Order" tab to access the order form. Complete all required fields and ensure that your entries meet the specified criteria.

## Technology Stack

- **Backend:** Flask is used to build the backend of the application, handling routing, data processing, and interactions with the database.

- **Frontend:** The user interface is designed using HTML, CSS, and JavaScript to ensure a smooth and intuitive user experience.

- **Database:** The application employs an Azure SQL Database as its database system to store order-related data.

## Infrastructure Setup

## DevOps Pipeline Architecture

![UML Diagram](![alt text](image.png))


### Versioning and Docker Setup

Used Git for version control. The repository is structured to support collaboration and feature development using branches and pull requests.

Cloning the Repository
To clone the repository onto your local machine, use the following command:

bash
Copy code
git clone https://github.com/<your-username>/Web-App-DevOps-Project.git
Creating Issues
Issues are used to track and manage tasks and features. To create a new issue, navigate to the Issues tab and click New Issue. Provide a descriptive title and details about the task or feature.

Branching and Pull Requests
Branches are used to isolate work and avoid conflicts. To create a new feature branch, use the following command:

bash
Copy code
git checkout -b feature/add-delivery-date
Make necessary code changes and push them to the remote repository. Submit a pull request to merge the changes into the main branch.

Docker
The application is containerized using Docker for consistent packaging and deployment.

Dockerfile
The Dockerfile encapsulates all dependencies and configuration settings. To build the Docker image, run the following command:

bash
Copy code
docker build -t <name of the image> .
Running Docker Container
To run the Docker container locally and access the application, use the following command:

bash
Copy code
docker run -p 5000:5000 <name of the image>
Access the application at http://127.0.0.1:5000.

Docker Image Tagging and Pushing
Tag the Docker image with relevant information and push it to Docker Hub for accessibility and deployment:

bash
Copy code
docker tag <name of the image> <docker-hub-username>/<image-name>:<tag>
docker push <docker-hub-username>/<image-name>:<tag>

### Networking Module (Terraform)

The repository includes Terraform configurations for provisioning networking services for an Azure Kubernetes Service (AKS) cluster.

Initialization
To initialize the networking module, navigate to the networking-module directory and run:

bash
Copy code
terraform init
Variables
Inside the cluster module directory, create a variables.tf configuration file. In this file, define the input variables for this module. These will allow you to customize various aspects of the AKS cluster.

Define the following input variables:

aks_cluster_name: Represents the name of the AKS cluster you wish to create
cluster_location: Specifies the Azure region where the AKS cluster will be deployed to
dns_prefix: Defines the DNS prefix of the cluster
kubernetes_version: Specifies which Kubernetes version the cluster will use
service_principal_client_id: Provides the Client ID for the service principal associated with the cluster
service_principal_secret: Supplies the Client Secret for the service principal
Additionally, add the output variables from the networking module as input variables for this module:

resource_group_name
vnet_id
control_plane_subnet_id
worker_node_subnet_id


Main Configuration
Within the cluster module's main.tf configuration file, defined the necessary Azure resources for provisioning an AKS cluster. This includes creating the AKS cluster, specifying the node pool and the service principal. Used the input variables defined in the previous task to specify the necessary arguments.

Outputs
Inside the cluster module created an outputs.tf configuration file, this defined the output variables of this module. These will capture essential information about the provisioned AKS cluster.

Defined the following output variables:

aks_cluster_name: Stores the name of the provisioned cluster
aks_cluster_id: Stores the ID of the cluster
aks_kubeconfig: Captures the Kubernetes configuration file of the cluster. This file is essential for interacting with and managing the AKS cluster using kubectl.
Usage
Initialize the cluster module to ensure it is ready to use within your main project. Make sure you are in the correct directory (aks-cluster-module) before running the initialization command.

Terraform Main Configuration
Created a main.tf configuration file in the main project directory (aks-terraform). Within this file, first defined the Azure provider block to enable authentication to Azure using your service principal credentials. Remember to define input variables for the client_id and client_secret arguments in a variables.tf file, and then created equivalent environment variables to store the values without exposing your credentials.

After provisioning the provider block, integrate the networking module in the project's main configuration file. This integration ensures that the networking resources previously defined in their respective module are included, and therefore accessible in the main project.

Provided the following input variables when calling the module:

Set resource_group_name to  "networking-resource-group"
Set location to an Azure region that is geographically close ("UK South")
Set vnet_address_space to ["10.0.0.0/16"]
Integrate the cluster module in the main project configuration file. This step connects the AKS cluster specifications to the main project, as well as allowing you to provision the cluster within the previously defined networking infrastructure.

Provided the following input variables when calling the module:

Set cluster_name to "terraform-aks-cluster"
Set location to an Azure region that is geographically close ("UK South")
Set dns_prefix to "myaks-project"
Set kubernetes_version to a Kubernetes version supported by AKS, such as "1.26.6"
Set service_principal_client_id and service_principal_secret to your service principal credentials
Use variables referencing the output variables from the networking module for the other input variables required by the cluster module such as: resource_group_name, vnet_id, control_plane_subnet_id, worker_node_subnet_id, and aks_nsg_id
Within the main project directory initialize the Terraform project. Once the project is initialized, you can apply the Terraform configuration. This will initiate the creation of the defined infrastructure, including the networking resources and AKS cluster.

Retrieved the kubeconfig file once the AKS cluster was rovisioned. This configuration file allowed you to connect to the AKS cluster securely. Connected to the newly created cluster to ensure that the provisioning process was successful and the cluster is operational.

## Kubernetes Deployment

### Kubernetes Manifests

- **Deployment Resource:** Defined a Deployment named `flask-app-deployment` to manage the containerized application.
- **Replicas:** Specified two replicas for scalability and high availability.
- **Selector and Labels:** Used the label `app: flask-app` for pod identification and management.
- **Container Configuration:** Configured the Deployment to use the container image hosted on Docker Hub.
- **Port Exposition:** Exposed port 5000 for communication within the AKS cluster.
- **Deployment Strategy:** Implemented Rolling Updates for seamless application updates.

### Service Manifest

- **Service Resource:** Defined a service named `flask-app-service` for internal communication within the AKS cluster.
- **Selector Matching:** Ensured the service matches the labels (`app: flask-app`) of the pods in the Deployment.
- **Port Configuration:** Configured the service to use TCP protocol on port 80, with targetPort set to 5000.
- **Service Type:** Set to ClusterIP for internal service within the AKS cluster.

## Deployment Process

1. **Setting Context:** Ensured the correct AKS cluster context was set for deployment.
2. **Applying Manifests:** Applied the Kubernetes manifests using `kubectl apply -f application-manifest.yaml`.
3. **Monitoring Deployment:** Monitored the deployment process for feedback and completion.
4. **Verification:** Verified the status and details of deployed pods and services using `kubectl get pods` and `kubectl get services`.

## Testing Application Locally

1. **Pods and Services Status:** Confirmed the status of pods and services within the AKS cluster.
2. **Port Forwarding:** Initiated port forwarding using `kubectl port-forward <pod-name> 5000:5000`.
3. **Local Access:** Accessed the application locally at `http://127.0.0.1:5000`.
4. **Functional Testing:** Thoroughly tested the application, particularly the orders table and Add Order functionality.




### Setting Up CI/CD Pipeline

# CI/CD Pipeline Setup Documentation

This documentation outlines the steps to set up a CI/CD pipeline for deploying a containerized web application onto a Terraform-provisioned AKS (Azure Kubernetes Service) cluster using Azure DevOps. It covers configuring the pipeline, integrating with GitHub and Docker Hub, connecting to AKS, and testing the deployment.

## Creating Azure DevOps Project

1. **Create Project:** Created a new Azure DevOps project in the Azure DevOps account associated with the Azure account.
2. **Log In:** Logged into Azure DevOps with the same email account linked to the Azure account.

## Configuring Pipeline

1. **Source Repository:** Configured the source control system as GitHub and selected the repository containing the application code.
2. **Pipeline Creation:** Created a new pipeline using the Starter Pipeline template for initial setup.

## Setting up Docker Hub Service Connection

1. **Generate Token:** Created a personal access token on Docker Hub.
2. **Service Connection:** Configured an Azure DevOps service connection using the Docker token.
3. **Verification:** Ensured the successful establishment of the connection.

## Building and Pushing Docker Image

1. **Pipeline Modification:** Added a Docker task with the buildandPush command to the pipeline.
2. **Automation:** Set up the pipeline to trigger automatically on each push to the main branch.
3. **Testing:** Ran the CI/CD pipeline and tested the Docker image functionality by pulling and running it locally.

## Establishing AKS Service Connection

1. **Creation:** Created and configured an AKS service connection within Azure DevOps.
2. **Secure Link:** Ensured a secure link between the CI/CD pipeline and the AKS cluster for seamless deployments.

## Modifying CI/CD Pipeline for Kubernetes Deployment

1. **Pipeline Configuration:** Incorporated the Deploy to Kubernetes task with the deploy kubectl command.
2. **Leveraging Manifests:** Utilized the deployment manifest available in the application repository for deployment.

## Testing and Validation

1. **Monitoring Pods:** Monitored pod status within the cluster to confirm correct creation.
2. **Secure Access:** Initiated port forwarding using kubectl for secure access to the application running on AKS.
3. **Functional Testing:** Tested application functionality to ensure correct operation and validate CI/CD pipeline effectiveness.

# Monitoring and Security Setup for AKS Cluster

This README provides instructions on setting up monitoring, logging, alerting, and security measures for an AKS (Azure Kubernetes Service) cluster.

## Enable Container Insights

1. **Enable Managed Identity:**
   - Enable managed identity on the AKS cluster for secure authentication.
2. **Set Permissions for Service Principal:**
   - Grant necessary permissions to the Service Principal for accessing Azure resources.
3. **Enable Container Insights:**
   - Enable Container Insights on the AKS cluster to start collecting performance and diagnostic data.

## Metrics Explorer Configuration

Configure and save the following charts in Metrics Explorer:

- **Average Node CPU Usage**
- **Average Pod Count**
- **Used Disk Percentage**
- **Bytes Read and Written per Second**

Access the dashboard to verify correct visualization of the metrics.

## Log Analytics Configuration

Configure Log Analytics to execute and save the following logs:

- **Average Node CPU Usage Percentage per Minute**
- **Average Node Memory Usage Percentage per Minute**
- **Pods Counts with Phase**
- **Find Warning Value in Container Logs**
- **Monitoring Kubernetes Events**

## Alerting Configuration

Set up alert rules to trigger alarms for critical conditions:

- **Used Disk Percentage Alert**
- **CPU and Memory Usage Alerts**


## Securing Application Code with Azure Key Vault

1. **Create Azure Key Vault:**
   - Create an Azure Key Vault to securely store sensitive information.
2. **Assign Key Vault Administrator Role:**
   - Assign the Key Vault Administrator role to manage secrets.
3. **Create Secrets:**
   - Create secrets in the Key Vault to secure database credentials.
4. **Enable Managed Identity for AKS:**
   - Enable managed identity for AKS to interact securely with Key Vault.
5. **Assign Key Vault Secrets Officer Role:**
   - Assign Key Vault Secrets Officer role to AKS managed identity.
6. **Integrate Azure Identity and Azure Key Vault Libraries:**
   - Integrate libraries into the Python application code for secure retrieval of database connection details.
7. **Testing and Deployment:**
   - Test the modified application locally and deploy it to the AKS cluster using Azure DevOps CI/CD pipeline.

## Conclusion
In summary, the journey from Git versioning to monitoring and logging in an AKS environment underscores the importance of seamless collaboration, automation, visibility, and security in modern DevOps practices. By leveraging Azure DevOps for CI/CD pipelines, Docker Hub for container management, and Azure Monitor for real-time insights, organizations can achieve efficient development, deployment, monitoring, and maintenance of cloud-native applications on AKS clusters. This integrated approach fosters agility, reliability, and security, enabling teams to deliver high-quality software with confidence and efficiency.



## Contributors 

- [Maya Iuga](https://github.com/maya-a-iuga)

## License

This project is licensed under the MIT License. For more details, refer to the [LICENSE](LICENSE) file.

