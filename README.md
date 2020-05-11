# MySQL_String_Functions

[MySQL_String_Function_Doc](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)

[SQL-Formatter](https://sql-format.com/)

## Book Data (book_data.sql) and SQL Code to create TABLE
```
CREATE TABLE books 
	(
		book_id INT NOT NULL AUTO_INCREMENT,
		title VARCHAR(100),
		author_fname VARCHAR(100),
		author_lname VARCHAR(100),
		released_year INT,
		stock_quantity INT,
		pages INT,
		PRIMARY KEY(book_id)
	);

INSERT INTO books (title, author_fname, author_lname, released_year, stock_quantity, pages)
VALUES
('The Namesake', 'Jhumpa', 'Lahiri', 2003, 32, 291),
('Norse Mythology', 'Neil', 'Gaiman',2016, 43, 304),
('American Gods', 'Neil', 'Gaiman', 2001, 12, 465),
('Interpreter of Maladies', 'Jhumpa', 'Lahiri', 1996, 97, 198),
('A Hologram for the King: A Novel', 'Dave', 'Eggers', 2012, 154, 352),
('The Circle', 'Dave', 'Eggers', 2013, 26, 504),
('The Amazing Adventures of Kavalier & Clay', 'Michael', 'Chabon', 2000, 68, 634),
('Just Kids', 'Patti', 'Smith', 2010, 55, 304),
('A Heartbreaking Work of Staggering Genius', 'Dave', 'Eggers', 2001, 104, 437),
('Coraline', 'Neil', 'Gaiman', 2003, 100, 208),
('What We Talk About When We Talk About Love: Stories', 'Raymond', 'Carver', 1981, 23, 176),
("Where I'm Calling From: Selected Stories", 'Raymond', 'Carver', 1989, 12, 526),
('White Noise', 'Don', 'DeLillo', 1985, 49, 320),
('Cannery Row', 'John', 'Steinbeck', 1945, 95, 181),
('Oblivion: Stories', 'David', 'Foster Wallace', 2004, 172, 329),
('Consider the Lobster', 'David', 'Foster Wallace', 2005, 92, 343); 
```

## What if I want full names?, CONCAT, CONCAT_WS: WS measn "with separator"
```
mysql> SELECT author_fname, author_lname FROM books;
+--------------+----------------+
| author_fname | author_lname   |
+--------------+----------------+
| Jhumpa       | Lahiri         |
| Neil         | Gaiman         |
| Neil         | Gaiman         |
| Jhumpa       | Lahiri         |
| Dave         | Eggers         |
| Dave         | Eggers         |
| Michael      | Chabon         |
| Patti        | Smith          |
| Dave         | Eggers         |
| Neil         | Gaiman         |
| Raymond      | Carver         |
| Raymond      | Carver         |
| Don          | DeLillo        |
| John         | Steinbeck      |
| David        | Foster Wallace |
| David        | Foster Wallace |
+--------------+----------------+
16 rows in set (0.01 sec)


mysql> SELECT
    ->   CONCAT(author_fname, ' ', author_lname)
    -> FROM books;
+-----------------------------------------+
| CONCAT(author_fname, ' ', author_lname) |
+-----------------------------------------+
| Jhumpa Lahiri                           |
| Neil Gaiman                             |
| Neil Gaiman                             |
| Jhumpa Lahiri                           |
| Dave Eggers                             |
| Dave Eggers                             |
| Michael Chabon                          |
| Patti Smith                             |
| Dave Eggers                             |
| Neil Gaiman                             |
| Raymond Carver                          |
| Raymond Carver                          |
| Don DeLillo                             |
| John Steinbeck                          |
| David Foster Wallace                    |
| David Foster Wallace                    |
+-----------------------------------------+
16 rows in set (0.00 sec)


mysql> SELECT
    ->   CONCAT(author_fname, ' ', author_lname)
    ->   AS 'full name'
    -> FROM books;
+----------------------+
| full name            |
+----------------------+
| Jhumpa Lahiri        |
| Neil Gaiman          |
| Neil Gaiman          |
| Jhumpa Lahiri        |
| Dave Eggers          |
| Dave Eggers          |
| Michael Chabon       |
| Patti Smith          |
| Dave Eggers          |
| Neil Gaiman          |
| Raymond Carver       |
| Raymond Carver       |
| Don DeLillo          |
| John Steinbeck       |
| David Foster Wallace |
| David Foster Wallace |
+----------------------+
16 rows in set (0.00 sec)

mysql> SELECT author_fname AS first, author_lname AS last,
    ->   CONCAT(author_fname, ' ', author_lname) AS full
    -> FROM books;
+---------+----------------+----------------------+
| first   | last           | full                 |
+---------+----------------+----------------------+
| Jhumpa  | Lahiri         | Jhumpa Lahiri        |
| Neil    | Gaiman         | Neil Gaiman          |
| Neil    | Gaiman         | Neil Gaiman          |
| Jhumpa  | Lahiri         | Jhumpa Lahiri        |
| Dave    | Eggers         | Dave Eggers          |
| Dave    | Eggers         | Dave Eggers          |
| Michael | Chabon         | Michael Chabon       |
| Patti   | Smith          | Patti Smith          |
| Dave    | Eggers         | Dave Eggers          |
| Neil    | Gaiman         | Neil Gaiman          |
| Raymond | Carver         | Raymond Carver       |
| Raymond | Carver         | Raymond Carver       |
| Don     | DeLillo        | Don DeLillo          |
| John    | Steinbeck      | John Steinbeck       |
| David   | Foster Wallace | David Foster Wallace |
| David   | Foster Wallace | David Foster Wallace |
+---------+----------------+----------------------+
16 rows in set (0.00 sec)

mysql> SELECT CONCAT(title, '-', author_fname, '-', author_lname) FROM books;
+--------------------------------------------------------------------+
| CONCAT(title, '-', author_fname, '-', author_lname)                |
+--------------------------------------------------------------------+
| The Namesake-Jhumpa-Lahiri                                         |
| Norse Mythology-Neil-Gaiman                                        |
| American Gods-Neil-Gaiman                                          |
| Interpreter of Maladies-Jhumpa-Lahiri                              |
| A Hologram for the King: A Novel-Dave-Eggers                       |
| The Circle-Dave-Eggers                                             |
| The Amazing Adventures of Kavalier & Clay-Michael-Chabon           |
| Just Kids-Patti-Smith                                              |
| A Heartbreaking Work of Staggering Genius-Dave-Eggers              |
| Coraline-Neil-Gaiman                                               |
| What We Talk About When We Talk About Love: Stories-Raymond-Carver |
| Where I'm Calling From: Selected Stories-Raymond-Carver            |
| White Noise-Don-DeLillo                                            |
| Cannery Row-John-Steinbeck                                         |
| Oblivion: Stories-David-Foster Wallace                             |
| Consider the Lobster-David-Foster Wallace                          |
+--------------------------------------------------------------------+
16 rows in set (0.00 sec)

mysql> SELECT
    ->     CONCAT_WS(' - ', title, author_fname, author_lname)
    -> FROM books;
+------------------------------------------------------------------------+
| CONCAT_WS(' - ', title, author_fname, author_lname)                    |
+------------------------------------------------------------------------+
| The Namesake - Jhumpa - Lahiri                                         |
| Norse Mythology - Neil - Gaiman                                        |
| American Gods - Neil - Gaiman                                          |
| Interpreter of Maladies - Jhumpa - Lahiri                              |
| A Hologram for the King: A Novel - Dave - Eggers                       |
| The Circle - Dave - Eggers                                             |
| The Amazing Adventures of Kavalier & Clay - Michael - Chabon           |
| Just Kids - Patti - Smith                                              |
| A Heartbreaking Work of Staggering Genius - Dave - Eggers              |
| Coraline - Neil - Gaiman                                               |
| What We Talk About When We Talk About Love: Stories - Raymond - Carver |
| Where I'm Calling From: Selected Stories - Raymond - Carver            |
| White Noise - Don - DeLillo                                            |
| Cannery Row - John - Steinbeck                                         |
| Oblivion: Stories - David - Foster Wallace                             |
| Consider the Lobster - David - Foster Wallace                          |
+------------------------------------------------------------------------+
16 rows in set (0.01 sec)
```

## SUBSTRING
```

mysql> SELECT SUBSTRING('Hello World', 1, 4);
+--------------------------------+
| SUBSTRING('Hello World', 1, 4) |
+--------------------------------+
| Hell                           |
+--------------------------------+
1 row in set (0.02 sec)

mysql> SELECT SUBSTRING('Hello World', 7);
+-----------------------------+
| SUBSTRING('Hello World', 7) |
+-----------------------------+
| World                       |
+-----------------------------+
1 row in set (0.01 sec)

mysql> SELECT SUBSTRING('Hello World', -3);
+------------------------------+
| SUBSTRING('Hello World', -3) |
+------------------------------+
| rld                          |
+------------------------------+
1 row in set (0.08 sec)

mysql> SELECT SUBSTRING(title, 1, 10) AS 'short title' FROM books;
+-------------+
| short title |
+-------------+
| The Namesa  |
| Norse Myth  |
| American G  |
| Interprete  |
| A Hologram  |
| The Circle  |
| The Amazin  |
| Just Kids   |
| A Heartbre  |
| Coraline    |
| What We Ta  |
| Where I'm   |
| White Nois  |
| Cannery Ro  |
| Oblivion:   |
| Consider t  |
+-------------+
16 rows in set (0.08 sec)

mysql> SELECT CONCAT
    ->     (
    ->         SUBSTRING(title, 1, 10),
    ->         '...'
    ->     ) AS 'short title'
    -> FROM books;
+---------------+
| short title   |
+---------------+
| The Namesa... |
| Norse Myth... |
| American G... |
| Interprete... |
| A Hologram... |
| The Circle... |
| The Amazin... |
| Just Kids...  |
| A Heartbre... |
| Coraline...   |
| What We Ta... |
| Where I'm ... |
| White Nois... |
| Cannery Ro... |
| Oblivion: ... |
| Consider t... |
+---------------+
16 rows in set (0.00 sec)
```
## REPLACE
```
mysql> SELECT REPLACE('HellO World', 'o', '*');
+----------------------------------+
| REPLACE('HellO World', 'o', '*') |
+----------------------------------+
| HellO W*rld                      |
+----------------------------------+
1 row in set (0.00 sec)

mysql> SELECT
    ->   REPLACE('cheese bread coffee milk', ' ', ' and ');
+---------------------------------------------------+
| REPLACE('cheese bread coffee milk', ' ', ' and ') |
+---------------------------------------------------+
| cheese and bread and coffee and milk              |
+---------------------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT REPLACE(title, 'e ', '3') FROM books;
+---------------------------------------------------+
| REPLACE(title, 'e ', '3')                         |
+---------------------------------------------------+
| Th3Namesake                                       |
| Nors3Mythology                                    |
| American Gods                                     |
| Interpreter of Maladies                           |
| A Hologram for th3King: A Novel                   |
| Th3Circle                                         |
| Th3Amazing Adventures of Kavalier & Clay          |
| Just Kids                                         |
| A Heartbreaking Work of Staggering Genius         |
| Coraline                                          |
| What W3Talk About When W3Talk About Love: Stories |
| Wher3I'm Calling From: Selected Stories           |
| Whit3Noise                                        |
| Cannery Row                                       |
| Oblivion: Stories                                 |
| Consider th3Lobster                               |
+---------------------------------------------------+
16 rows in set (0.00 sec)

mysql> SELECT
    ->     SUBSTRING(REPLACE(title, 'e', '3'), 1, 10) AS 'weird string'
    -> FROM books;
+--------------+
| weird string |
+--------------+
| Th3 Nam3sa   |
| Nors3 Myth   |
| Am3rican G   |
| Int3rpr3t3   |
| A Hologram   |
| Th3 Circl3   |
| Th3 Amazin   |
| Just Kids    |
| A H3artbr3   |
| Coralin3     |
| What W3 Ta   |
| Wh3r3 I'm    |
| Whit3 Nois   |
| Cann3ry Ro   |
| Oblivion:    |
| Consid3r t   |
+--------------+
16 rows in set (0.00 sec)
```

## REVERSE
```
mysql> SELECT REVERSE('Hello World');
+------------------------+
| REVERSE('Hello World') |
+------------------------+
| dlroW olleH            |
+------------------------+
1 row in set (0.00 sec)

mysql> SELECT CONCAT(author_fname, REVERSE(author_fname)) FROM books;
+---------------------------------------------+
| CONCAT(author_fname, REVERSE(author_fname)) |
+---------------------------------------------+
| JhumpaapmuhJ                                |
| NeillieN                                    |
| NeillieN                                    |
| JhumpaapmuhJ                                |
| DaveevaD                                    |
| DaveevaD                                    |
| MichaelleahciM                              |
| PattiittaP                                  |
| DaveevaD                                    |
| NeillieN                                    |
| RaymonddnomyaR                              |
| RaymonddnomyaR                              |
| DonnoD                                      |
| JohnnhoJ                                    |
| DaviddivaD                                  |
| DaviddivaD                                  |
+---------------------------------------------+
16 rows in set (0.00 sec)
```
## CHAR_LENGTH
```

mysql> SELECT CHAR_LENGTH('Hello World');
+----------------------------+
| CHAR_LENGTH('Hello World') |
+----------------------------+
|                         11 |
+----------------------------+
1 row in set (0.00 sec)

mysql> SELECT author_lname, CHAR_LENGTH(author_lname) AS 'length' FROM books;
+----------------+--------+
| author_lname   | length |
+----------------+--------+
| Lahiri         |      6 |
| Gaiman         |      6 |
| Gaiman         |      6 |
| Lahiri         |      6 |
| Eggers         |      6 |
| Eggers         |      6 |
| Chabon         |      6 |
| Smith          |      5 |
| Eggers         |      6 |
| Gaiman         |      6 |
| Carver         |      6 |
| Carver         |      6 |
| DeLillo        |      7 |
| Steinbeck      |      9 |
| Foster Wallace |     14 |
| Foster Wallace |     14 |
+----------------+--------+
16 rows in set (0.00 sec)

mysql> SELECT
    ->   CONCAT(author_lname, ' is ', CHAR_LENGTH(author_lname), ' characters long')
    -> FROM books;
+-----------------------------------------------------------------------------+
| CONCAT(author_lname, ' is ', CHAR_LENGTH(author_lname), ' characters long') |
+-----------------------------------------------------------------------------+
| Lahiri is 6 characters long                                                 |
| Gaiman is 6 characters long                                                 |
| Gaiman is 6 characters long                                                 |
| Lahiri is 6 characters long                                                 |
| Eggers is 6 characters long                                                 |
| Eggers is 6 characters long                                                 |
| Chabon is 6 characters long                                                 |
| Smith is 5 characters long                                                  |
| Eggers is 6 characters long                                                 |
| Gaiman is 6 characters long                                                 |
| Carver is 6 characters long                                                 |
| Carver is 6 characters long                                                 |
| DeLillo is 7 characters long                                                |
| Steinbeck is 9 characters long                                              |
| Foster Wallace is 14 characters long                                        |
| Foster Wallace is 14 characters long                                        |
+-----------------------------------------------------------------------------+
16 rows in set (0.00 sec)
```
