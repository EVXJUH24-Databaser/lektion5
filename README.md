---
author: Lektion 5
date: MMMM dd, YYYY
paging: "%d / %d"
---

# Teams lektion 5

## Agenda

1. Frågor och repetition
2. Genomgång av övningar (1 - 5)
3. Constraints med relationer
4. Repetition och mer om joins
5. Övning med handledning
6. Quiz frågor

---

# Fråga

(från lektion 4, introduktion till relationer och joins)
Det finns ju arrays i postgress, både enkla arrayer samt 2d arrayer. Vad är syftet med dem?

# Svar

De kan användas för enkla värden (string array) men skall undvikas om det går. De är till för undantagsfall.

---

# Exempel med relationer

- E-commerce: produkter och ordrar
- Social media plattform: användare, inlägg, kommentarer
- GitHub: användare, repositories, inställningar

---

# Constraints

- `ON DELETE`
  - `RESTRICT` - Förhindra radering när relationer finns
  - `CASCADE` - Radera alla rader med matchande foreign key
  - `SET NULL` - Radera matchande foreign keys (sätt till NULL)
- `ON UPDATE`
  - `RESTRICT` - Förhindra ändring av primary key
  - `CASCADE` - Uppdaterar foreign key om primary key ändras
  - `SET NULL` - Radera matchande foreign keys (sätt till NULL)

---

# Constraints exempel

```sql
CREATE TABLE departments (
    id SERIAL PRIMARY KEY,
    name TEXT
);

CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name TEXT,
    department_id INT REFERENCES departments(id)
        ON DELETE CASCADE
        ON UPDATE SET NULL
);
```

---

# Repetition joins

- Hämta data från två eller fler tabeller samtidigt
- Används i samband med relationer
- Olika typer av joins för olika situationer

Joins kan användas för att hämta data i en tabell om man endast har information om en annan tabell.

Exempel: du har namnet på en person men vill veta vad namnen på husdjuren är.

---

# Typer av joins

- `INNER JOIN` - Hämtar endast rader med matchande värden
- `LEFT JOIN` - Hämtar alla rader från vänster, och de rader från höger som matchar
- `RIGHT JOIN` - Hämtar alla rader från höger, och de rader från vänster som matchar
- `FULL OUTER JOIN` - Hämtar alla rader från båda tabeller och matchar de som går

---

# Joins exempel

```sql
SELECT 
    employees.name,
    departments.name
FROM employees
INNER JOIN departments ON employees.department_id = departments.id;

SELECT 
    employees.name,
    departments.name
FROM employees
LEFT JOIN departments ON employees.department_id = departments.id;

SELECT 
    employees.name,
    departments.name
FROM employees
FULL OUTER JOIN departments ON employees.department_id = departments.id;
```

---

# Joins exempel (One-to-Many)

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT, password TEXT);
CREATE TABLE comments (id SERIAL PRIMARY KEY, content TEXT, user_id INT REFERENCES users(id));

-- Hämta all kommentarer från ett användarnamn
SELECT content from comments 
INNER JOIN users 
  ON users.id = comments.user_id 
WHERE users.name = 'Ironman';
```

---

# Joins och Many-to-Many

Many-to-Many kräver ofta två eller flera joins.

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT);
CREATE TABLE repos (id SERIAL PRIMARY KEY, name TEXT, public BOOLEAN);
CREATE TABLE user_repos(repo_id INT REFERENCES repos(id), user_id INT REFERENCES users(id));

-- Hämta all repos från ett användarnamn
SELECT repos.name FROM repos 
INNER JOIN user_repos 
  ON repos.id = user_repos.repo_id 
INNER JOIN users 
  ON users.id = user_repos.user_id 
WHERE users.name = 'Ironman';
```

---

# Quiz frågor

- Varför skall man inte lägga in query-argument genom string concatenation?
- Kan en primary key vara av datatypen `INT`?
- Varför är det ovanligt att ha One-to-One relationer?
- Vad är det för skillnad på Many-to-One och One-to-Many relationer?
- Hur implementeras Many-to-Many relationer med tabeller?
- Ge ett exempel på en Many-to-Many relation.
- Vad är det för skillnad på `LEFT JOIN` och `RIGHT JOIN?`
- Vad gör `INNER JOIN`?
- Vad gör `ON DELETE CASCADE`?
- Kan en query innehålla flera joins?
