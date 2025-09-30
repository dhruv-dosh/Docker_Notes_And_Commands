# --------------------------------------------------------------------------
# STAGE 1: Build the Spring Boot JAR
# --------------------------------------------------------------------------
FROM maven:3.9-jdk-17-slim AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the source code and the Maven project file (pom.xml)
COPY . /app

# Package the application into an executable JAR file
# '-DskipTests' saves time by skipping test execution during the Docker build
RUN mvn clean package -DskipTests

# --------------------------------------------------------------------------
# STAGE 2: Run the Spring Boot Application
# This stage uses a light JRE image for a smaller final container size.
# --------------------------------------------------------------------------
FROM openjdk:17-jre-slim

# Expose the backend port (5054, as specified in your project)
EXPOSE 5054

# Set the working directory
WORKDIR /app

# Copy the executable JAR from the 'build' stage
# The name '*.jar' assumes you have one main JAR file created by Maven
COPY --from=build /app/target/*.jar app.jar

# Define the command to run the Spring Boot application
# The Spring application profile can be set here or via environment variables in docker-compose
ENTRYPOINT ["java", "-jar", "app.jar"]