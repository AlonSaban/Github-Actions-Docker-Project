FROM maven:3.9.0 AS build
WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline

COPY . .

RUN mvn -B -DskipTests clean package

# Use an OpenJDK image for the runtime stage
FROM openjdk:11-jre-slim

RUN useradd -r -m -d /home/appuser appuser

WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

USER appuser

CMD ["java", "-jar", "app.jar"]
