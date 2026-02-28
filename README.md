# Airtibe-MMT-Assignment Solution

## Project Brief
Bookmyshow is a ticketing platform where you can book tickets for a movie show. The image attached represents that for a given theatre we can see the next 7 dates. As one chooses the date, we get list of movies and their respective show timings.

**P1** - As part of this assignment, we need to list down all the entities, their attributes and the table structures for the scenario mentioned in the previous slide. You also need to write the SQL queries for the given problems.

---

## 1. Tables and Attributes

The database follows BCNF normalization rules, separating the entities to prevent data redundancy.

### Theatres
Stores the physical cinema locations.

| Column | Type | Constraint |
|--------|------|-----------|
| theatre_id | INT | Primary Key |
| name | VARCHAR | NOT NULL |

### Movies
Stores details specific to the films.

| Column | Type | Constraint |
|--------|------|-----------|
| movie_id | INT | Primary Key |
| title | VARCHAR | NOT NULL |
| certification | VARCHAR | - |
| language | VARCHAR | - |
| format | VARCHAR | - |

### Shows
The junction table mapping movies to theatres at specific times, including the technical screening details.

| Column | Type | Constraint |
|--------|------|-----------|
| show_id | INT | Primary Key |
| theatre_id | INT | Foreign Key → Theatres(theatre_id) |
| movie_id | INT | Foreign Key → Movies(movie_id) |
| show_date | DATE | - |
| show_time | TIME | - |
| audio_specs | VARCHAR | - |
| subtitle_lang | VARCHAR | - |

---

## 2. Normalization Check

The schema design adheres to the standard normal forms to ensure data integrity:

**1NF**: All attributes contain atomic (indivisible) values. There are no repeating groups within any of the tables.

**2NF**: All tables utilize a single-column primary key, meaning there are no partial dependencies (where an attribute depends on only part of a composite primary key).

**3NF**: There are no transitive dependencies. For example, in the Movies table, the language attribute depends directly on the primary key movie_id, not on the title.

**BCNF**: For every non-trivial functional dependency X→Y, X is a superkey. Every determinant in these tables is a primary key, strictly fulfilling Boyce-Codd Normal Form requirements.

---

## 3. Example Rows

### Theatres Table

| theatre_id | name |
|---|---|
| 1 | PVR: Nexus (Formerly Forum), Koramangala |

### Movies Table

| movie_id | title | certification | language | format |
|---|---|---|---|---|
| 1 | Dasara | UA | Telugu | 2D |
| 2 | Kisi Ka Bhai Kisi Ki Jaan | UA | Hindi | 2D |

### Shows Table

| show_id | theatre_id | movie_id | show_date | show_time | audio_specs | subtitle_lang |
|---|---|---|---|---|---|---|
| 101 | 1 | 1 | 2023-04-25 | 12:15:00 | 4K Dolby 7.1 | ENG |
| 102 | 1 | 2 | 2023-04-25 | 13:00:00 | 4K ATMOS | ENG |

---

## 4. P1 Solution: Schema Creation and Sample Data (MySQL)

```sql
-- Create Theatres Table
CREATE TABLE Theatres (
    theatre_id INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

-- Create Movies Table
CREATE TABLE Movies (
    movie_id INT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    certification VARCHAR(10),
    language VARCHAR(50),
    format VARCHAR(10)
);

-- Create Shows Table
CREATE TABLE Shows (
    show_id INT PRIMARY KEY,
    theatre_id INT,
    movie_id INT,
    show_date DATE,
    show_time TIME,
    audio_specs VARCHAR(100),
    subtitle_lang VARCHAR(10),
    FOREIGN KEY (theatre_id) REFERENCES Theatres(theatre_id) ON DELETE CASCADE,
    FOREIGN KEY (movie_id) REFERENCES Movies(movie_id) ON DELETE CASCADE
);

-- Insert Sample Data into Theatres
INSERT INTO Theatres (theatre_id, name) VALUES 
(1, 'PVR: Nexus (Formerly Forum), Koramangala');

-- Insert Sample Data into Movies
INSERT INTO Movies (movie_id, title, certification, language, format) VALUES 
(1, 'Dasara', 'UA', 'Telugu', '2D'),
(2, 'Kisi Ka Bhai Kisi Ki Jaan', 'UA', 'Hindi', '2D'),
(3, 'Tu Jhoothi Main Makkaar', 'UA', 'Hindi', '2D'),
(4, 'Avatar: The Way of Water', 'UA', 'English', '3D');

-- Insert Sample Data into Shows (For Date: 2023-04-25)
INSERT INTO Shows (show_id, theatre_id, movie_id, show_date, show_time, audio_specs, subtitle_lang) VALUES 
(101, 1, 1, '2023-04-25', '12:15:00', '4K Dolby 7.1', 'ENG'),
(102, 1, 2, '2023-04-25', '13:00:00', '4K ATMOS', 'ENG'),
(103, 1, 2, '2023-04-25', '16:10:00', '4K ATMOS', 'ENG'),
(104, 1, 2, '2023-04-25', '18:20:00', '4K Dolby 7.1', 'ENG'),
(105, 1, 2, '2023-04-25', '19:20:00', '4K ATMOS', 'ENG'),
(106, 1, 2, '2023-04-25', '22:30:00', '4K ATMOS', 'ENG'),
(107, 1, 3, '2023-04-25', '13:15:00', 'Dolby 7.1', 'ENG'),
(108, 1, 4, '2023-04-25', '13:20:00', 'Playhouse 4K', 'ENG');
'''

P2 - Write a query to list down all the shows on a given date at a given theatre along with their respective show timings.

5. P2 Solution: Query to List Shows (MySQL)
'''SQL
-- Query to list all shows on a given date at a given theatre along with timings
SELECT 
    m.title AS `Movie Title`,
    m.language AS `Language`,
    m.format AS `Format`,
    s.show_time AS `Show Timing`,
    s.audio_specs AS `Audio/Video Specs`,
    s.subtitle_lang AS `Subtitles`
FROM Shows s
JOIN Movies m ON s.movie_id = m.movie_id
JOIN Theatres t ON s.theatre_id = t.theatre_id
WHERE t.name = 'PVR: Nexus (Formerly Forum), Koramangala' 
  AND s.show_date = '2023-04-25'
ORDER BY m.title, s.show_time;
'''
