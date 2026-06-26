# G-School Backend

API REST du projet **G-School**.

## Stack technique

- **Java 21**
- **Spring Boot 3.5** (Spring Web, Spring Data JPA, Spring Security, Validation)
- **Maven** (avec Maven Wrapper)
- **PostgreSQL** (base de données, via Docker en local)
- **Lombok**
- **JWT** — prévu ultérieurement (non implémenté)

## Architecture des packages

Package racine : `com.gschool.backend`

```
src/main/java/com/gschool/backend/
├── config/    # Configuration transverse (sécurité, etc.)
├── auth/      # Authentification / JWT (à venir)
├── user/      # Domaine utilisateur (à venir)
├── course/    # Domaine cours (à venir)
├── common/    # Éléments partagés (health-check, utilitaires)
└── GschoolBackendApplication.java
```

## Base de données (PostgreSQL via Docker)

Une instance PostgreSQL locale est fournie via [docker-compose.yml](docker-compose.yml).
Les identifiants par défaut sont des **valeurs de développement local non
sensibles** (surchargeables par variables d'environnement).

| Élément          | Valeur par défaut |
|------------------|-------------------|
| Base de données  | `gschool_db`      |
| Utilisateur      | `gschool_user`    |
| Mot de passe     | `gschool_dev`     |
| Port             | `5432`            |

```bash
# Démarrer PostgreSQL en arrière-plan
docker compose up -d

# Vérifier l'état
docker compose ps

# Arrêter
docker compose down

# Arrêter et supprimer les données
docker compose down -v
```

## Configuration

La connexion à la base est configurée via des **variables d'environnement**
(aucun secret de production n'est versionné). Valeurs par défaut dans
[application.yml](src/main/resources/application.yml), alignées sur le
`docker-compose.yml` :

| Variable                | Défaut                                        |
|-------------------------|-----------------------------------------------|
| `DB_URL`                | `jdbc:postgresql://localhost:5432/gschool_db` |
| `DB_USERNAME`           | `gschool_user`                                |
| `DB_PASSWORD`           | `gschool_dev`                                 |
| `SERVER_PORT`           | `8080`                                        |
| `CORS_ALLOWED_ORIGINS`  | `http://localhost:4200`                       |

Exemple de surcharge :

```bash
export DB_PASSWORD=mon_mot_de_passe
```

> Les **tests** utilisent une base H2 en mémoire et ne nécessitent ni Docker ni
> PostgreSQL.

## Lancer le projet

```bash
# 1. Démarrer PostgreSQL (voir section ci-dessus)
docker compose up -d

# 2. Démarrer l'application
./mvnw spring-boot:run

# Compiler
./mvnw clean compile

# Lancer les tests
./mvnw test

# Construire le jar
./mvnw clean package
```

## Endpoint de test

```http
GET /api/health
```

Réponse :

```json
{
  "status": "UP",
  "app": "G-School Backend"
}
```

Vérification rapide une fois l'application lancée :

```bash
curl http://localhost:8080/api/health
```
