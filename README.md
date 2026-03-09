# SportsPulse — Athlete Intelligence Platform
### Java / Spring Boot conversion of the original HTML single-page application

---

## Project Structure

```
sportspulse/
├── pom.xml
└── src/main/
    ├── java/com/sportspulse/
    │   ├── SportsPulseApplication.java          ← Entry point
    │   ├── model/
    │   │   ├── Player.java                      ← Athlete data model
    │   │   ├── TrainingSession.java             ← Weekly training plan entry
    │   │   ├── ActivityFeedItem.java            ← Live feed entry
    │   │   └── AthleteStats.java                ← Radar chart stat model
    │   ├── service/
    │   │   ├── DataService.java                 ← In-memory data store (replaces JS arrays)
    │   │   └── SecurityConfig.java              ← Spring Security / role-based login
    │   └── controller/
    │       ├── DashboardController.java         ← All page routes (GET)
    │       └── ApiController.java               ← REST JSON endpoints (live KPI polling)
    └── resources/
        ├── application.properties
        ├── templates/
        │   ├── login.html                       ← Login page
        │   ├── fragments/layout.html            ← Reusable sidebar + topbar
        │   ├── overview.html                    ← Coach/Analyst overview
        │   ├── athletes.html                    ← Athletes grid + filter
        │   ├── analytics.html                   ← Analytics charts + heatmap
        │   ├── training.html                    ← Weekly training plan
        │   ├── injuries.html                    ← Injury risk monitor
        │   ├── settings.html                    ← Settings page
        │   ├── athlete-perf.html                ← Athlete performance + radar
        │   ├── athlete-training.html            ← Athlete training plan
        │   ├── athlete-progress.html            ← Athlete 6-month progress
        │   └── athlete-profile.html             ← Athlete editable profile
        └── static/
            ├── css/main.css                     ← All styles (extracted from original HTML)
            └── js/app.js                        ← All client-side JS
```

---

## How to Run

**Prerequisites:** Java 17+, Maven 3.6+

```bash
cd sportspulse
mvn spring-boot:run
```

Open **http://localhost:8080** in your browser.

---

## Demo Credentials

| Username | Password  | Role    | Access                            |
|----------|-----------|---------|-----------------------------------|
| coach    | pulse123  | Coach   | All pages                         |
| analyst  | pulse123  | Analyst | All pages                         |
| athlete  | pulse123  | Athlete | My Performance, Training, Progress, Profile |

---

## Architecture: HTML → Java Mapping

| Original JS / HTML                  | Java Equivalent                             |
|--------------------------------------|---------------------------------------------|
| `const PLAYERS = [...]`             | `DataService.getPlayers()`                  |
| `const ACTIVITIES = [...]`          | `DataService.getActivities()`               |
| `const TRAINING_PLAN = [...]`       | `DataService.getTrainingPlan()`             |
| `const USERS = {...}`               | `SecurityConfig.userDetailsService()`       |
| `doLogin()` / `doLogout()`          | Spring Security form login / logout         |
| `switchPage(name, el)`              | GET routes in `DashboardController`         |
| `buildDashboard()` / `buildChart()` | Thymeleaf templates + `app.js`              |
| `startLiveUpdates()` setInterval    | `GET /api/live-kpis` polled every 2.5s      |
| `assignRest()` onclick alert        | `POST /api/injuries/assign-rest/{name}`     |
| `drawRadar()` Canvas API            | Same function in `app.js`                   |
| `localStorage` persistence          | Spring Security session (in-memory)         |

---

## REST API Endpoints

| Method | URL                                   | Description              |
|--------|---------------------------------------|--------------------------|
| GET    | `/api/live-kpis`                      | Live randomised KPI data |
| GET    | `/api/players?position=all`           | Full player list         |
| GET    | `/api/players/top`                    | Top 5 performers         |
| POST   | `/api/injuries/assign-rest/{name}`    | Assign rest protocol     |
| GET    | `/api/analytics/summary`             | Analytics KPI values     |
| GET    | `/api/team/fitness`                   | Team fitness scores      |
| GET    | `/api/training/plan`                  | Weekly training plan     |
| GET    | `/api/activities`                     | Activity feed items      |

---

## Extending to a Real Database

Replace `DataService` with Spring Data JPA repositories:

```java
// Example
@Repository
public interface PlayerRepository extends JpaRepository<Player, Long> {
    List<Player> findByPositionIgnoreCase(String position);
}
```

Add the JPA dependency and a datasource in `application.properties`:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/sportspulse
spring.datasource.username=postgres
spring.datasource.password=secret
spring.jpa.hibernate.ddl-auto=update
```
