# 1. Imagine de bază Java 17
FROM openjdk:17-jdk-slim

# 2. Directorul de lucru în container
WORKDIR /app

# 3. Copiază jar-ul generat de Maven în container
COPY target/bookmanagement-0.0.1-SNAPSHOT.jar app.jar

# 4. Comanda de rularee
ENTRYPOINT ["java","-jar","app.jar"]
