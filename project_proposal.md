# Inventory Management

## Motivation
Effective inventory management is essential for organizations that rely on physical goods such as retail stores, laboratories, warehouses, and educational departments. Many small and medium-sized organizations still manage their stock manually using spreadsheets or outdated desktop software. These methods are prone to human error, lack real-time synchronization, and provide limited visibility into stock changes across users or locations.

In settings where supplies move quickly, like lab consumables, classroom materials, or small retail items, delayed updates or duplicate entries can cause shortages, oversupply, and loss of accountability. Traditional inventory systems are often expensive, complex, or lack modern cloud capabilities, making them unsuitable for teams that need collaboration and accessibility from multiple locations.

The Inventory Management System (IMS) addresses these problems by providing a cloud-native, real-time, and collaborative platform for tracking inventory levels and stock movements. It includes secure, role-based access so that managers and staff can work efficiently without data conflicts. Managers can monitor stock levels, review trends, and receive automatic alerts for low-stock items, while staff can record and update inventory in real time.

This project is worth pursuing because it demonstrates how cloud computing principles can be applied to solve a practical and common business problem. The system uses containerization, orchestration, persistent storage, and edge deployment to ensure reliability, scalability, and performance. By deploying the application on Fly.io, the system benefits from edge regions that deliver low-latency access for distributed teams and faster response times.

The target users will be mostly small to medium business owners/staff in industries that will require frequent tracking of their physical inventory/stock such as labs, retail stores, etc.

The proposed solution will provide a cost-effective, lightweight, and scalable tool for organizations that need an efficient way to manage their inventory. It also aligns closely with the course’s learning objectives by showcasing the use of Docker, Kubernetes, PostgreSQL, monitoring, and serverless functions in building a stateful cloud application.

## Objectives and Key Features
* The main objective of this project is to design, implement, and deploy a stateful, cloud-native inventory management system that allows users to manage stock efficiently in real time. 
* The system will ensure data consistency, security, and availability across distributed environments. 
* It will use modern cloud technologies to achieve scalability, persistence, and resilience.
* The project focuses on achieving the following specific goals:
    * Develop a user-friendly web application for inventory management with role-based access control for managers and staff.
    * Implement a robust backend service to handle inventory operations and ensure data persistence using PostgreSQL with volumes.
    * Deploy the application to a cloud provider with full containerization and orchestration support.
    * Integrate monitoring and automated alerts to ensure system stability and performance.
    * Demonstrate the use of at least two advanced cloud features such as real-time updates and serverless automation.


### Core Features
#### Containerization and Local Development
* The application will be fully containerized using **Docker** to ensure portability and consistent execution across environments.
* A **Docker Compose** file will define and manage the multi-container setup, which includes the Python backend API and PostgreSQL database. This enables easy local testing and faster iteration during development.
#### State Management
* **PostgreSQL** will be used as the relational database for storing inventory data, user information, and transaction history.
* **Fly.io Volumes** will provide persistent storage so that the application state is maintained even if the containers are restarted or redeployed.
#### Deployment Provider
* The system will be deployed on **Fly.io**, which provides global edge hosting. This platform allows the application to be replicated across regions for lower latency and better user experience. 
* Fly.io also supports integrated metrics, logs, and persistent storage for stateful workloads.
#### Orchestration Approach
* The project will use **Kubernetes** for orchestration. 
* The system will include Deployments, Services, and PersistentVolume definitions to manage application components and ensure high availability. 
* Kubernetes will handle load balancing, scaling, and automated restarts in case of failures.
#### Monitoring and Observability
* The team will use Fly.io’s built-in monitoring tools (**fly.io metric** and **grafana dashboard**) to track key metrics such as CPU usage, memory, and disk utilization. 
* Alerts will be configured for abnormal resource consumption or downtime. 
* Logs from application containers will be aggregated for easy debugging and performance analysis.

### Planned Advanced Features
#### Real-time Stock Updates
* **WebSockets** will enable real-time communication between the backend and frontend. 
* When a user updates stock information, all connected clients will immediately see the changes without refreshing the page.
#### External integration using sendgrid
* The application will use **sendgrid email service** to send notifications. This is the easiest and most reliable way of sending notifications instead of using the inbuilt SMTP method. 
* Sendgrid will handle DKIM/SPF authentication, Spam filtering, High deliverability, Scaling and rate limits. 
* The Fly.io function just calls sendgrid API with any SMTP setup or DNS headaches.
#### Serverless Integration
* **Fly.io Functions** will be used to send automated email alerts when item quantities fall below a specified threshold. This ensures that managers are promptly informed about low inventory levels. 
* This function will act like serverless since a separate lightweight python or node.js application will be configured to listen to the REST api call and simply send the email notification. 
* The app will be configured so that it automatically scales to 0 when not in use there by acting serverless.
#### CI/CD pipelines
The project will implement a continuous integration and continuous deployment (CI/CD) pipeline using **GitHub Actions**. Each commit or pull request will trigger automated workflows that:
* Run unit tests to ensure code quality before merging.
* Build Docker images for the backend and frontend services.
* Automatically deploy the latest stable build to Fly.io.
* This ensures fast iteration, consistent deployments, and reduced risk of manual deployment errors.

