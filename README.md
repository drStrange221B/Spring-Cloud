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
 

SPRING CLOUD EUREKA DASHBOARD
	ENABLED BY DEFAULT
	- eureka.dashboard.enabled = true
	Display useful metadata and service status
use below url: 
	localhost:8761
	
	
CONFIGURING SPRING CLOUD EUREKA
	There are several different areas where you can configure Spring Cloud Eureka. there are mainly 3
1) eureka.server.*
2) eureka.client.*
3) eureka.instance.*
	
1) EUREKA SERVER CONFIGURATION
	Eureka Server - the discovery server: contains a registry of services that can be disovered
ALL CONFIGURATION UNDER THE eureka.server prefix
	EurekaServerConfigBean
	
2) Eureka Client Configuration ths is responsible for how the discovery client interacts with the Discovery server. 
	Eureka Client - anything that can discover services
ALL CONFIGURATION UNDER THE eureka.client prefix
	EurekaClientConfigBean
	
3) EUREKA INSTANCE CONFIGURATION 
	Eureka Instance - anything that registers itself with the Eureka Server to be discovered by others
All CONFIGURATION UNDER THE eurika.instance prefix 
	EurekaInstaneConfigBean

	
	SPRING CLOUD EUREKA: HEALTH AND HIGH AVAILABILITY

- Regularly checks the status fo services
- Clients sends heartbeats every 30 second ( default) 
- Services removed after 90 secs of no heartbeats (default)
- Can customize configuration to use /health endpoint
	eureka.client.healthcheck.enabled
	
	
- the registry is distributed (cashed locally on every client)
- Clients can operate without discovery server
- Fetches deltas to update registry.
	
	
	AWS SUPPORT OF SPRING CLOUD EURICA SERVER
	
When an application that's using the Eureka Client starts up, it checks to see if it's running on an AWS instance. if it is, it calls out to the local metadata service and retrives some metadata about that instance. And it gets things that are specific to AWS. such as the Amazon Machine Image that's running or what 
region it's running in or what zone. then it send that information up to the Disvovery Server when it registers. Given the facts that things can change so often in AWS, it is important that the Discovery Server be locatied at a well-know location, So Eureka ads support for Elastic IP binding. When a Eureka Server starts up and it notices that it's running in AWS, it will try to bind to the next available Elastic IP so that it has a static or well-known IP. Eureka Client is also zone aware with a preferene for the zone that it's currently running in. So it will try to contact the Discovery Server in its current zone. it it cannot reach one on current zone. Then it will try the next zone and tyr to find the next available Discovery server. And last you can configure the Eureka Client to fetch the registry of different remote regions. 
	
	CONFIGURING SPRINGCLOUD EUREKA FOR AWS
	
	@Configuration
	public class AppConfig{
	
		@Bean
                public EurekaInstanceConfigBean eurekaInstanceConfig (InetUtilsProperies properties){
	
	EurekaInstanceConfigBean bean = new EurekaInstanceConfigBean(new InetUtils(properties));
	AmazonInfo info = AmazonInfo.Builder.newBuilder().autoBuild("eureka")';
	bean.setDataCenterInfo(info);
	return bean;
	
	
	}
	
	}

	Availability Zones Configuration in application.properties
	eureka.client.availability-zones.[region] = [az1], [az2],[az3]
	e.g
	eureka.client.availability-zones.us-east-1 = us-east-1b, us-east-1e
	
	
	Service URL Configuration in application.properties
	eureka.client.service-url.[zone] = http://[eip-dns]/eureka
	- * Use EIP DNS name. Do not use IP (as of version Eureka 1.4) 
	
	
	

	
	
