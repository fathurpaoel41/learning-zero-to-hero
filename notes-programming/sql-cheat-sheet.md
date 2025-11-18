# SQL Cheat Sheet

## Quick Reference untuk MySQL/SQL

### DDL (Data Definition Language)

#### CREATE DATABASE
```sql
CREATE DATABASE belajar;
USE belajar;
```

#### CREATE TABLE
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### ALTER TABLE
```sql
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users MODIFY COLUMN name VARCHAR(150);
ALTER TABLE users DROP COLUMN phone;
```

#### DROP TABLE
```sql
DROP TABLE users;
```

### DML (Data Manipulation Language)

#### INSERT
```sql
INSERT INTO users (name, email, age) 
VALUES ('Budi', 'budi@example.com', 20);

INSERT INTO users (name, email, age) 
VALUES 
    ('Budi', 'budi@example.com', 20),
    ('Siti', 'siti@example.com', 21);
```

#### SELECT
```sql
SELECT * FROM users;
SELECT name, email FROM users;
SELECT * FROM users WHERE age > 18;
SELECT * FROM users ORDER BY name ASC;
SELECT * FROM users LIMIT 10;
```

#### UPDATE
```sql
UPDATE users 
SET name = 'Budi Updated', age = 21 
WHERE id = 1;
```

#### DELETE
```sql
DELETE FROM users WHERE id = 1;
DELETE FROM users WHERE age < 18;
```

### WHERE Clause
```sql
-- Comparison
WHERE age = 20
WHERE age != 20
WHERE age > 18
WHERE age < 30
WHERE age >= 18
WHERE age <= 25

-- BETWEEN
WHERE age BETWEEN 18 AND 25

-- IN
WHERE age IN (20, 21, 22)

-- LIKE
WHERE name LIKE 'B%'        -- Dimulai dengan B
WHERE name LIKE '%o'         -- Berakhiran o
WHERE name LIKE '%Santoso%'  -- Mengandung Santoso

-- NULL
WHERE email IS NULL
WHERE email IS NOT NULL

-- AND, OR, NOT
WHERE age > 18 AND age < 30
WHERE age < 18 OR age > 65
WHERE NOT age = 20
```

### JOIN
```sql
-- INNER JOIN
SELECT u.name, p.title
FROM users u
INNER JOIN posts p ON u.id = p.user_id;

-- LEFT JOIN
SELECT u.name, p.title
FROM users u
LEFT JOIN posts p ON u.id = p.user_id;

-- RIGHT JOIN
SELECT u.name, p.title
FROM users u
RIGHT JOIN posts p ON u.id = p.user_id;
```

### Aggregate Functions
```sql
COUNT(*)              -- Jumlah baris
SUM(column)           -- Jumlah nilai
AVG(column)           -- Rata-rata
MIN(column)           -- Minimum
MAX(column)           -- Maximum

-- Contoh
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users;
SELECT MAX(age) FROM users;
```

### GROUP BY
```sql
SELECT department, COUNT(*) as total
FROM employees
GROUP BY department;

SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 5000;
```

### ORDER BY
```sql
ORDER BY name ASC;        -- A-Z
ORDER BY age DESC;        -- Tertua ke termuda
ORDER BY age DESC, name ASC;  -- Multiple columns
```

### LIMIT & OFFSET
```sql
LIMIT 10;                 -- 10 data pertama
LIMIT 10 OFFSET 20;       -- Data 21-30
LIMIT 20, 10;             -- Sama dengan OFFSET 20 LIMIT 10
```

### Subquery
```sql
-- Mahasiswa dengan umur di atas rata-rata
SELECT * FROM students
WHERE age > (SELECT AVG(age) FROM students);

-- Mahasiswa dari jurusan populer
SELECT * FROM students
WHERE department_id IN (
    SELECT id FROM departments 
    WHERE student_count > 100
);
```

### Common Functions
```sql
-- String
LENGTH(name)              -- Panjang string
UPPER(name)               -- Uppercase
LOWER(name)               -- Lowercase
SUBSTRING(name, 1, 5)     -- Substring
CONCAT(first, ' ', last)   -- Concatenate
REPLACE(name, 'old', 'new') -- Replace

-- Date
NOW()                     -- Waktu sekarang
CURDATE()                 -- Tanggal sekarang
DATE_FORMAT(date, '%Y-%m-%d') -- Format tanggal
DATEDIFF(date1, date2)   -- Selisih hari

-- Math
ROUND(price, 2)           -- Pembulatan
CEIL(price)               -- Pembulatan ke atas
FLOOR(price)              -- Pembulatan ke bawah
ABS(price)                -- Nilai absolut
```

### Index
```sql
CREATE INDEX idx_email ON users(email);
CREATE UNIQUE INDEX idx_email ON users(email);
DROP INDEX idx_email ON users;
```

### Constraints
```sql
PRIMARY KEY              -- Primary key
FOREIGN KEY              -- Foreign key
NOT NULL                 -- Tidak boleh NULL
UNIQUE                   -- Harus unik
DEFAULT value            -- Nilai default
AUTO_INCREMENT           -- Auto increment
CHECK (condition)        -- Check constraint
```

