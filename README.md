# Microservice communication with RabbitMQ

# Project Documentation

Overview
This project consists of multiple microservices that interact with each other. The services include a producer and several consumers, each with a specific functionality. The project also includes configurations for Docker, Kubernetes, and CI/CD pipelines.

Python Flask Application
Overview
This project is built using Python and Flask, a lightweight web framework. Flask is used to create the main application logic and the RESTful APIs for various services. The microservices architecture allows each service to operate independently, making the system more modular and easier to maintain.

###Project directory Structure
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

### Project Structure
Each service in the project has its own directory with the following components:

- **Producer**: Handles the production of data or tasks to be consumed by other services.
- **Consumer One**: Consumes data from the producer and performs health checks.
- **Consumer Two**: Consumes data and handles item creation.
- **Consumer Three**: Manages stock information.
- **Consumer Four**: Processes orders.

### Key Components

#### Producer

- **producer.py**: Contains the main application logic for the producer service.
- **Dockerfile**: Used to create a Docker image for the producer.
- **requirements.txt**: Lists the dependencies required for the producer.

#### Consumer One

- **Dockerfile**: Used to create a Docker image for consumer one.
- **healthcheck.py**: Implements the health check functionality for consumer one.
- **requirements.txt**: Lists the dependencies required for consumer one.

#### Consumer Two

- **Dockerfile**: Used to create a Docker image for consumer two.
- **item_creation.py**: Implements the item creation functionality for consumer two.
- **requirements.txt**: Lists the dependencies required for consumer two.
- **repository/**: Contains modules related to data storage and retrieval.
  - **__init__.py**: Initializes the repository module.
  - **database.py**: Handles database connections and operations.
  - **entity.py**: Defines data entities.

#### Consumer Three

- **Dockerfile**: Used to create a Docker image for consumer three.
- **stock_management.py**: Implements the stock management functionality for consumer three.
- **requirements.txt**: Lists the dependencies required for consumer three.

#### Consumer Four

- **Dockerfile**: Used to create a Docker image for consumer four.
- **order_processing.py**: Implements the order processing functionality for consumer four.
- **requirements.txt**: Lists the dependencies required for consumer four.

### Setting Up the Flask Application

1. **Clone the repository**:
   ```sh
   git clone <repository_url>
   cd <repository_directory>
   ```

2. **Create a virtual environment**:
   ```sh
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies**:
   ```sh
   pip install -r requirements.txt
   ```

4. **Run the application**:
   ```sh
   flask run
   ```

### Environment Variables

Ensure you set the necessary environment variables required by each Flask application. These may include database URLs, API keys, and other configuration settings specific to your environment.

---

## Docker and Kubernetes

### Docker Compose

- **docker-compose.yml**: Docker Compose file for deploying the entire application. It orchestrates the setup of all microservices along with RabbitMQ and MySQL.

#### Running with Docker Compose

1. Build and start all services:
   ```sh
   docker-compose up --build
   ```

2. To stop the services:
   ```sh
   docker-compose down
   ```

### Kubernetes

- **manifest/**: Contains Kubernetes deployment and service manifests for each microservice and required dependencies like RabbitMQ and MySQL.
- **helm/**: Contains Helm charts for deploying the services in Kubernetes.

#### Deploying to Kubernetes

1. Apply the Kubernetes manifests:
   ```sh
   kubectl apply -f kubernetes/manifest/
   ```

2. Alternatively, use Helm to deploy the charts:
   ```sh
   helm install consumer-one kubernetes/helm/consumer_one/
   # Repeat for other services
   ```

## CI/CD Pipelines

### Jenkins Pipeline

- **build.pipeline**: Jenkins pipeline script for building the application.
- **deployment.pipeline**: Jenkins pipeline script for deploying the application.
- **shared_library/**: Contains reusable functions for Jenkins pipelines.

### Setting Up Jenkins

1. Configure Jenkins to use the provided pipeline scripts.
2. Ensure the shared library functions are available in Jenkins:
   ```sh
   # Place the shared library in the appropriate directory
   ```

## Contributing

1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Make your changes.
4. Commit your changes (`git commit -am 'Add new feature'`).
5. Push to the branch (`git push origin feature-branch`).
6. practice in your local, if descoverd somthing new to contribute then Create a new Pull Request.
