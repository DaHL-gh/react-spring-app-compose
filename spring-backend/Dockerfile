# компилируем java
FROM maven:3.8.5-openjdk-17 AS build

WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline

COPY src ./src
RUN mvn clean install -DskipTests

# образ для запуска backend
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

RUN addgroup -S dockeruser && adduser -S dockeruser -G dockeruser

COPY --from=build /app/target/*.jar app.jar

RUN chown -R dockeruser:dockeruser app.jar
USER dockeruser

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
