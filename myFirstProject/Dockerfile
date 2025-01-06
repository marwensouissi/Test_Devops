FROM openjdk:17-alpine

WORKDIR /app

# Copy the JAR file from your build process
COPY target/myFirstProject-1.0.jar app.jar

# Expose port 8089
EXPOSE 8089

# Run the Spring Boot application
ENTRYPOINT ["java", "-Dserver.port=8089", "-jar", "app.jar"]
