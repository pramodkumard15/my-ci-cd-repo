# Use an official Maven image to build the app
FROM maven:3.8.4-openjdk-17-slim AS build

# Set the working directory
WORKDIR /app

# Copy pom.xml and install dependencies
COPY pom.xml . 
RUN mvn dependency:go-offline

# Copy the entire src folder from the host machine to the container
COPY src/ /app/src/

# Package the app using Maven
RUN mvn clean package

# Use an official OpenJDK runtime for the final image
FROM openjdk:17-slim

# Set the working directory
WORKDIR /app

# Copy the jar file from the build stage
COPY --from=build /app/target/my-backend-1.0-SNAPSHOT.jar my-app.jar

# Expose the application port
EXPOSE 8081

# Run the Java application
CMD ["java", "-jar", "my-app.jar"]
