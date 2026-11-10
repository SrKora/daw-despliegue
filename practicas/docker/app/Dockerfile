FROM maven:3.9.16-eclipse-temurin-21-alpine AS build

WORKDIR /app

COPY pom.xml .
RUN mvn -B dependency:go-offline

COPY src ./src
RUN mvn -B package

FROM eclipse-temurin:21.0.12_8-jre-alpine-3.24

RUN addgroup -S escaparate \
 && adduser -S -G escaparate -H -s /sbin/nologin escaparate \
 && mkdir -p /app/uploads \
 && chown escaparate:escaparate /app/uploads

WORKDIR /app

COPY --from=build /app/target/escaparate.war app.war

ENV APP_STORAGE_PATH=/app/uploads

USER escaparate

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/app.war"]
