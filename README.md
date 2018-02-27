# Crypto Cloud
Personal project putting together all the stuff i learned about Microservices in the last couple of months. Project will cover service configuration, discovery, resiliency, routing, eventing, tracing and finally deployment. As i'm personal interested in crypto currencies like Ethereum et al i decided to build a "Crypto Cloud" with a couple of services around the topic. The following figure gives you a rough overview:

<img src="./cryptocloud.png">

Services are build on top of Spring Boot, Spring Cloud and Netflix OSS using either Java or Kotlin. 

**DISCLAIMER**: This project is not a perfect use case for a microservice architecture. It is only for learning the above-mentioned techniques.

### Services

#### API Gateway
The API Gateway acts as entry point to services. All requests first go through the API Gateway. It then routes requests to the appropriate service. 

E.g. the following configuration makes the Currency Service available under http://{{api-gateway}}/api/currencyservice
```yaml
zuul:
  prefix: /api
  sensitiveHeaders: Cookie,Set-Cookie
  routes:
    currencyservice:
      path: /currencyservice/**
      serviceId: currencyservice
```

Service uses Zuul Proxy together with Spring. 

#### Currency Service
Simple REST API with the possibility to list available currencies by page or id as well as saving currencies. See [Postman Request Collection](./Crypto-Cloud.postman_collection.json) for example requests. See [Postman Docs](https://www.getpostman.com/docs/) to get more information about Postman.

#### Rates Collector
Microservice responsible for collecting up-to-date currency rates from an external data source and pushing those rates into the Currency Service database. The synchronization takes place every three minutes.

### Side cars
#### Configuration Service
Sidecar to manage Crypto-Cloud wide configuration for each service. E.g. used by the currency service to configure the mongodb. To see the configuration start the cloud and navigate to the [Currency Service Default Configuration](http://localhost:8888/currencyservice/default). The technology behind this configuration service is [Spring Cloud Config](https://cloud.spring.io/spring-cloud-config/)

#### Discovery Service
Netflix open source Service Discovery Server [Eureka](https://netflix.github.io)

# Running the cloud on your local machine

## Pre requisites
1.	[Apache Maven](http://maven.apache.org).
2.	[Docker](http://docker.com).

## Building & Running the cloud on your local machine
First you have to build the code as a docker image. Therefor open a terminal, navigate to source code root directory and run the following maven command:

   **mvn clean package docker:build**

Once the build is finished you can start the cloud with the following docker-compose command:

   **docker-compose -f docker/docker-compose.yml up**

# Learning microservices
Finally a big thank you to the authors of "Building Microservices" (Sam Newman), "The TAO of Microservices" (Richard Rodger) and "Spring Microservices in Action" (John Carnell) who helped me to dive deeper in this complex topic.