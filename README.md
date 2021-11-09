# Spring-Cloud

Netflix OSS + Spring + SpringBoot = Spring Cloud Netflix

Service discovery
Distrubuted configuration
Client-Side load balancing
Intelligent Routing
Fault tolerance

two main fundamental topics 1) Spring Cloud 2) Spring Cloud Netflix


Service Discovery
Discovering Service with Spring Cloud
- Using Eureka client and server
- Configuration
- Health and High Availability
- Dashboard
- AWS Support

An actively managed registry of service locations
- source of truth
- one or more instances
- Spring Cloud Project
- Spring Cloud Eureka Server


###################### configuration of Discovery Server ###########################

- pom.xml
- Using Spring Cloud Eureka Server

<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>

<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>${spring-cloud.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
 </dependencyManagement>


application.properties
spring.application.name = discovery-server

application.yml
spring:
 application:
  name: discovery-server
  
  
  
