# =========================
# 1️⃣ Build stage
# =========================
FROM maven:3.9.9-eclipse-temurin-17 AS build

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers Maven pour télécharger les dépendances
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

# Répertoire de travail
WORKDIR /app

# Copier le JAR depuis le build
COPY --from=build /app/target/*.jar app.jar

# Exposer le port (à configurer selon ton application)
EXPOSE 8082

# Commande pour démarrer le service
ENTRYPOINT ["java", "-jar", "app.jar"]
