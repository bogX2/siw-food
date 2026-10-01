# SiwFood

**Recipe-sharing web app** where chefs publish their recipes with ingredients, quantities and photos. Built with **Spring Boot**, **Spring Security**, **Thymeleaf** and **PostgreSQL**.
Individual project for the *Sistemi Informativi su Web* (Web Information Systems) course, Bachelor's degree in Computer Engineering, Roma Tre University. The specification was provided by the course.

Visitors can browse and search recipes, chefs and ingredients. Registered chefs manage their own recipes, and an administrator manages the whole catalogue.

---

## Features

**Visitors (no login)**
- Browse **recipes**, **chefs** and **ingredients**, each with its own detail page
- **Search** recipes, chefs and ingredients by name
- A recipe page shows the photo, description, author and the ingredient list with quantities and units

**Chefs (registered users)**
- Sign-up creates both the account and the chef profile (name, surname, date of birth, biography, photo)
- Personal dashboard to **create, edit and delete their own recipes**
- Upload a **recipe photo**, add ingredients with **quantity and unit**, or create a new ingredient if it does not exist yet

**Administrator**
- Full control over **chefs, recipes and ingredients**: create, update, delete
- Assign a recipe to a chef and manage the ingredient list of any recipe

**Validation**
- Custom validators prevent **duplicate** usernames, emails, chefs, recipes and ingredients
- Bean validation on required fields and non-negative quantities

## Tech stack

| Layer | Technology |
|---|---|
| Backend | Java 17, Spring Boot 3, Spring MVC |
| Security | Spring Security: form login, role-based access (`DEFAULT` = chef, `ADMIN`), BCrypt password hashing |
| Persistence | Spring Data JPA, Hibernate, PostgreSQL |
| Frontend | Thymeleaf, Bootstrap 5, custom CSS |
| Images | Uploaded as `MultipartFile` and stored in the database as Base64 text |
| Build | Maven (wrapper included) |

## Data model

```
Cuoco (Chef) 1 ──── * Ricetta (Recipe) 1 ──── * LineaIngrediente (Ingredient line) * ──── 1 Ingrediente (Ingredient)
   │                                                    quantity + unit
   1
User 1 ── 1 Credentials (username, hashed password, role)
```

A **`LineaIngrediente`** is the join entity between a recipe and an ingredient. It stores the **quantity and unit** (for example *200 g flour*), so the same ingredient can be reused across recipes with different amounts.

## Project structure

```
src/main/java/it/uniroma3/siw/
├── model/           # JPA entities: Cuoco, Ricetta, Ingrediente, LineaIngrediente, User, Credentials
├── repository/      # Spring Data repositories
├── service/         # business logic
├── controller/      # MVC controllers + validator/ (custom validators)
└── authentication/  # Spring Security configuration
src/main/resources/
├── templates/       # public pages, cuoco/ (chef area), admin/ (admin area)
├── static/          # CSS and images
└── application.properties
```

## Getting started

Requirements: **Java 17+** and **PostgreSQL**.

1. **Create the database**
   ```sql
   CREATE DATABASE siwfood2;
   ```

2. **Set the database credentials** (defaults: user `postgres`, password `postgres`, database on `localhost:5432`)
   ```bash
   # Linux / macOS
   export DB_USERNAME=postgres
   export DB_PASSWORD=your_password
   # Windows (PowerShell)
   $env:DB_USERNAME="postgres"; $env:DB_PASSWORD="your_password"
   ```

3. **Run the app**
   ```bash
   ./mvnw spring-boot:run        # Windows: mvnw.cmd spring-boot:run
   ```
   Open http://localhost:8080. The tables are created automatically.

4. **Create an administrator**: register a user from the site, then promote it:
   ```sql
   UPDATE credentials SET role = 'ADMIN' WHERE username = 'your_username';
   ```

## Author

**Bogdan Andrei Tutuianu** – [GitHub](https://github.com/bogX2)
