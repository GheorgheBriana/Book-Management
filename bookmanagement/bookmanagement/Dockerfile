# ---------- Stage 1: Build ----------
FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
# Copiem doar fișierele de build întâi (caching mai bun)
COPY pom.xml ./
COPY src ./src
# Build fără teste -> produce JAR în /app/target/
RUN ./mvnw -q -DskipTests package || (apk add --no-cache maven && mvn -q -DskipTests package)

# ---------- Stage 2: Run ----------
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
# Copiem JAR-ul rezultat din etapa de build
COPY --from=build /app/target/*.jar /app/app.jar
# Portul îl injectează Render prin $PORT
ENV JAVA_OPTS=""
CMD ["sh", "-c", "java $JAVA_OPTS -Dserver.port=$PORT -jar /app/app.jar"]