### Database Schema
This project will have three tables - users, items & transactions.

***users***
| Column        | Data Type    | Constraints                         | Description                                |
| ------------- | ------------ | ----------------------------------- | ------------------------------------------ |
| id            | SERIAL       | PRIMARY KEY                         | Unique identifier for each user.           |
| name          | VARCHAR(100) | NOT NULL                            | Full name of the user.                     |
| email         | VARCHAR(255) | UNIQUE, NOT NULL                    | Email address (used for login).            |
| password_hash | TEXT         | NOT NULL                            | Hashed password for secure authentication. |
| role          | VARCHAR(20)  | CHECK (role IN ('manager','staff')) | Defines user’s access level.               |

***items***
| Column        | Data Type    | Constraints               | Description                             |
| ------------- | ------------ | ------------------------- | --------------------------------------- |
| id            | SERIAL       | PRIMARY KEY               | Unique identifier for each item.        |
| name          | VARCHAR(100) | NOT NULL                  | Name of the inventory item.             |
| category      | VARCHAR(50)  |                           | Item category or type.                  |
| quantity      | INTEGER      | DEFAULT 0                 | Current quantity in stock.              |
| min_threshold | INTEGER      | DEFAULT 0                 | Minimum level before triggering alerts. |
| location      | VARCHAR(100) |                           | Storage or shelf location.              |
| updated_at    | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP | Last update timestamp for this item.    |

***transactions***
| Column        | Data Type | Constraints               | Description                                      |
| ------------- | --------- | ------------------------- | ------------------------------------------------ |
| id            | SERIAL    | PRIMARY KEY               | Unique identifier for each transaction.          |
| item_id       | INTEGER   | FOREIGN KEY → items(id)   | The item being adjusted.                         |
| change_amount | INTEGER   | NOT NULL                  | Quantity added (positive) or removed (negative). |
| user_id       | INTEGER   | FOREIGN KEY → users(id)   | The user who made the transaction.               |
| timestamp     | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | When the transaction occurred.                   |
| note          | TEXT      |                           | Optional comment or reason for the change.       |


### Scope and Feasibility
* The scope of the project is well-defined and achievable within the given course timeline. 
* The project includes all mandatory components such as Docker, PostgreSQL, Kubernetes orchestration, persistent storage, and monitoring. 
* The planned advanced features are feasible for a three-member team and can be implemented incrementally after the core functionality is complete.
* By combining practical use cases with the required cloud technologies, this project will effectively demonstrate an understanding of cloud-native design principles while delivering a useful and realistic application.

## Tentative Plan

### Member 1 - Mahta Miandashti
* Design and create the PostgreSQL database schema (users, items, transactions).
* Integrate the database with Fly.io persistent volumes for reliable data storage.
* Implement RESTful API endpoints for all inventory CRUD operations.
* Develop authentication and role-based access control (Manager, Staff).
* Write unit tests for core backend logic (CRUD, authentication, transactions).
* Implement business logic for handling stock adjustments and low-stock detection.
* Support integration with the real-time and notification components developed by others.
* If time permits, using fly.io deploy the application in multiple locations for global edge setup.

### Member 2 - Tharun Seshachalam
* Implement real-time inventory updates using WebSockets (Socket.IO or FastAPI WebSocket).
* Build notification triggers that send low-stock alerts to the serverless function.
* Develop the serverless function (Python/Node.js) for automated email notifications.
* Integrate SendGrid API for sending external email alerts securely.
* Help design backend APIs related to transaction logging and alert triggers.
* Contribute to testing real-time event flow between backend and frontend.
* If time permits, assist with Kubernetes setup for service scaling.
* If time permits, implement PostgreSQL automated backups.

### Member 3 - Zhao Ji Wang
* Write Dockerfiles for backend, frontend, and PostgreSQL containers.
* Create and maintain docker-compose.yml for local multi-container development.
* Set up Kubernetes configurations (Deployments, Services, PersistentVolumes, Ingress).
* Deploy the full system to Fly.io with persistent storage and global edge setup.
* Configure Fly.io health monitoring (CPU, memory, disk) and automated alerts.
* Implement GitHub Actions pipelines for CI/CD — automated testing, build, and deployment.
* If time permits, secure configuration using Fly.io Secrets Manager and HTTPS enforcement.

### Shared / Collaborative Tasks
All the team memebers are expected to follow and help with the following tasks:
* Follow Gitflow branching strategy for code commits and use pull requests with peer review for merging.
* Ensure CI/CD pipelines are triggered for all major feature branches.
* Integrates all components (database, backend APIs, WebSocket service, and serverless function) into a unified cloud environment.
* Frontend setup using React or similar framework (UI for login, registration, data display, inventory level dashboard and CRUD interaction).
* Testing and integration across modules before deployment.
* Documentation of architecture, cloud setup, and monitoring configuration.
* Conduct a final system integration test and help prepare the demo/presentation showcasing all key and advanced features.