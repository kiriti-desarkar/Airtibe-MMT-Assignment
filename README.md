# Airtibe-MMT-Assignment Solution

## Project Brief
Bookmyshow is a ticketing platform where you can book tickets for a movie show. The image attached represents that for a given theatre we can see the next 7 dates. As one chooses the date, we get list of all shows running in that theatre along with the show timings.

P1 - As part of this assignment, we need to list down all the entities, their attributes and the table structures for the scenario mentioned in the previous slide. You also need to write the SQL queries required to create these tables along with few sample entries. Ensure the tables follow 1NF, 2NF, 3NF and BCNF rules.

1. Tables and Attributes

The database follows BCNF normalization rules, separating the entities to prevent data redundancy.

**Theatres**: Stores the physical cinema locations.

* theatre_id (INT, Primary Key)

* name (VARCHAR)

**Movies**: Stores details specific to the films.

* movie_id (INT, Primary Key)

* title (VARCHAR)

* certification (VARCHAR) - e.g., UA, A

* language (VARCHAR)

* format (VARCHAR) - e.g., 2D, 3D

**Shows**: The junction table mapping movies to theatres at specific times, including the technical screening details.

* show_id (INT, Primary Key)

* theatre_id (INT, Foreign Key referencing Theatres)

* movie_id (INT, Foreign Key referencing Movies)

* show_date (DATE)

* show_time (TIME)

* audio_specs (VARCHAR)

subtitle_lang (VARCHAR)
