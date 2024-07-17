FROM openjdk:8u111-jdk-alpine
COPY target/productcatalogue-0.0.1-SNAPSHOT.jar app.jar
COPY product-catalogue.yml app-config.yml
EXPOSE 8020
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","app.jar", "server", "app-config.yml"]
