#FROM debian:10 AS build
#RUN apt-get update && apt-get -y install maven
#WORKDIR /opt/shipping
#COPY pom.xml /opt/shipping/
#RUN mvn dependency:resolve
#COPY src /opt/shipping/src/
#RUN mvn package
#
#FROM openjdk:8-jre-alpine
#EXPOSE 8080
#WORKDIR /opt/shipping
#ENV CART_ENDPOINT=cart:8080
#ENV DB_HOST=mysql
#COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar
#CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]


# Build Stage
FROM maven:3.8-jdk-8 AS build
WORKDIR /opt/shipping

# Copy the pom.xml to resolve dependencies first (this helps with caching)
COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve

# Copy the source code to the container and package the application
COPY src /opt/shipping/src/
RUN mvn package -DskipTests

# Runtime Stage
FROM openjdk:8-jre-alpine

# Expose the port the app will run on
EXPOSE 8080

# Set working directory
WORKDIR /opt/shipping

# Set environment variables for the app
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

# Copy the packaged JAR from the build stage
COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

# Command to run the application
CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]

# #
# # Build
# #
# FROM debian:10 AS build
# RUN apt-get update && apt-get -y install maven
# WORKDIR /opt/shipping
# COPY pom.xml /opt/shipping/
# RUN mvn dependency:resolve
# COPY src /opt/shipping/src/
# RUN mvn package

# #
# # Run
# #
# FROM openjdk:8-jdk
# EXPOSE 8080
# WORKDIR /opt/shipping
# ENV CART_ENDPOINT=cart:8080
# ENV DB_HOST=mysql
# COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar
# CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]



