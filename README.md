# OptiTraffic AI: Traffic Signal Prediction System

A next-generation urban traffic control platform that leverages both microservices and SOAP SOA architectures. OptiTraffic AI dynamically predicts and optimizes traffic signals using real-time simulation, machine learning, and secure, scalable service communication. The project includes a live simulation frontend, robust backend services, and a comparative implementation of RESTful microservices and SOAP XML web services.

## Table of Contents

- [Project Overview](#project-overview)
- [Why OptiTraffic AI?](#why-optitraffic-ai)
- [System Architecture](#system-architecture)
- [Key Features](#key-features)
- [Microservices vs. SOAP SOA: Major Differences](#microservices-vs-soap-soa-major-differences)
- [Simulation & WebSocket Streaming](#simulation--websocket-streaming)
- [How It Works](#how-it-works)
- [Setup & Deployment](#setup--deployment)
- [Component Summary](#component-summary)

## Project Overview

OptiTraffic AI is an intelligent traffic signal prediction and control system designed to address urban congestion using a dual-architecture approach:

- **Microservices Pattern:** Modern, containerized RESTful services with JWT authentication, Kafka messaging, and independent deployment.
- **SOAP SOA Pattern:** Enterprise-grade, WSDL-defined SOAP services with WS-Security, XML encryption, and contract-first design.

The system ingests real-time traffic data from a CityFlow simulation, processes it securely, predicts optimal signal phases using an LSTM neural network, and provides live monitoring and notifications through a web dashboard.

## Why OptiTraffic AI?

- **Urban Mobility Optimization:** Reduces congestion, fuel waste, and emissions by dynamically adapting traffic signals.
- **Enterprise-Ready Design:** Demonstrates both cloud-native microservices and legacy-compatible SOAP SOA for real-world integration.
- **Security & Scalability:** Implements end-to-end encryption, robust authentication, and scalable service orchestration.
- **Educational Value:** Offers a hands-on comparison of two major architectural paradigms in a complex, data-driven application.

## System Architecture

### Microservices Architecture

- **API Gateway:** NGINX-based, handles JWT validation, routing, and rate limiting.
- **Services:** Independent containers for login, simulation, prediction, monitoring, and notification.
- **Messaging:** Apache Kafka for asynchronous, decoupled event streaming.
- **Authentication:** JWT tokens for secure, stateless access.
- **Frontend:** React dashboard with live updates via REST and WebSocket.

### SOAP SOA Architecture

- **Service Contracts:** WSDL-defined interfaces for login, prediction, monitoring, and notification.
- **Security:** WS-Security with XML encryption and UsernameToken authentication.
- **Integration:** Centralized service bus for message routing and validation.
- **Frontend:** React dashboard (SOAP backend only; live streaming not supported).

## Key Features

- **Dual Deployment:** Both microservices and SOAP SOA implementations for all core services.
- **Real-Time Simulation:** CityFlow-based traffic simulation with live vehicle and signal data.
- **Secure Data Pipeline:** Fernet (AES-256) encryption for all Kafka messages.
- **AI Signal Prediction:** LSTM neural network predicts optimal green light phases.
- **Live Dashboard:** Frontend displays intersection state, vehicle flow, and analytics.
- **Notification System:** Automated alerts for congestion, accidents, and light failures.
- **Concurrent Authentication:** Multithreaded login service supports high concurrency.

## Microservices vs. SOAP SOA: Major Differences

| Aspect                | Microservices                                   | SOAP SOA                                         |
|-----------------------|-------------------------------------------------|--------------------------------------------------|
| **Scope**             | Application-level, fine-grained services        | Enterprise-level, coarse-grained reusable services|
| **Communication**     | REST/JSON over HTTP, WebSocket, Kafka           | SOAP/XML over HTTP, WSDL contracts, ESB          |
| **Security**          | JWT, HTTPS, Fernet encryption                   | WS-Security, XML encryption, UsernameToken        |
| **Deployment**        | Independent containers, easy scaling            | Centralized, often monolithic deployment          |
| **Data Management**   | Each service manages its own data               | Shared data storage, centralized governance       |
| **Streaming Support** | Native WebSocket for live data                  | Not supported (SOAP is request/response only)     |
| **Frontend Integration** | Real-time updates via WebSocket/REST         | No real-time streaming; polling only              |

## Simulation & WebSocket Streaming

**Live Data Streaming with WebSocket:**
- The simulation service streams live vehicle and signal data to the frontend using WebSocket, enabling real-time visualization of traffic flow and signal changes.
- **Why SOAP XML Could Not Be Used:**  
  SOAP is fundamentally a request-response protocol designed for stateless, synchronous operations. It does not support persistent, bidirectional connections required for real-time streaming. WebSocket, on the other hand, enables full-duplex communication, allowing the server to push updates to the client instantly—essential for live simulation. This architectural limitation is why WebSocket was used for the simulation service, while SOAP was applied to other backend services.

## How It Works

1. **Simulation:** CityFlow generates realistic traffic patterns and vehicle data.
2. **Secure Messaging:** Data is encrypted and published to Kafka topics.
3. **Prediction:** The LSTM model predicts the optimal green light based on recent traffic snapshots.
4. **Monitoring:** The monitoring service aggregates analytics and exposes them via REST (microservices) or SOAP (SOA).
5. **Notification:** Events like congestion or accidents trigger notifications and emails.
6. **Frontend:** The React dashboard displays live intersection state, analytics, and notifications. In microservices mode, it receives live updates via WebSocket; in SOAP mode, it polls for updates.

## Setup & Deployment

### Prerequisites

- **Docker & Docker Compose**
- **Python 3.8+**
- **Node.js (for frontend)**
- **CityFlow traffic simulator**
- **Kafka & Zookeeper (managed by Docker Compose)**

### Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yourusername/optitraffic-ai-dual-architecture.git
   cd optitraffic-ai-dual-architecture
   ```

2. **Build & Start Services**
   - For microservices:
     ```bash
     cd [MICROSERVICES]/docker
     docker-compose up --build
     ```
   - For SOAP SOA:
     ```bash
     cd [SOAP XML]/docker
     docker-compose up --build
     ```

   > **Note:** The frontend service is automatically built and started by Docker Compose using its Dockerfile. There is no need to manually run `npm install` or `npm start` in the frontend directory.

3. **Simulation**
   - The simulation service will start automatically via Docker Compose and begin streaming data.

4. **Configuration**
   - Environment variables for encryption keys, JWT, and email credentials are set in the Docker Compose files and `.env` files as needed.

## Component Summary

| Component         | Description                                                      |
|-------------------|------------------------------------------------------------------|
| **ai_model**      | LSTM model training, preprocessing, and prediction scripts       |
| **cityflow_simulation** | Traffic simulation and data generation                    |
| **login_service** | User authentication (JWT for microservices, WS-Security for SOAP)|
| **traffic_signal**| Signal prediction API (REST/SOAP)                               |
| **traffic_monitoring** | Analytics and reporting API (REST/SOAP)                    |
| **notification**  | Event notification and email alerts                             |
| **frontend**      | React dashboard for live monitoring and control                  |
| **docker**        | Docker Compose and service Dockerfiles for orchestration         |
