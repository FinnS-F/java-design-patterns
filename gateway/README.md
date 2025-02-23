---
title: Gateway
category: Structural
language: en
tag:
- Gang of Four
- Decoupling

---

## Intent

Provide a interface to access a set of external systems or functionalities. Gateway provides a simple uniform view of
external resources to the internals of an application.

## Explanation

Real-world example

> Gateway acts like a real front gate of a certain city. The people inside the city are called
> internal system, and different outside cities are called external services. The gateway is here
> to provide access for internal system to different external services.

In plain words

> Gateway can provide an interface which lets internal system to utilize external service.

Wikipedia says

> A server that acts as an API front-end, receives API requests, enforces throttling and security
> policies, passes requests to the back-end service and then passes the response back to the requester.

**Programmatic Example**

The main class in our example is the `ExternalService` that contains items.

```java
class ExternalServiceA implements Gateway {
    @Override
    public void execute() throws Exception {
        System.out.println("Executing Service A");
        // Simulate a time-consuming task
        Thread.sleep(1000);
    }
}

/**
 * ExternalServiceB is one of external services.
 */
class ExternalServiceB implements Gateway {
    @Override
    public void execute() throws Exception {
        System.out.println("Executing Service B");
        // Simulate a time-consuming task
        Thread.sleep(1000);
    }
}

/**
 * ExternalServiceC is one of external services.
 */
class ExternalServiceC implements Gateway {
    @Override
    public void execute() throws Exception {
        System.out.println("Executing Service C");
        // Simulate a time-consuming task
        Thread.sleep(1000);
    }

    public void error() throws Exception {
        // Simulate an exception
        throw new RuntimeException("Service C encountered an error");
    }
}
```

To operate these external services, Here's the `App` class:

```java
public class App {
    /**
     * Simulate an application calling external services.
     */
    public static void main(String[] args) throws Exception {
        GatewayFactory gatewayFactory = new GatewayFactory();

        // Register different gateways
        gatewayFactory.registerGateway("ServiceA", new ExternalServiceA());
        gatewayFactory.registerGateway("ServiceB", new ExternalServiceB());
        gatewayFactory.registerGateway("ServiceC", new ExternalServiceC());

        // Use an executor service for asynchronous execution
        Gateway serviceA = gatewayFactory.getGateway("ServiceA");
        Gateway serviceB = gatewayFactory.getGateway("ServiceB");
        Gateway serviceC = gatewayFactory.getGateway("ServiceC");

        // Execute external services
        try {
            serviceA.execute();
            serviceB.execute();
            serviceC.execute();
        } catch (ThreadDeath e) {
            System.out.println("Interrupted!" + e);
            throw e;
        }
    }
}
```

The `Gateway` interface is extremely simple.

```java
interface Gateway {
    void execute() throws Exception;
}
```

Program output:

```java
        Executing Service A
        Executing Service B
        Executing Service C
```

## Class diagram

![alt text](./etc/gateway.urm.png "gateway")

## Applicability

Use the Gateway pattern

* To access an aggregate object's contents without exposing its internal representation.
* To integration with multiple external services or APIs.
* To provide a uniform interface for traversing different aggregate structures.

## Tutorials

* [Pattern: API Gateway / Backends for Frontends](https://microservices.io/patterns/apigateway.html)

## Known uses

* [API Gateway](https://java-design-patterns.com/patterns/api-gateway/)
* [10 most common use cases of an API Gateway](https://apisix.apache.org/blog/2022/10/27/ten-use-cases-api-gateway/)

## Credits

* [Gateway](https://martinfowler.com/articles/gateway-pattern.html)
* [What is the difference between Facade and Gateway design patterns?](https://stackoverflow.com/questions/4422211/what-is-the-difference-between-facade-and-gateway-design-patterns)
