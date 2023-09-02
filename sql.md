# Mastering SQL Queries: From Basic to Advanced

SQL, or Structured Query Language, is a primary language used to interact with databases. This guide will walk you
through the basics and lead up to advanced SQL queries.

## Prerequisites: Installing SQLite3

To start with, we need to set up SQLite3, a lightweight, fast, and standalone SQL database engine.

- **Linux**
  ```bash
  sudo apt-get update
  sudo apt-get install sqlite3
  ```

- **Windows**
    1. Visit the [SQLite Downloads page](https://www.sqlite.org/download.html).
    2. Download the `sqlite-tools-win32-x86-XXXXXXX.zip` or `sqlite-tools-win64-x64-XXXXXXX.zip`file.
    3. Unzip the files and place them in a directory you prefer.
    4. Add the directory to your system PATH to access SQLite from any command prompt.

- **Mac**
  ```bash
  brew install sqlite3
  ```

## Creating our Sample Database and Tables

Now that we've installed SQLite3, let's create our sample database:

```sql
sqlite3 sampleDB.db
```

We will design a database for a fictional book store. It'll include tables for books, authors, and genres. The 'books'
table will have foreign keys linking to 'authors' and 'genres'.

```sql
CREATE TABLE authors
(
    author_id INTEGER PRIMARY KEY,
    name      TEXT NOT NULL
);
```

```sql
CREATE TABLE genres
(
    genre_id   INTEGER PRIMARY KEY,
    genre_name TEXT NOT NULL
);
```

```sql
CREATE TABLE books
(
    book_id   INTEGER PRIMARY KEY,
    title     TEXT NOT NULL,
    author_id INTEGER,
    genre_id  INTEGER,
    FOREIGN KEY (author_id) REFERENCES authors (author_id),
    FOREIGN KEY (genre_id) REFERENCES genres (genre_id)
);
```

## SQL Queries: Basics to Advanced

### INSERT: Add data into tables.

```sql
INSERT INTO authors (name)
VALUES ('George Orwell');
```

```sql
INSERT INTO genres (genre_name)
VALUES ('Dystopian');
```

```sql
INSERT INTO books (title, author_id, genre_id)
VALUES ('1984', 1, 1);
```

This inserts George Orwell as an author, 'Dystopian' as a genre, and '1984' as a book written by George Orwell in the
Dystopian genre.

**After running the above commands, copy and paste the commands from [populate_db](populate_db.md) to populate the
database**

### SELECT: Fetch data.

```sql
SELECT *
FROM books;
```

This fetches all records from the 'books' table.

### JOIN: Combining rows from two or more tables.

```sql
SELECT books.title, authors.name, genres.genre_name
FROM books
         JOIN authors ON books.author_id = authors.author_id
         JOIN genres ON books.genre_id = genres.genre_id;
```

This fetches the book titles along with their authors and genres.

### UPDATE: Modify existing data.

```sql
UPDATE books
SET title = 'Animal Farm'
WHERE book_id = 1;
```

This updates the title of the book with `book_id = 1` to 'Animal Farm'. (1984 -> Animal Farm)

### DELETE: Remove data.

```sql
DELETE
FROM books
WHERE title = 'Animal Farm';
```

This deletes the book titled 'Animal Farm'. (If you run SELECT * FROM books; after this, you'll see that the book is no
longer there.)

### Complex Queries: Filtering using `WHERE` and ordering using `ORDER BY`.

```sql
SELECT *
FROM books
WHERE author_id = 1
ORDER BY title ASC;
```

This fetches all books written by the author with `author_id = 1`, ordering them by their titles in ascending order.

### Aggregate Functions: Functions like `COUNT`, `SUM`, `AVG`.

```sql
SELECT COUNT(*)
FROM books
WHERE author_id = 1;
```

This counts the number of books written by the author with `author_id = 1`.

## Wildcards

Wildcards are special characters used with the `LIKE` operator to perform pattern matching. The two wildcards used in
SQL are:

- `%`: Matches zero, one, or multiple characters.
- `_`: Matches exactly one character.

### Using `%`** Wildcard

-
    1. Find all books with titles that start with 'The':

```sql
SELECT *
FROM books
WHERE title LIKE 'The%';
```

-
    2. Finding all books with titles that end with '1984':

```sql
SELECT *
FROM books
WHERE title LIKE '%1984';
```

-
    3. Finding all books with titles that contain the word "War" anywhere:

```sql
SELECT *
FROM books
WHERE title LIKE '%War%';
```

-
  4. Finding all authors whose names start with "J" and end with "n":

```sql
SELECT *
FROM authors
WHERE name LIKE 'J%n';
```

**Using `_` Wildcard**

-
    1. Finding all books with titles that have exactly 5 characters:

```sql
SELECT *
FROM books
WHERE title LIKE '_____';
```

-
    2. Finding all books with titles that have any character followed by "1984":

```sql
SELECT *
FROM books
WHERE title LIKE '_________1984';
```

### Combining `%` and `_` Wildcards

-
    1. Finding all books with titles that have "The" followed by any character followed by "1984":

```sql
SELECT *
FROM books
WHERE title LIKE 'The_1984%';
```

-
    2. Finding all books with titles that start with "War" followed by a space with the second word beginning with "T":

```sql
SELECT *
FROM books
WHERE title LIKE 'War_T%';
```