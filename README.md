# Azure 3-Tier E-Commerce Application

## Overview

This project demonstrates the end-to-end deployment of a secure and scalable 3-tier e-commerce application on Microsoft Azure using Infrastructure as a Service (IaaS) components.

The primary objective was to design a cloud-native architecture that follows security best practices by isolating the presentation, application, and database layers into separate network segments while maintaining secure communication between tiers.

To strengthen the security posture, Azure Bastion was implemented for administrative access and Azure NAT Gateway was used to provide controlled outbound internet connectivity without exposing private workloads directly to the internet.

The solution simulates a real-world e-commerce platform where users can browse products, view pricing and images, and retrieve product information through a backend REST API connected to a MySQL database.


## Business Requirements

The application was designed to meet the following requirements:

* Deploy a fully functional e-commerce application on Azure.
* Separate frontend, backend, and database workloads into dedicated subnets.
* Restrict direct internet access to backend and database servers.
* Enable secure administrative access without exposing SSH ports publicly.
* Implement secure communication between application tiers.
* Provide controlled outbound internet connectivity for software updates and package installation.
* Demonstrate Azure networking, Linux administration, and application deployment skills.


## Solution Architecture

The application follows a traditional 3-tier architecture pattern:

Internet
    │
    ▼
Public IP
    │
    ▼
Frontend VM
(NGINX Web Server)
Frontend Subnet
10.0.1.0/24
    │
    │ REST API Calls
    ▼
Backend VM
(Python Flask API)
Backend Subnet
10.0.2.0/24
    │
    │ MySQL Queries
    ▼
Database VM
(MySQL Server)
Database Subnet
10.0.3.0/24
Administrative Access
Azure Bastion
    ├── Frontend VM
    ├── Backend VM
    └── Database VM
Outbound Connectivity
Azure NAT Gateway
    ├── Frontend VM
    ├── Backend VM
    └── Database VM

## Azure Resources Deployed

| Resource                | Purpose                                 |
| ----------------------- | --------------------------------------- |
| Resource Group          | Logical container for project resources |
| Virtual Network         | Network isolation and communication     |
| Frontend Subnet         | Hosts web layer                         |
| Backend Subnet          | Hosts application layer                 |
| Database Subnet         | Hosts data layer                        |
| Azure Bastion           | Secure VM administration                |
| NAT Gateway             | Outbound internet connectivity          |
| Network Security Groups | Traffic control and segmentation        |
| Ubuntu Virtual Machines | Application hosting                     |
| Public IP Address       | Frontend application access             |



## Technology Stack

### Frontend Layer

* Ubuntu Server
* NGINX
* HTML
* CSS
* JavaScript

Responsibilities:

* Serves the e-commerce storefront
* Displays product images and pricing
* Sends API requests to the backend
* Handles user interactions

### Backend Layer

* Ubuntu Server
* Python Flask
* REST API

Responsibilities:

* Processes application requests
* Retrieves product information
* Connects securely to the database
* Returns JSON responses

### Database Layer

* Ubuntu Server
* MySQL Server

Responsibilities:

* Stores product catalog information
* Maintains application data
* Supports backend queries



## Network Security Design

A layered security approach was implemented to minimize the attack surface and enforce workload isolation.

### Frontend Tier

Allowed Inbound Traffic:

* HTTP (TCP 80)
* HTTPS (TCP 443)

### Backend Tier

Allowed Inbound Traffic:

* TCP 5000 only from the Frontend Subnet

### Database Tier

Allowed Inbound Traffic:

* TCP 3306 only from the Backend Subnet

As a result:

* Users can only access the web tier.
* Backend services remain private.
* Database services remain completely isolated from internet traffic.

## Bastion-Based Administration

Rather than exposing SSH (Port 22) to the internet, Azure Bastion was deployed to provide secure browser-based access to all virtual machines.

Benefits:

* Eliminates public SSH exposure
* Reduces attack surface
* Centralizes administration
* Improves security compliance

## NAT Gateway Implementation

All virtual machines require outbound internet access for:

* Operating system updates
* Package installation
* Security patching
* Application dependency downloads

Azure NAT Gateway was attached to the application subnets to provide secure outbound connectivity while preventing unsolicited inbound connections.



## Application Workflow

The following sequence occurs when a user accesses the application:

1. User opens the storefront using the Frontend VM public IP.
2. NGINX serves the e-commerce web interface.
3. Product requests are sent to the Flask REST API.
4. The API queries the MySQL database.
5. Product information is returned as JSON.
6. The frontend dynamically renders product details, pricing, and images.

Client Browser
      │
      ▼
NGINX Web Server
      │
      ▼
Flask REST API
      │
      ▼
MySQL Database


## Validation and Testing

### Frontend to Backend Connectivity

curl http://<backend-private-ip>:5000/products

### Backend to Database Connectivity

mysql -h <database-private-ip> -u ecomuser -p


## Key Skills Demonstrated

### Microsoft Azure

* Azure Virtual Machines
* Azure Bastion
* Azure NAT Gateway
* Azure Virtual Network
* Azure Network Security Groups
* Public and Private Networking

### Linux Administration

* Ubuntu Server Management
* Service Configuration
* Network Troubleshooting
* Package Management

### Application Deployment

* NGINX Web Server
* Python Flask API
* MySQL Database
* REST API Integration

### Cloud Architecture

* 3-Tier Architecture Design
* Network Segmentation
* Secure Administrative Access
* Private Application Infrastructure
* Defense-in-Depth Security


## Project Outcome

Successfully designed and deployed a secure 3-tier e-commerce platform on Microsoft Azure using industry-standard architectural principles.

The implementation demonstrates practical experience in cloud networking, workload isolation, Linux administration, application deployment, and Azure security services while closely resembling the architecture patterns commonly used in enterprise production environments.

