# Basic Creating Docker Images
#FROM openjdk:21-slim
#COPY target/*.jar app.jar
#EXPOSE 5000
#ENTRYPOINT ["java","-jar","/app.jar"]

#Build jar with multy stage with maven 
#FROM maven:3.9-eclipse-temurin-21 AS build
#WORKDIR /home/app
#COPY . /home/app
#RUN mvn -f /home/app/pom.xml clean package

#FROM openjdk:21-slim
#EXPOSE 5000
#COPY --from=build /home/app/target/*.jar app.jar
#ENTRYPOINT ["java","-jar","/app.jar"] 

# Docker trabaja con capas asi que primero ponemos todo lo que casi no cambia
# En este caso el pom.xml y la clase *Application por lo regular no cambia 
# la descarga de dependencias suele ser lo mas tardado, por eso lo copiamos primero 
# Si no ha cambiado se puede ahorrar ese tiempo de construccion en la imagen 

FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /home/app

COPY ./pom.xml /home/app/pom.xml
COPY ./src/main/java/com/in28minutes/rest/webservices/restfulwebservices/RestfulWebServicesApplication.java	/home/app/src/main/java/com/in28minutes/rest/webservices/restfulwebservices/RestfulWebServicesApplication.java

RUN mvn -f /home/app/pom.xml clean package

COPY . /home/app
RUN mvn -f /home/app/pom.xml clean package

FROM openjdk:21-slim
EXPOSE 5000
COPY --from=build /home/app/target/*.jar app.jar
ENTRYPOINT [ "sh", "-c", "java -jar /app.jar" ]

# Mediante el plugin de maven que viene por default en un projecto spring boot
# Se puede generar la imagen de docker con ayuda del comando se debe de quitar el archivo Dockerfile
# mvn spring-boot:build-image(Build a container image)
#<build>
#		<plugins>
#			<plugin>
#				<groupId>org.springframework.boot</groupId>
#				<artifactId>spring-boot-maven-plugin</artifactId>
#			</plugin>
#		</plugins>
#	</build> 
# Otros comandos utiles son 
# mvn spring-boot:repackage(create a jar o war file)
# mvn spring-boot:run(run application)