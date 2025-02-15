# Microservice Design Patterns

This repository provides implementations and examples of various microservice design patterns using Java and Spring Boot. It serves as a reference for building scalable, maintainable, and well-structured microservices.

## 📂 Project Structure

- **`cqrs-ms-design`** - Implements CQRS (Command Query Responsibility Segregation) with Axon Server.
- **`ms-custom-registry`** - Custom service registry implementation for microservices.
- **`ms-gateway-external-config`** - Contains a microservices banking service with service registry, API Gateway, and centralized configuration.
- **`ms-java-design`** - Java-based implementation of various microservice design patterns.

## 🚀 Technologies Used

- **Java 17**  
- **Spring Boot 3.x**  
- **Axon Server (CQRS Implementation)**  
- **Spring Cloud Gateway**  
- **Spring Cloud Config Server**  
- **Eureka / Custom Service Registry**  
- **Docker** (if applicable)  

## 📌 Getting Started

1. Clone the repository:  
   ```sh
   git clone https://github.com/ps1437/microservice-design-pattern.git
   cd microservice-design-pattern
   ```

2. Build the project:  
   ```sh
   mvn clean install
   ```

3. Run individual services as needed:  
   ```sh
   cd cqrs-ms-design  
   mvn spring-boot:run  
   ```

## 🛠️ Features & Design Patterns

- **CQRS (Command Query Responsibility Segregation)**  
- **API Gateway with centralized configuration**  
- **Custom Service Registry**  
- **Microservice Banking Service Implementation**  
- **Microservice Communication Patterns**  

## 📜 Contributing

Feel free to contribute by submitting issues or pull requests.


