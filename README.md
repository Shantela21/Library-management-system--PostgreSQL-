# 📘 Library Management System (PostgreSQL)

### 🧾 Project Overview
This project implements a **Library Management System** using **PostgreSQL**.  
It manages **books**, **authors**, and **patrons**, allowing a librarian to:
- Add, view, update, and delete records
- Track borrowed books
- Run advanced database queries

All tasks are organized into **sprints**.

---

## 🖥️ Tools Required
- **PostgreSQL** (v14+ recommended)
- **pgAdmin 4** or **psql (PostgreSQL shell)**

---

## ⚙️ Setting Up the Project

### Option 1: Using **pgAdmin**
1. Open **pgAdmin 4**.
2. Connect to your PostgreSQL server.
3. Create a new database:
   - Right-click “Databases” → **Create** → **Database...**
   - Name it **LibraryDB**
4. Open the **Query Tool**.
5. Copy and paste the SQL commands from this README (section by section).
6. Execute them by clicking the ▶️ **Run** button.

### Option 2: Using **psql**
1. Open your terminal.
2. Login to PostgreSQL:
   ```sql
   psql -U postgres



## Sprint 1: Project Setup
### Create a new database
~~~sql
CREATE DATABASE LibraryDB;
~~~
### Create the required tables: Books, Authors, Patrons
#### Create Authors table
~~~sql
CREATE TABLE authors (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    nationality VARCHAR(50),
    birth_year INT,
    death_year INT
);
~~~

#### Create Books table
~~~sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    author_id INT REFERENCES authors(id) ON DELETE CASCADE,
    genres TEXT[],
    published_year INT,
    available BOOLEAN DEFAULT TRUE
);
~~~

#### Create Patrons table
~~~sql
CREATE TABLE patrons (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);
~~~

## Sprint 2: Insert Data
#### Insert sample authors
~~~sql
INSERT INTO authors (id, name, nationality, birth_year, death_year) VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);
~~~

#### Insert sample books
~~~sql
INSERT INTO books (id, title, author_id, genres, published_year, available) VALUES
(1, '1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);
~~~

#### Insert sample patrons
```sql
INSERT INTO patrons (id, name, email, borrowed_books) VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1, 2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4, 5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7, 8]);
```
## Sprint 3: Read Operations (Queries)
#### Get all books
```sql
SELECT * FROM books;
```

#### Get a single book by title
```sql
SELECT * FROM books WHERE title = '1984';
```

#### Get all books by a specific author 
```sql
SELECT b.* FROM books b
JOIN authors a ON b.author_id = a.id
WHERE a.name = 'George Orwell';
```

#### Get all available books
```sql
SELECT * FROM books WHERE available = TRUE;
```

## Sprint 4: Update Operations
#### Mark a book as borrowed (set available = FALSE)
```sql
UPDATE books
SET available = FALSE
WHERE id = 1;
```
#### Add a new genre to an existing book
```sql
UPDATE books
SET genres = array_append(genres, 'Classic')
WHERE title = 'The Great Gatsby';
```
#### Add a borrowed book to a patron’s record
```sql
UPDATE patrons
SET borrowed_books = array_append(borrowed_books, 1)
WHERE name = 'Alice Johnson';
```
## Sprint 5: Delete Operations
#### Delete a book by title
```sql
DELETE FROM books WHERE title = 'The Hobbit';
```
#### Delete an author by ID
```sql
DELETE FROM authors WHERE id = 3;
```
## Sprint 6: Advanced Queries
#### Find books published after 1950
```sql
SELECT * FROM books 
WHERE published_year > 1950;
```
#### Find all American authors
```sql
SELECT * FROM authors
WHERE nationality;
```
#### Set all books as available
```sql
UPDATE books
SET available = true;
```
#### Find all books that are available AND published after 1950
```sql
SELECT * FROM books
WHERE available = true 
AND  published_year > 1950;
```
#### Find authors whose names contain "George"
```sql
SELECT * FROM authors
WHERE name ILIKE ('george%')
```
#### Increment the published year 1869 by 1
```sql
UPDATE books 
SET published_year = 1870
WHERE id = 8;
```


## 📄 Author

**Shantela Noyila**
