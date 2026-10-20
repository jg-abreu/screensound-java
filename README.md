# ScreenSound

![Java 17](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot 4](https://img.shields.io/badge/Spring_Boot-4-6DB33F?logo=springboot&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A command-line music catalog: register artists and their songs in PostgreSQL, search songs by artist and fetch each artist's biography from the Wikipedia API.

## Features

- **Register artists** with a name and type (`SOLO`, `DUO` or `BAND`)
- **Register songs** with title and album, linked to an existing artist
- **List all songs**, grouped by artist
- **Search songs by artist** — case-insensitive, matching part of the name
- **Artist biography** — fetches the artist's summary from the Wikipedia REST API in real time

## How it works

- `Artist` and `Song` are JPA entities with a bidirectional one-to-many relationship (`@OneToMany` / `@ManyToOne`), and saving an artist cascades to its songs.
- The search by artist is a custom JPQL query in `ArtistRepository` using `LOWER(...) LIKE` for partial, case-insensitive matches.
- `WikipediaQueryService` calls `https://en.wikipedia.org/api/rest_v1/page/summary/{name}` with Java's native `HttpClient`.

## Tech stack

Java 17 · Spring Boot 4 (Spring Data JPA, command-line runner) · Hibernate · PostgreSQL · Java `HttpClient` · Maven

## Running

Requirements: Java 17+ and a running PostgreSQL server.

1. Create the database:
   ```sql
   CREATE DATABASE screensound_db;
   ```
   The tables are created by Hibernate on startup (`ddl-auto=update`).

2. Set the connection through environment variables (the defaults are shown in parentheses):

   | Variable | Default |
   |---|---|
   | `DB_HOST` | `localhost:5432` |
   | `DB_USER` | `postgres` |
   | `DB_PASSWORD` | `postgres` |

3. Run the app:
   ```bash
   git clone https://github.com/jg-abreu/screensound-java.git
   cd screensound-java
   ./mvnw spring-boot:run
   ```

4. Use the menu in the terminal:
   ```
   *** Screen Sound Music ***

   1 - Register artists
   2 - Register songs
   3 - List songs
   4 - Search songs by artist
   5 - Search artist details

   9 - Exit
   ```

## Project structure

```
src/main/java/br/com/alura/screensound
├── main/         interactive menu
├── model/        Artist, Song and ArtistType
├── repository/   Spring Data repository with the JPQL query
└── service/      Wikipedia API client
```

## Credits

Built as the solution to a challenge from [Alura](https://www.alura.com.br)'s Java and Spring Data JPA course. The implementation is my own.

## License

Released under the [MIT License](LICENSE).

## Author

**João Abreu** — back-end developer and Computer Science student in Rio de Janeiro, Brazil.

[GitHub](https://github.com/jg-abreu) · [LinkedIn](https://www.linkedin.com/in/jo%C3%A3o-abreuuu) · jgabreudev@gmail.com
