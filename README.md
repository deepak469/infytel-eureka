Open the hosts file in C:\Windows\System32\drivers\etc

Add the below hostnames:

127.0.0.1    Eur1
127.0.0.1    Eur2
127.0.0.1    Eur3
Use the below yml file in the infytel-eureka server
spring:
  profiles: Eureka1
  application:
    name: Eureka
server:
  port: 2222 
eureka:
  instance:
    hostname: Eur1
  client:
    registerWithEureka: true
    fetchRegistry: true        
    serviceUrl:
      defaultZone: http://Eur2:2223/eureka/,http://Eur3:2224/eureka/
---
spring:
  profiles: Eureka2
  application:
    name: Eureka
server:
  port: 2223
eureka:
  instance:
    hostname: Eur2
  client:
    registerWithEureka: true
    fetchRegistry: true        
    serviceUrl:
      defaultZone: http://Eur1:2222/eureka/,http://Eur3:2224/eureka/
---
spring:
  profiles: Eureka3
  application:
    name: Eureka
server:
  port: 2224 
eureka:
  instance:
    hostname: Eur3
  client:
    registerWithEureka: true
    fetchRegistry: true        
    serviceUrl:
      defaultZone: http://Eur1:2222/eureka/,http://Eur2:2223/eureka/
Comma separated values in the defaultZone indicate peer awareness. Eureka1, Eureka2 and Eureka3 are peers of each other and hence will replicate the details across each other.
Update the application.properties file in GIT for the below property:
eureka.client.service-url.defaultZone=http://Eur1:2222/eureka,http://Eur3:2223/eureka,http://Eur3:2224/eureka
Run all the three profiles of the Eureka server and restart all the microservices

You will get three dashboards in three different Eureka ports. Since we have a cluster, each dashboard will have the same details of microservices as the other two Eureka Servers in the cluster.

Bring down a microservice. You will find that since each Eureka server in a cluster replicates itself, all Eureka servers in the cluster will now have the same updated information.

