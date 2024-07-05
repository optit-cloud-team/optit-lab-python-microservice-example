# Microservice communication with RabbitMQ

# Project Documentation
Overview
This project consists of multiple microservices that interact with each other. The services include a producer and several consumers, each with a specific functionality. The project also includes configurations for Docker, Kubernetes, and CI/CD pipelines.

Python Flask Application
This project is built using Python and Flask, a lightweight web framework. Flask is used to create the main application logic and the RESTful APIs for various services. The microservices architecture allows each service to operate independently, making the system more modular and easier to maintain.

### Project directory Structure [after completion]
```
producer/
│
├── producer.py               # Main application logic
├── Dockerfile                # Dockerfile for building the producer component
└── requirements.txt          # Python dependencies for the producer

consumer_one/
│   ├── Dockerfile            # Dockerfile for building consumer_one
│   ├── healthcheck.py        # Consumer_one functionality
│   └── requirements.txt      # Python dependencies for consumer_one

consumer_two/
│   ├── Dockerfile            # Dockerfile for building consumer_two
│   ├── item_creation.py      # Consumer_two functionality
│   ├── requirements.txt      # Python dependencies for consumer_two
│   └── repository/           # Repository module for consumer_two
│       ├── __init__.py
│       ├── database.py
│       └── entity.py

consumer_three/
│   ├── Dockerfile            # Dockerfile for building consumer_three
│   ├── stock_management.py   # Consumer_three functionality
│   └── requirements.txt      # Python dependencies for consumer_three

consumer_four/
│   ├── Dockerfile            # Dockerfile for building consumer_four
│   ├── order_processing.py   # Consumer_four functionality
│   └── requirements.txt      # Python dependencies for consumer_four

dockercompose/
│   ├── docker-compose.yml    # Docker Compose file for deploying the entire application

pipeline_files/
│   ├── build.pipeline        # Jenkins pipeline for building the application
│   ├── deployment.pipeline   # Jenkins pipeline for deploying the application
│   └── shared_library/       # Shared Jenkins library for reusable functions

kubernetes/
│   ├── manifest/
│   │   ├── consumer_one/
│   │   │   ├── consumer-one-deployment.yaml
│   │   │   └── consumer-one-service.yaml
│   │   │
│   │   ├── consumer_two/
│   │   │   ├── consumer-two-deployment.yaml
│   │   │   └── consumer-two-service.yaml
│   │   │
│   │   ├── consumer_three/
│   │   │   ├── consumer-three-deployment.yaml
│   │   │   └── consumer-three-service.yaml
│   │   │
│   │   ├── consumer_four/
│   │   │   ├── consumer-four-deployment.yaml
│   │   │   └── consumer-four-service.yaml
│   │   │
│   │   ├── producer/
│   │   │   ├── producer-deployment.yaml
│   │   │   └── producer-service.yaml
│   │   │
│   │   ├── rabbitmq/
│   │   │   ├── rabbitmq-statefulset.yaml
│   │   │   └── rabbitmq-service.yaml
│   │   │
│   │   └── mysql/
│   │       ├── mysql-statefulset.yaml
│   │       └── mysql-service.yaml
repository/
├── __init__.py
├── database.py
├── entity.py
└── README.md
```


# Inventory Management System [DETAILED OVERVIEW OF APPLICATION]

## Overview

The Inventory Management System is designed to handle inventory operations using a microservice architecture with RabbitMQ as the messaging system. It consists of a producer and multiple consumers, each handling specific tasks.

## Architecture

The system is built using the following components:

- **Producer**: Sends messages to RabbitMQ queues.
- **Consumers**: Process messages from RabbitMQ queues.
- **MySQL Database**: Stores inventory data.
- **RabbitMQ**: Message broker for communication between services.

## Services

### Producer

The producer is a Flask application that exposes RESTful APIs to send messages to RabbitMQ queues.

#### API Endpoints

