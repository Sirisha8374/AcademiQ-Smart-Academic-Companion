# AcademiQ

A simple app for students to keep track of their academic life: subjects, assignments, notes, attendance, marks, exams and study goals in one place. Built with JavaFX and MySQL.

## Features

- **Accounts**: register and log in; passwords are stored as salted PBKDF2 hashes. Each user only sees their own data.
- **Subjects and assignments**: add, edit and delete; mark as submitted; filter by subject or status; sort by due date; overdue detection.
- **Notes**: attach files to a subject, search by keyword, open or delete them.
- **Attendance**: per-subject totals and percentage, low-attendance warning, and how many classes you can miss (or must attend) to stay above the threshold.
- **Marks**: assessment marks with totals, percentages and configurable grade boundaries.
- **Academic summary**: attendance and marks combined, one row per subject.
- **Exam timetable** and **study planner** with goals, sessions, progress and a study streak.
- **Dashboard**: counters, upcoming deadlines and exams, attendance and marks charts.
- **Reminders**: a background check announces assignments that are due soon.
- **Reports**: export records to CSV.

## Tech stack

- Java 17+
- JavaFX 21
- MySQL 8 (JDBC, `PreparedStatement` everywhere)
- Maven

## Getting started

### Prerequisites

- JDK 17 or newer
- Maven 3.8+
- MySQL server running locally (default port 3306)

### Setup

```bash
git clone https://github.com/Sirisha8374/AcademiQ-Smart-Academic-Companion
cd AcademiQ
cp academiq.properties.example academiq.properties
```

Open `academiq.properties` and set `db.user` and `db.password` to your MySQL login. The `academiq` database and all tables are created automatically on first run.

### Run

```bash
mvn javafx:run
```

The login window opens. Click "Create account", register, then log in.

To load some sample data, register an account first, put its email in `database/sample_data.sql`, and run:

```bash
mysql -u root -p academiq < database/sample_data.sql
```

### Running from an IDE

Open the folder as a Maven project and run `com.academiq.Main`.

## Configuration

Settings live in `academiq.properties` (see `academiq.properties.example`):

| Key | Meaning |
|---|---|
| `db.url`, `db.user`, `db.password` | MySQL connection |
| `attendance.threshold` | Required attendance percentage |
| `reminder.hoursBefore` | Remind about assignments due within this many hours |
| `reminder.checkSeconds` | How often the reminder check runs |
| `files.dir` | Where uploaded notes are copied |
| `grade.<name>` | Minimum percentage for each grade (optional) |

## Project structure

```
src/main/java/com/academiq/
├── Main.java               entry point
├── ModuleRegistry.java     list of screens shown in the sidebar
├── common/                 config, DB connection, session, validators, shared UI helpers
├── account/                login, profile, subjects, assignments
├── records/                notes, attendance, marks, academic summary
└── planner/                dashboard, exams, study planner, reminders, reports
src/main/resources/sql/     table creation scripts
database/sample_data.sql    optional demo data
```
## Demo video

[Watch the project demo](https://drive.google.com/file/d/1Y3Cp-3_5vRqHS_rX5Ni0Bdai4ohRkRvq/view?usp=sharing)
## Screenshots

### Dashboard
![Dashboard](screenshots/dashboard.jpeg)

### Assignments
![Assignments](screenshots/assignments.jpeg)

### Academic summary
![Academic summary](screenshots/summary.jpeg)

### Study planner
![Study planner](screenshots/study-planner.jpeg)

Each feature package follows the same layering: `ui` (JavaFX screens) -> `service` (rules and validation) -> `dao` (SQL) -> MySQL, with plain classes in `model`.

Screens are registered by class name in `ModuleRegistry` and loaded by `ModuleHost`, so adding a new screen means writing a class that implements `ModulePanel` and adding one line to the registry.
