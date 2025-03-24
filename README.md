# limits-services

## Limits Service is a Spring Boot microservice that retrieves configuration values from a Spring Cloud Config Server.
## It exposes an API endpoint to return the configured minimum and maximum limits.

## 📌 Project Structure
    limits-service/
    ├── src/main/java/com/in28minutes/microservices/limits_service/
    │   ├── bean/
    │   │   ├── Limits.java
    │   ├── configuration/
    │   │   ├── Configuration.java
    │   ├── controller/
    │   │   ├── LimitsController.java
    ├── resources/
    │   ├── application.properties

## 📂 File Breakdown

###  1️⃣ Limits.java (Model Class)
        -> Located in bean/, this class represents the response object for limits.

        public class Limits {
            private int minimum;
            private int maximum;
    
            // Default constructor
            public Limits() {}
    
            // Parameterized constructor
            public Limits(int minimum, int maximum) {
                this.minimum = minimum;
                this.maximum = maximum;
            }
    
            // Getters and Setters
        }
    
        -> This class is used to store and return minimum and maximum limit values.

###  2️⃣ Configuration.java (Configuration Binding)

        -> Located in configuration/, this class binds the external configuration values using @ConfigurationProperties.

        @Component
        @ConfigurationProperties("limits-service")
        public class Configuration {
            private int minimum;
            private int maximum;

            // Getters and Setters
        }

        -> Reads values from limits-service properties configured via Spring Cloud Config Server.
        -> Injected into the LimitsController.

###  3️⃣ LimitsController.java (REST Controller)

        -> Located in controller/, this class defines an API endpoint to retrieve limits.
        
        @RestController
        public class LimitsController {
        @Autowired
        private Configuration configuration;

        @GetMapping("/limits")
        public Limits retrieveLimits() {
            return new Limits(configuration.getMinimum(), configuration.getMaximum());
            }
        }

        -> The /limits endpoint returns values configured in Spring Cloud Config Server.
        -> Uses the Configuration class to inject values dynamically.

###  4️⃣ application.properties (Client Configuration)

    -> Located in resources/, this file configures the service to connect with the Spring Cloud Config Server.

    spring.application.name=limits-service
    spring.config.import=optional:configserver:http://localhost:8888
    limits-service.maximum=998
    limits-service.minimum=2
    spring.profiles.active=dev

    * spring.application.name → Defines the service name.
    * spring.config.import → Points to the Config Server URL.
    * limits-service.maximum & minimum → Local default values.
    * spring.profiles.active → Activates the dev profile.