- **Healthcheck**: `/healthcheck`
  - Method: `GET`
  - Description: Sends a health check message to the `healthcheck` queue.
  - Response:
    ```json
    {
      "message": "Health check message sent"
    }
    ```

- **Create Item**: `/create_item`
  - Method: `POST`
  - Description: Sends an inventory item creation message to the `create_item` queue.
  - Request Body:
    ```json
    {
      "sku": "IB123456789",
      "name": "Tropicana Apple Juice",
      "label": "juice",
      "price": 25.00,
      "quantity": 50
    }
    ```
  - Response:
    ```json
    {
      "message": "Create item message sent"
    }
    ```

- **Stock Management**: `/stock_management`
  - Method: `POST`
  - Description: Sends a stock management message to the `stock_management` queue.
  - Request Body:
    ```json
    {
      "sku": "IB123456789"
    }
    ```
  - Response:
    ```json
    {
      "message": "Stock management (item delete) message sent"
    }
    ```

- **Order Processing**: `/order_processing`
  - Method: `GET`
  - Description: Sends an order processing message to the `order_processing` queue and waits for a response from the consumer.
  - Response:
    ```json
    [
      {
        "sku": "IB123456789",
        "name": "Tropicana Apple Juice",
        "label": "juice",
        "price": 25.00,
        "quantity": 50
      }
    ]
    ```

### Consumers

#### Consumer One: Healthcheck

Processes health check messages from the `healthcheck` queue.

#### Consumer Two: Item Creation

Processes item creation messages from the `create_item` queue and stores the inventory item in the MySQL database.

#### Consumer Three: Stock Management

Processes stock management messages from the `stock_management` queue and deletes the specified inventory item from the MySQL database.

#### Consumer Four: Order Processing

Processes order processing messages from the `order_processing` queue and returns the list of inventory items as a response.

### Database

The MySQL database stores the inventory data. The database schema is defined in `entity.py`.

### Repository

Contains the database connection setup and initialization.

### Docker

The services are containerized using Docker. The `docker-compose.yml` file defines the multi-container setup.

## Setup and Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Build and run the Docker containers**:
   ```bash
   docker-compose up --build
   ```

3. **Access the Producer API**:
   - Healthcheck: `http://localhost:5000/healthcheck`
   - Create Item: `http://localhost:5000/create_item`
   - Stock Management: `http://localhost:5000/stock_management`
   - Order Processing: `http://localhost:5000/order_processing`

## Directory Structure

```plaintext
.
├── consumer_one
│   ├── Dockerfile
│   ├── healthcheck.py
│   └── requirement.txt
├── consumer_two
│   ├── Dockerfile
│   ├── item_creation.py
│   ├── requirement.txt
│   └── repository
│       ├── __init__.py
│       ├── database.py
│       └── entity.py
├── consumer_three
│   ├── Dockerfile
│   ├── stock_management.py
│   ├── requirement.txt
│   └── repository
│       ├── __init__.py
│       ├── database.py
│       └── entity.py
├── consumer_four
│   ├── Dockerfile
│   ├── order_processing.py
│   ├── requirement.txt
│   └── repository
│       ├── __init__.py
│       ├── database.py
│       └── entity.py
├── docker-compose.yml
└── producer
    ├── Dockerfile
    ├── producer.py
    └── requirements.txt
```

## Dependencies

- `pika`: For RabbitMQ messaging
- `flask`: For creating RESTful APIs
- `sqlalchemy`: For ORM with MySQL
- `mysql-connector-python` and `pymysql`: For MySQL connection
- `cryptography`: For secure connections

[DEVOPS PART]

- Ensure that the RabbitMQ and MySQL services are running before starting the producer and consumers.
- The producer service waits for the RabbitMQ and consumer services to be ready before starting.
- The health check consumer logs the received messages, while the item creation, stock management, and order processing consumers interact with the MySQL database.

# DEVOPS STEPS AND FILES

