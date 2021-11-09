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
	
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>

application.properties
spring.application.name = service
eureka.client.service-url.defaultZone = http://localhost:8761/eureka

application.yml
spring:
 application:
  name: service
eureka:
 client:
  service-url: 
   defaultZone: http://localhost:8761/eureka
   
   

Application Client: application client can be both client and service.
- Calls another application service to implement its functionality
- The issuer of requests 
- Depends on other service
- User of the discovery client
  to find service locations 
  
  Using Spring Cloud Eureka Client in an Application Client
  
  pom.xml
  
  <dependencyManagement>
	<dependencies>
		<dependency>
			<proupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-dependencies</artifactId>
			<version>Camden.SRS</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyMahagement>

<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>

application.properties
spring.application.name = client
spring.client.service-url.defaultZone = http://localhost:8761/eureka
eureka.client.register-with-eureka=false

application.yml
spring:
 application:
  name: client
eureka:
 client:
  server-url:
   sefaultZone: http://localhost:8761/eureka
  register-with-eureka: false
  
  Application.java
  
  @SpringBootApplication
  @EnableDiscoveryClient
  public class Application{
  	}
	
	
Discovering Services as a Client: Two Options

1) Eureka server Specific
  - @Inject
  - EurekaClient client

Using the EurekaClient: getNextServerFromEureka - pick the next instance using round-robin

- 1st argument - virtual host name or service id of service to call 
  By default, apps use the spring.application.name as their virtual hostname when registering
- 2nd argument - whether or not this is a secure request

InstanceInfo instance = eurekaClient.getNextServerFromEureka("service-id", false);
String baseUrl = instance.getHomePageUrl();


  
2) Discovery server agnostic. ( this is spring discovery client not nettlix discovery client) 
  - DiscoveryClient client
  
Using the Spring DiscoveryClient
getInstance - return all instances of the given service id
- 1st argument - virtual host name or service id of service to call 
- By default, apps use the spring.application.name as the virtual hostname when registering 

List<ServiceInstance> instances = client.getInstances("service-id");
String baseUrl = instances.get(0).getUri().toString();
 

	
