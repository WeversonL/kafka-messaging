<h1 align="center">
   Kafka Messaging
</h1>

Central repository to unify [kafka-producer](https://github.com/WeversonL/kafka-producer.git) and [kafka-consumer](https://github.com/WeversonL/kafka-consumer.git) projects. 

The idea of the projects is a simple payment queue, to integrate with Apache Kafka. The producer API will generate the payment and send it to the Kafka queue. The consumer API will consume the payments from this queue, process them and finally record them in a database table to store the processed payments

## Technologies
 
- [Spring Boot](https://spring.io/projects/spring-boot)
- [Spring MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html)
- [Spring Data JPA](https://spring.io/projects/spring-data-jpa)
- [Spring Kafka](https://docs.spring.io/spring-kafka/reference/html/)
- [Docker](https://docs.docker.com/get-started/)

## Practices adopted

- SOLID
- API REST
- Sending and consuming data in Message Broker queues
- Queries with Spring Data JPA
- Dependency Injection
- Automated tests
- Containerization with docker

## Get started

In the project, an [example environment variables file](example.env) will be available. The project will start using it, if you want to change the database user and password, change the DB_USER and DB_PASS values

0. Clone git repository

        git clone https://github.com/WeversonL/kafka-messaging.git
        cd kafka-messaging

### Running the application with docker-compose

1. Start with docker-compose

        docker-compose up -d

To monitor the created queue with [Kafdrop](https://github.com/obsidiandynamics/kafdrop), you can access: [localhost:19000](http://localhost:8080).

## API Endpoints

To make the HTTP requests below, the [httpie](https://httpie.io) tool was used:

- Create a payment 
```
$ http POST :8081/payment customerName="Weverson L" paymentMethod="C" paymentAmount=1000.0

[
  {
    "id": "2b7df20b-69ff-44a7-8f7d-530acad43c7d",
    "customerName": "Weverson L",
    "paymentAmount": 1000.0,
    "paymentMethod": "CREDIT",
    "status": "ANALYSIS"
  }
]
```

The created payments will be queued and you can watch them with Kafdrop as mentioned above. 
To view the data written to the table by the Consumer API. You can use the database manager of your choice. Here I will leave it in the command line to use [PSQL](https://www.postgresql.org/docs/current/app-psql.html)

## Enter the psql console passing the user. You will be asked for the bank access password, use the credentials that are in the examples environment variables file

```
$ psql -h localhost -p 5432 -U example-user -d kfk-payments
```

## Inside the console, execute the SELECT command

```
kfk-payments=# SELECT * FROM payments;
```

## License

`Kafka Messaging` is [MIT licensed](LICENSE).