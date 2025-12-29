# =========================
# 1️⃣ Build stage
# =========================
FROM maven:3.9.9-eclipse-temurin-17 AS build

WORKDIR /app

# Copier les fichiers Maven
COPY pom.xml .
COPY .mvn .mvn
COPY mvnw .
RUN chmod +x mvnw

# Télécharger les dépendances (cache)
RUN ./mvnw dependency:go-offline

# Copier le code source
COPY src src

# Build le JAR
RUN ./mvnw clean package -DskipTests

# =========================
# 2️⃣ Runtime stage
# =========================
FROM eclipse-temurin:17-jre

WORKDIR /app

# Copier le JAR depuis le build
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8082

ENTRYPOINT ["java", "-jar", "app.jar"]

