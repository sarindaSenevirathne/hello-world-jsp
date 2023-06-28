# Use a base image with Maven to build the application
FROM maven:3.5.4-jdk-8-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Maven project definition file(s) to the container
COPY pom.xml .

# Download the project dependencies
RUN mvn dependency:go-offline -B

# Copy the application source code to the container
COPY src ./src

# Build the application and generate the WAR file
RUN mvn package -DskipTests

# Use a base image with Java and Tomcat pre-installed
FROM tomcat:9.0.76-jdk21-openjdk

USER 10014

# Set the working directory inside the container
WORKDIR /usr/local/tomcat/webapps

# Remove the default Tomcat application
RUN rm -rf ROOT

# Copy the generated WAR file to the container
COPY --from=builder /app/target/demo-1.0-SNAPSHOT.war ./ROOT.war

# Expose the default Tomcat port
EXPOSE 8080

# Start Tomcat when the container launches
CMD ["catalina.sh", "run"]
