# Event-Driven Microservices Architecture with Kafka, ASP.NET Core, and Domain-Driven Design

This repository contains a set of services organized around an event-driven architecture using Kafka as the messaging platform. The services are built using C#, ASP.NET Core, and GraphQL, with event sourcing and CQRS principles. The system is designed following Domain-Driven Design (DDD) and Railway-Oriented Programming (ROP) patterns. 

## Technologies and Tools Used

- **Programming Language:** C#
- **Frameworks:**
  - ASP.NET Core
  - GraphQL
- **Messaging Platform:** Apache Kafka
- **Databases:**
  - SQL Server (for persistent storage)
  - MongoDB (for event storage)
- **Containerization:** Docker (for managing Kafka, Zookeeper, SQL Server instances)
- **Architecture Patterns:**
  - Domain-Driven Design (DDD)
  - Railway-Oriented Programming (ROP)
  - Event Handlers
  - MediatR
- **Used Tools and Environment**
   - Rider
   - vs code for databas project and as mongo studio and as  sql server studio
   - Linux Os
   - Miro to create eventStorm
## Services Overview

The system consists of the following microservices:

### 1. **Purchase Order Service** 
   - **Repository:** [PurchaseOrder Service](https://github.com/mohamedabotir/POContext) please read also markdown of project repo 
   - **Description:** This service handles purchase order requests and is responsible for publishing various events such as `Order Created`, `Order Approved`, `Order Begun Shipped`, `Order Shipped`, and `Order Closed`.
   - **Key Responsibilities:**
     - Handle and process incoming purchase order requests.
     - Publish events to Kafka for other services to react to.
   
### 2. **Shipping Order Service**
   - **Repository:** [Shipping Order Service](https://github.com/mohamedabotir/Shipping) please read also markdown of project repo 
   - **Description:** This service is responsible for shipping orders. It listens for the `Order Approved` event and consumes it to create a shipping order. It then publishes events like `Order Being Shipped` and `Order Shipped`.
   - **Key Responsibilities:**
     - Consume `Order Approved` event to initiate the shipping process.
     - Publish `Order Being Shipped` and `Order Shipped` events for further processing by other services.

### 3. **Inventory Service**
   - **Repository:** [Inventory Service](https://github.com/mohamedabotir/InventoryContext) please read also markdown of project repo
   - **Description:** This service manages inventory items and stock levels. It listens for the `Order Shipped` event and updates stock levels, as well as producing `Order Closed` events once the order has been completed.
   - **Key Responsibilities:**
     - Manage item creation and stock.
     - Consume `Order Shipped` event and produce `Order Closed` event.

### 4. **Shared Kernel**
   - Resides inside the Purchase Order service and contains common logic shared across the services, such as event definitions, utility classes, and other shared domain objects.

## Event Storming Diagram

The event storming diagram provides a high-level visualization of the events, commands, and interactions within the system. The events represent state changes that occur in the system as each service responds to them.

[Event Storming Diagram](https://miro.com/app/board/uXjVLyMevBk=/?share_link_id=775755712380)

## Functional Video Demonstration

This video demonstrates the functional flow of the services, showing how they interact with each other and how the events flow between services.

[Watch the video]([functional record .mp4](https://github.com/mohamedabotir/Po-Project/blob/main/functional%20record%20.mp4))

## Docker Setup

This system is containerized using Docker. Kafka, Zookeeper, and SQL Server are run as Docker containers to simulate the distributed environment. To get started with the project, follow these steps:

### Prerequisites:
- Docker (ensure Docker Desktop is running)

### Running the Services Locally:
1. Clone the repository.
then install below services 
 

# Run the Docker Compose
   ```bash
docker-compose up -d
```
# then build database projects 
will create databses 
# then start ruuning services (replace your api on applicationSettings.json,open port(not safe) or use sql server within the same network 'adapter')
```bash 
sudo docker build -t pocontext:latest <pocontext root path>
sudo docker run -d -p 8080:8080 --name pocontext-container --network kafka-network pocontext:latest
sudo ufw allow 1433/tcp 
sudo ufw allow 9092/tcp
sudo ufw allow 27017/tcp
sudo ufw allow 2181/tcp

```


