# multi stage build 
FROM maven as build
WORKDIR /app
COPY . .
RUN mvn clean install

FROM openjdk:11.0.10-jre
WORKDIR /app
COPY --from=build /app/target/zuul-0.0.1-SNAPSHOT.jar .
EXPOSE 9999
CMD ["java","-jar","zuul-0.0.1-SNAPSHOT.jar"]
