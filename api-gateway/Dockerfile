# STAGE 1: Build the JAR (The "Heavy" Stage)
# We use Maven 3.9 with Eclipse Temurin (Java 17) for the best performance in 2026
FROM maven:3.9.6-eclipse-temurin-17 AS builder

# Set the working directory inside the container
WORKDIR /app

# 1. Copy the pom.xml first to download dependencies (Caching)
# This makes your future builds MUCH faster on Render/Railway
COPY pom.xml .
RUN mvn dependency:go-offline -B

# 2. Copy the source code and build the application
COPY src ./src
RUN mvn clean package -DskipTests

# STAGE 2: Run the JAR (The "Light" Stage)
# We switch to a "JRE-only" image to reduce size and improve security
FROM eclipse-temurin:17-jre-jammy

WORKDIR /app

# 3. Copy only the final JAR from the builder stage
# We use a wildcard (*) so it finds your JAR regardless of the version number
COPY --from=builder /app/target/*.jar app.jar

# 4. Standard Spring Boot port
EXPOSE 8080

# 5. Run the application with container-optimized JVM settings
ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75.0", "-jar", "app.jar"]