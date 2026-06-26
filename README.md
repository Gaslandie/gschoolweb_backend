# G-School Backend

API REST du projet **G-School**.

## Stack technique

- **Java 21**
- **Spring Boot 3.3** (Spring Web, Spring Data JPA, Spring Security, Validation)
- **Maven** (avec Maven Wrapper)
- **PostgreSQL** (base de données)
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

## Configuration

La base de données est configurée via des **variables d'environnement** (aucun
secret n'est versionné). Valeurs par défaut dans
[application.yml](src/main/resources/application.yml) :

| Variable      | Défaut                                         |
|---------------|------------------------------------------------|
| `DB_URL`      | `jdbc:postgresql://localhost:5432/gschool`     |
| `DB_USERNAME` | `gschool`                                       |
| `DB_PASSWORD` | `change-me-in-env`                              |
| `SERVER_PORT` | `8080`                                          |

Exemple :

```bash
export DB_URL=jdbc:postgresql://localhost:5432/gschool
export DB_USERNAME=gschool
export DB_PASSWORD=mon_mot_de_passe
```

> Prérequis : une base PostgreSQL `gschool` accessible. Les **tests** utilisent
> une base H2 en mémoire et ne nécessitent pas PostgreSQL.

## Lancer le projet

```bash
# Démarrer l'application
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
