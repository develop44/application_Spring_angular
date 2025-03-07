### STAGE 1: Build ###
FROM openjdk:17-alpine AS build
WORKDIR /app

# Copier les fichiers Maven pour télécharger les dépendances en premier
COPY .mvn/ .mvn
COPY mvnw pom.xml ./
RUN chmod +x mvnw
RUN ./mvnw dependency:go-offline

# Copier le reste du code source et construire l'application
COPY src ./src 
RUN ./mvnw -B package -DskipTests

### STAGE 2: Run ###
FROM openjdk:17-alpine
COPY --from=build /app/target/*.jar /spring-back.jar        
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/spring-back.jar"]
