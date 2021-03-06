# Serverless Java container [![Build Status](https://travis-ci.org/awslabs/aws-serverless-java-container.svg?branch=master)](https://travis-ci.org/awslabs/aws-serverless-java-container) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.amazonaws.serverless/aws-serverless-java-container/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.amazonaws.serverless/aws-serverless-java-container) [![Help](http://img.shields.io/badge/help-gitter-E91E63.svg?style=flat-square)](https://gitter.im/awslabs/aws-serverless-java-container)

## Serverless Framework - Why the fork?
This example was originally created by AWS Labs. We here at [Serverless Guru](https://serverlessguru.com/) forked the project from [AWS Labs](https://github.com/awslabs) to make a version of the [Spring Boot pet store example](https://github.com/serverless-guru/aws-serverless-java-container/tree/master/samples/springboot/pet-store) using the [Serverless Framework](https://github.com/serverless/serverless) instead. As [Serverless.com partners](https://serverless.com/partners/), Serverless Guru also published [other serverless framework](https://github.com/serverless-guru/serverless-termination-protection) content.

**Note:** The only sample in the `samples/` folder that has been converted from `SAM` to the Serverless Framework, is the one in the `samples/springboot` folder.

## About
The `aws-serverless-java-container` makes it easy to run Java applications written with frameworks such as [Spring](https://spring.io/), [Spring Boot](https://projects.spring.io/spring-boot/), [Apache Struts](http://struts.apache.org/), [Jersey](https://jersey.java.net/), or [Spark](http://sparkjava.com/) in [AWS Lambda](https://aws.amazon.com/lambda/).

Serverless Java Container natively supports API Gateway's proxy integration models for requests and responses, you can create and inject custom models for methods that use custom mappings.

Follow the quick start guides in [our wiki](https://github.com/awslabs/aws-serverless-java-container/wiki) to integrate Serverless Java Container with your project:
* [Spring quick start](https://github.com/awslabs/aws-serverless-java-container/wiki/Quick-start---Spring)
* [Spring Boot quick start](https://github.com/awslabs/aws-serverless-java-container/wiki/Quick-start---Spring-Boot)
* [Apache Struts quick start](https://github.com/awslabs/aws-serverless-java-container/wiki/Quick-start---Struts)
* [Jersey quick start](https://github.com/awslabs/aws-serverless-java-container/wiki/Quick-start---Jersey)
* [Spark quick start](https://github.com/awslabs/aws-serverless-java-container/wiki/Quick-start---Spark)   

Below is the most basic AWS Lambda handler example that launches a Spring application. You can also take a look at the [samples](https://github.com/awslabs/aws-serverless-java-container/tree/master/samples) in this repository, our main wiki page includes a [step-by-step guide](https://github.com/awslabs/aws-serverless-java-container/wiki#deploying-the-sample-applications) on how to deploy the various sample applications using Maven and the [Serverless Framework](https://github.com/serverless/serverless). 

```java
public class StreamLambdaHandler implements RequestStreamHandler {
    private static SpringLambdaContainerHandler<AwsProxyRequest, AwsProxyResponse> handler;
    static {
        try {
            handler = SpringLambdaContainerHandler.getAwsProxyHandler(PetStoreSpringAppConfig.class);
        } catch (ContainerInitializationException e) {
            // if we fail here. We re-throw the exception to force another cold start
            e.printStackTrace();
            throw new RuntimeException("Could not initialize Spring framework", e);
        }
    }

    @Override
    public void handleRequest(InputStream inputStream, OutputStream outputStream, Context context)
            throws IOException {
        handler.proxyStream(inputStream, outputStream, context);
    }
}
``` 
