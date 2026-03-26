#
# Build (Updated to use a modern Maven/Java 8 image)
#
FROM maven:3.8-openjdk-8 AS build

# No need to install maven or ca-certificates; they are already here.
WORKDIR /opt/shipping

COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve
COPY src /opt/shipping/src/
RUN mvn package

#
# Run (Keeping your existing Run stage)
#
FROM eclipse-temurin:8-jdk

EXPOSE 8080

WORKDIR /opt/shipping

ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]

