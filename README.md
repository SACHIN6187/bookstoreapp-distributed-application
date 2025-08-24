# BookStoreApp-Distributed-Application

<hr>

## About this project
This is an Ecommerce project currently in development, where users can add books to a cart and complete purchases.

The application is developed using Java, Spring, and React. It utilizes Spring Cloud Microservices and the Spring Boot Framework extensively to create a distributed architecture.

<hr>

## Frontend Checkout Flow
![CheckOutFlow](https://user-images.githubusercontent.com/14878408/103235826-06d5ca00-4969-11eb-87c8-ce618034b4f3.gif)

## Architecture
All microservices are developed using Spring Boot. These applications are registered with a Eureka discovery server.

The Frontend React App makes requests to an NGINX server which acts as a reverse proxy. The NGINX server redirects requests to the Zuul API Gateway.

Zuul routes requests to the appropriate microservice based on the URL route. Zuul also registers with Eureka to retrieve the IP or domain for the microservice while routing the request.

<hr>

## Run this project in Local Machine

> Frontend App 

Navigate to the "bookstore-frontend-react-app" folder.
Run the following commands to start the Frontend React Application:

```
yarn install
yarn start
```

> Backend Services

To start Backend Services, follow the steps below using IntelliJ, Eclipse, or the Command Line.

Import this project into your IDE and run all Spring Boot projects, or build all jars by running the "mvn clean install" command in the root parent pom.
All services will be active on the ports mentioned below.

Note: Running services this way does not provide microservice monitoring. To see metrics like JVM memory, Tomcat error counts, and other data, use the Docker deployment method.

> Using Docker (Recommended)

1. Start the Docker Engine on your machine.
2. Run "mvn clean install" at the root of the project to build all microservice jars.
3. Run "docker-compose up --build" to start all containers.

Use the "Postman Api collection" in the Postman directory to make requests to various services.

Services will be exposed on these ports:

```
Api Gateway Service       : 8765
Eureka Discovery Service  : 8761
Consul Discovery          : 8500
Account Service           : 4001
Billing Service           : 5001
Catalog Service           : 6001
Order Service             : 7001
Payment Service           : 8001
```

<hr>

### Service Discovery
This project uses Eureka or Consul as the Discovery service.

When running services locally, Eureka is used for service discovery. When running via Docker, Consul is utilized.

Consul is preferred in the Docker environment due to its expanded feature set. Local execution uses Eureka to avoid the overhead of managing a local Consul agent.

<hr>

### Troubleshooting

If you encounter issues starting services or if an API fails, it may be due to database schema updates.

In such cases, clear or drop the "bookstore_db" database. If issues persist, please raise an issue on GitHub and I will provide assistance.

<hr>

## Deployment (Future State)
AWS is the intended cloud provider for this project.

The project will be deployed across multiple Regions and Availability Zones.

The React App, Zuul, and Eureka will be public-facing services located in a public subnet. All microservices will be containerized using Docker and deployed in AWS ECS within a private subnet.

Private subnets will use a NAT Gateway for external internet requests. A Bastion host can be used to SSH into the private subnet microservices.

The AWS Architecture diagram below provides further detail:

![Bookstore Final](https://user-images.githubusercontent.com/14878408/65784998-000e4500-e171-11e9-96d7-b7c199e74c4c.jpg)

<hr>

## Monitoring
There are two setups available for monitoring:

1. Prometheus and Grafana.
2. TICK stack monitoring.

Prometheus operates on a pull model, using Consul discovery to provide target hosts dynamically. This ensures that new service instances are automatically monitored without manual configuration.

The TICK (Telegraf, InfluxDB, Chronograf, Kapacitor) stack supports both push and pull models. InfluxDB serves as the time-series database, while Telegraf pulls metrics from specified targets. Chronograf or Grafana can be used for visualization, and Kapacitor handles alarm configurations.

The "docker-compose" file manages the deployment of these monitoring containers.

Dashboards are available at the following ports:

```
Grafana    : 3030
Zipkin     : 9411
Prometheus : 9090
Telegraf   : 8125
InfluxDb   : 8086
Chronograf : 8888
Kapacitor  : 9092 
```

```
Initial Grafana Login:
Username : admin  
Password : admin
```

<hr>

**Screenshots of Tracing in Zipkin**

<img alt="Zipkin" src="https://user-images.githubusercontent.com/14878408/65939069-6b426a80-e442-11e9-90fd-d54b60786d41.png">
<hr>
<img alt="Zipkin" src="https://user-images.githubusercontent.com/14878408/65939165-bb213180-e442-11e9-90fd-d54b60786d41.png">

<hr>

**Screenshots of Monitoring in Grafana**

<img width="1680" alt="Screen Shot 1" src="https://user-images.githubusercontent.com/14878408/66936473-65ac6d80-f05b-11e9-9e7d-9652059438cd.png">

<img width="1680" alt="Screen Shot 2" src="https://user-images.githubusercontent.com/14878408/66936524-79f06a80-f05b-11e9-8898-1002813aad8e.png">

<hr>

**Screenshots of Monitoring in Chronograf (TICK)**

![Screen Shot 3](https://user-images.githubusercontent.com/14878408/66934353-f8e3a400-f057-11e9-82ab-eda7a230c09d.png)

![Screen Shot 4](https://user-images.githubusercontent.com/14878408/66934482-2e888d00-f058-11e9-8dea-f1f275765265.png)

<hr>

> Account Service

To obtain an "access_token" for a user, you require a "clientId" and "clientSecret".

```
clientId : '93ed453e-b7ac-4192-a6d4-c45fae0d99ac'
clientSecret : 'client.devd123'
```

Current system users:

```
Admin 
userName: 'admin.admin'
password: 'admin.devd123'
```

```
Normal User 
userName: 'devd.cores'
password: 'cores.devd123'
```

*To get the accessToken (Admin User):* 

```curl 93ed453e-b7ac-4192-a6d4-c45fae0d99ac:client.devd123@localhost:4001/oauth/token -d grant_type=password -d username=admin.admin -d password=admin.devd123```

<hr>

## Maintainer
This project is maintained by Aarthi Reddy Jannapureddy. Aarthi is a Data Analyst with over 3 years of experience in SQL, Python, and data visualization, focusing on building robust reporting systems and supporting data-driven business decisions.

Contact: aarthireddyj@gmail.com
LinkedIn: https://www.linkedin.com/in/aarthireddyj