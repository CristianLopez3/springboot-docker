# Spring Boot Docker
In this repository you can find an example of how to build docker images for 
java spring boot, first at all you need the next things:
* Java JDK
* Docker Engine 
* Maven (gradle si lo estas usando aunque las configuraciones ya serian diferentes)


### Basic docker image

```dockerfile
FROM openjdk:17-jdk-slim
ENV JAVA_OPTS " -Xms512m -Xmx512m -Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"
WORKDIR application
COPY ../../../../target/kbe-rest-brewery-0.0.1-SNAPSHOT.jar ./
ENTRYPOINT ["java", "-jar", "kbe-rest-brewery-0.0.1-SNAPSHOT.jar"]
```

after this build the image in your cli
```shell
docker build -f ./src/main/java/dockerBase/Dockerfile -t kbe-rest .
docker run -p 8080:8080 kbe-rest 
```
if you wanna see if the application is running you can use the next command
```shell
docker ps # to see the port
curl http://localhost:8080/api/v1/beer # use your navigator or some rest client
```


## Layers
Layers help us to prepare multi staging images in order to prepare for production.
```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <!-- MAVEN LAYERS FOR DOCKER FILES -->
                    <layers>
                        <enabled>true</enabled>
                        <includeLayerTools>true</includeLayerTools>
                    </layers>
                    <!-- MAVEN LAYERS FOR DOCKER FILES -->
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
</build>
```

