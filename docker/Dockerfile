# Use an official Maven image as a build stage
FROM maven:3.9.0 AS build
WORKDIR /app

# Copy the POM file and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy the rest of the application source code
COPY . .

# Build the application
RUN mvn -B -DskipTests clean package

# Use an OpenJDK image for the runtime stage
FROM openjdk:11-jre-slim

# Create a non-root user and set the home directory
RUN useradd -r -m -d /home/appuser appuser

WORKDIR /app

# Copy the built JAR file from the build stage
COPY --from=build /app/target/*.jar app.jar

# Switch to the non-root user
USER appuser

# Command to run the application
CMD ["java", "-jar", "app.jar"]

