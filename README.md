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
spring.application.name = discovery-server  # false for only one server
eureka.client.register-with-eureka=false # false for only one server
eureka.client.fetch-registry=false
server.port=8761

application.yml
spring:
 application:
  name: discovery-server
  
  
Application.java
@SpringBootApplication
@EnableEurekaServer
public class SpringCloudServiceDiscoveryApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringCloudServiceDiscoveryApplication.class, args);
	}

}


Application Service
Provides some application functionality
The receive of requests 
A dependency of other services 
One or more instances
User of the discovery client
- Register
- Deregister

pom.xml

<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-dependencies</artficatId>
			<version>Camden.SR2</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
	
  
