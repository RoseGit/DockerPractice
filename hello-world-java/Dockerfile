# Basic Creating Docker Images
#FROM openjdk:21-slim
#COPY target/*.jar app.jar
#EXPOSE 5000
#ENTRYPOINT ["java","-jar","/app.jar"]

FROM maven:3.9.6-eclipse-temurin-21-slim AS build
WORKDIR /home/app
COPY . /home/app
RUN mvn -f /home/app/pom.xml clean package

FROM openjdk:21-slim
EXPOSE 5000
COPY --from=build /home/app/target/*.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]