# Step 1: Use an official Gradle image to build the application
FROM gradle:8.3.0-jdk17 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the entire project to the container
COPY . .

# Build the application
RUN gradle clean bootJar

# Step 2: Use a lightweight JDK image for running the app
FROM eclipse-temurin:17-jdk

# Set the working directory for the runtime container
WORKDIR /app

# Copy the JAR file from the build stage
COPY --from=build /app/build/libs/*.jar app.jar

# Grafana's OTel Java distribution (zero-code instrumentation)
ADD https://github.com/grafana/grafana-opentelemetry-java/releases/download/v2.31.1/grafana-opentelemetry-java.jar /app/grafana-opentelemetry-java.jar

# Grafana Pyroscope Java agent
ADD https://github.com/grafana/pyroscope-java/releases/download/v2.9.2/pyroscope.jar /app/pyroscope.jar

ENV JAVA_TOOL_OPTIONS="-javaagent:/app/grafana-opentelemetry-java.jar -javaagent:/app/pyroscope.jar"

# Expose the port the application runs on
EXPOSE 8080

# Set the command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
