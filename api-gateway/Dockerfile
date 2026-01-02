# Stage 1: Build the application using Maven
FROM maven:3.5.9-openjdk-17 AS builder
WORKDIR /app
# Copy the pom.xml and source code to the container
COPY pom.xml .
COPY src ./src
# Build the application and skip tests for faster deployment
RUN mvn clean package -DskipTests

# Stage 2: Create the final lightweight image
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
# Copy only the built JAR file from the builder stage
# We use a wildcard (*) because the version name might change (e.g., 0.0.1-SNAPSHOT)
COPY --from=builder /app/target/*.jar app.jar
# Command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]