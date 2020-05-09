# Copy everything, install maven, and build the application
FROM openjdk:8 as java-app-builder
COPY . .
RUN apt-get update && apt-get install -y maven
RUN mvn install

# Now, build the "production" container from the build output from the prior image
FROM openjdk:8-jdk-alpine
# Environment Variable that defines the endpoint of sentiment-analysis python api.
ENV SA_LOGIC_API_URL http://localhost:5000
# Take just the jar output from the prior command
COPY --from=java-app-builder target/sentiment-analysis-web-0.0.1-SNAPSHOT.jar /
EXPOSE 8080
CMD ["java", "-jar", "sentiment-analysis-web-0.0.1-SNAPSHOT.jar", "--sa.logic.api.url=${SA_LOGIC_API_URL}"]
