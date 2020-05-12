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
## UPPER/LOWER case
```
mysql> SELECT UPPER('Hello World');
+----------------------+
| UPPER('Hello World') |
+----------------------+
| HELLO WORLD          |
+----------------------+
1 row in set (0.00 sec)

mysql> SELECT UPPER(title) FROM books;
+-----------------------------------------------------+
| UPPER(title)                                        |
+-----------------------------------------------------+
| THE NAMESAKE                                        |
| NORSE MYTHOLOGY                                     |
| AMERICAN GODS                                       |
| INTERPRETER OF MALADIES                             |
| A HOLOGRAM FOR THE KING: A NOVEL                    |
| THE CIRCLE                                          |
| THE AMAZING ADVENTURES OF KAVALIER & CLAY           |
| JUST KIDS                                           |
| A HEARTBREAKING WORK OF STAGGERING GENIUS           |
| CORALINE                                            |
| WHAT WE TALK ABOUT WHEN WE TALK ABOUT LOVE: STORIES |
| WHERE I'M CALLING FROM: SELECTED STORIES            |
| WHITE NOISE                                         |
| CANNERY ROW                                         |
| OBLIVION: STORIES                                   |
| CONSIDER THE LOBSTER                                |
+-----------------------------------------------------+
16 rows in set (0.00 sec)

mysql> SELECT
    ->   CONCAT('MY FAVORITE BOOK IS ', LOWER(title))
    -> FROM books;
+-------------------------------------------------------------------------+
| CONCAT('MY FAVORITE BOOK IS ', LOWER(title))                            |
+-------------------------------------------------------------------------+
| MY FAVORITE BOOK IS the namesake                                        |
| MY FAVORITE BOOK IS norse mythology                                     |
| MY FAVORITE BOOK IS american gods                                       |
| MY FAVORITE BOOK IS interpreter of maladies                             |
| MY FAVORITE BOOK IS a hologram for the king: a novel                    |
| MY FAVORITE BOOK IS the circle                                          |
| MY FAVORITE BOOK IS the amazing adventures of kavalier & clay           |
| MY FAVORITE BOOK IS just kids                                           |
| MY FAVORITE BOOK IS a heartbreaking work of staggering genius           |
| MY FAVORITE BOOK IS coraline                                            |
| MY FAVORITE BOOK IS what we talk about when we talk about love: stories |
| MY FAVORITE BOOK IS where i'm calling from: selected stories            |
| MY FAVORITE BOOK IS white noise                                         |
| MY FAVORITE BOOK IS cannery row                                         |
| MY FAVORITE BOOK IS oblivion: stories                                   |
| MY FAVORITE BOOK IS consider the lobster                                |
+-------------------------------------------------------------------------+
16 rows in set (0.00 sec)
```
## String Function Challange
```
mysql> SELECT
    ->    author_lname AS forwards,
    ->    REVERSE(author_lname) AS backwards
    -> FROM books;
+----------------+----------------+
| forwards       | backwards      |
+----------------+----------------+
| Lahiri         | irihaL         |
| Gaiman         | namiaG         |
| Gaiman         | namiaG         |
| Lahiri         | irihaL         |
| Eggers         | sreggE         |
| Eggers         | sreggE         |
| Chabon         | nobahC         |
| Smith          | htimS          |
| Eggers         | sreggE         |
| Gaiman         | namiaG         |
| Carver         | revraC         |
| Carver         | revraC         |
| DeLillo        | olliLeD        |
| Steinbeck      | kcebnietS      |
| Foster Wallace | ecallaW retsoF |
| Foster Wallace | ecallaW retsoF |
+----------------+----------------+
16 rows in set (0.00 sec)


mysql> SELECT
    ->   UPPER
    ->   (
    ->   CONCAT(author_fname, ' ', author_lname)
    ->   ) AS 'full name in caps'
    -> FROM books;
+----------------------+
| full name in caps    |
+----------------------+
| JHUMPA LAHIRI        |
| NEIL GAIMAN          |
| NEIL GAIMAN          |
| JHUMPA LAHIRI        |
| DAVE EGGERS          |
| DAVE EGGERS          |
| MICHAEL CHABON       |
| PATTI SMITH          |
| DAVE EGGERS          |
| NEIL GAIMAN          |
| RAYMOND CARVER       |
| RAYMOND CARVER       |
| DON DELILLO          |
| JOHN STEINBECK       |
| DAVID FOSTER WALLACE |
| DAVID FOSTER WALLACE |
+----------------------+
16 rows in set (0.00 sec)

mysql> SELECT
    ->    CONCAT(title, ' was released in ', released_year) AS blurb
    -> FROM books;
+--------------------------------------------------------------------------+
| blurb                                                                    |
+--------------------------------------------------------------------------+
| The Namesake was released in 2003                                        |
| Norse Mythology was released in 2016                                     |
| American Gods was released in 2001                                       |
| Interpreter of Maladies was released in 1996                             |
| A Hologram for the King: A Novel was released in 2012                    |
| The Circle was released in 2013                                          |
| The Amazing Adventures of Kavalier & Clay was released in 2000           |
| Just Kids was released in 2010                                           |
| A Heartbreaking Work of Staggering Genius was released in 2001           |
| Coraline was released in 2003                                            |
| What We Talk About When We Talk About Love: Stories was released in 1981 |
| Where I'm Calling From: Selected Stories was released in 1989            |
| White Noise was released in 1985                                         |
| Cannery Row was released in 1945                                         |
| Oblivion: Stories was released in 2004                                   |
| Consider the Lobster was released in 2005                                |
+--------------------------------------------------------------------------+
16 rows in set (0.00 sec)


mysql> SELECT
    ->    title,
    ->    CHAR_LENGTH(title) AS 'character count'
    -> FROM books;
+-----------------------------------------------------+-----------------+
| title                                               | character count |
+-----------------------------------------------------+-----------------+
| The Namesake                                        |              12 |
| Norse Mythology                                     |              15 |
| American Gods                                       |              13 |
| Interpreter of Maladies                             |              23 |
| A Hologram for the King: A Novel                    |              32 |
| The Circle                                          |              10 |
| The Amazing Adventures of Kavalier & Clay           |              41 |
| Just Kids                                           |               9 |
| A Heartbreaking Work of Staggering Genius           |              41 |
| Coraline                                            |               8 |
| What We Talk About When We Talk About Love: Stories |              51 |
| Where I'm Calling From: Selected Stories            |              40 |
| White Noise                                         |              11 |
| Cannery Row                                         |              11 |
| Oblivion: Stories                                   |              17 |
| Consider the Lobster                                |              20 |
+-----------------------------------------------------+-----------------+
16 rows in set (0.00 sec)

mysql> SELECT
    ->    CONCAT(SUBSTRING(title, 1, 10), '...') AS 'short title',
    ->    CONCAT(author_lname, ',', author_fname) AS author,
    ->    CONCAT(stock_quantity, ' in stock') AS quantity
    -> FROM books;
+---------------+----------------------+--------------+
| short title   | author               | quantity     |
+---------------+----------------------+--------------+
| The Namesa... | Lahiri,Jhumpa        | 32 in stock  |
| Norse Myth... | Gaiman,Neil          | 43 in stock  |
| American G... | Gaiman,Neil          | 12 in stock  |
| Interprete... | Lahiri,Jhumpa        | 97 in stock  |
| A Hologram... | Eggers,Dave          | 154 in stock |
| The Circle... | Eggers,Dave          | 26 in stock  |
| The Amazin... | Chabon,Michael       | 68 in stock  |
| Just Kids...  | Smith,Patti          | 55 in stock  |
| A Heartbre... | Eggers,Dave          | 104 in stock |
| Coraline...   | Gaiman,Neil          | 100 in stock |
| What We Ta... | Carver,Raymond       | 23 in stock  |
| Where I'm ... | Carver,Raymond       | 12 in stock  |
| White Nois... | DeLillo,Don          | 49 in stock  |
| Cannery Ro... | Steinbeck,John       | 95 in stock  |
| Oblivion: ... | Foster Wallace,David | 172 in stock |
| Consider t... | Foster Wallace,David | 92 in stock  |
+---------------+----------------------+--------------+
16 rows in set (0.00 sec)
```

# Refining Selections
## DISTINCT
```
mysql> INSERT INTO books
    ->     (title, author_fname, author_lname, released_year, stock_quantity, pages)
    ->     VALUES ('10% Happier', 'Dan', 'Harris', 2014, 29, 256),
    ->            ('fake_book', 'Freida', 'Harris', 2001, 287, 428),
    ->            ('Lincoln In The Bardo', 'George', 'Saunders', 2017, 1000, 367);
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT title FROM books;
+-----------------------------------------------------+
| title                                               |
+-----------------------------------------------------+
| The Namesake                                        |
| Norse Mythology                                     |
| American Gods                                       |
| Interpreter of Maladies                             |
| A Hologram for the King: A Novel                    |
| The Circle                                          |
| The Amazing Adventures of Kavalier & Clay           |
| Just Kids                                           |
| A Heartbreaking Work of Staggering Genius           |
| Coraline                                            |
| What We Talk About When We Talk About Love: Stories |
| Where I'm Calling From: Selected Stories            |
| White Noise                                         |
| Cannery Row                                         |
| Oblivion: Stories                                   |
| Consider the Lobster                                |
| 10% Happier                                         |
| fake_book                                           |
| Lincoln In The Bardo                                |
+-----------------------------------------------------+
19 rows in set (0.00 sec)

mysql> SELECT author_lname FROM books;
+----------------+
| author_lname   |
+----------------+
| Lahiri         |
| Gaiman         |
| Gaiman         |
| Lahiri         |
| Eggers         |
| Eggers         |
| Chabon         |
| Smith          |
| Eggers         |
| Gaiman         |
| Carver         |
| Carver         |
| DeLillo        |
| Steinbeck      |
| Foster Wallace |
| Foster Wallace |
| Harris         |
| Harris         |
| Saunders       |
+----------------+
19 rows in set (0.01 sec)

mysql> SELECT DISTINCT author_lname FROM books;
+----------------+
| author_lname   |
+----------------+
| Lahiri         |
| Gaiman         |
| Eggers         |
| Chabon         |
| Smith          |
| Carver         |
| DeLillo        |
| Steinbeck      |
| Foster Wallace |
| Harris         |
| Saunders       |
+----------------+
11 rows in set (0.00 sec)

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
| Dan          | Harris         |
| Freida       | Harris         |
| George       | Saunders       |
+--------------+----------------+
19 rows in set (0.00 sec)

mysql> SELECT DISTINCT author_fname, author_lname FROM books;
+--------------+----------------+
| author_fname | author_lname   |
+--------------+----------------+
| Jhumpa       | Lahiri         |
| Neil         | Gaiman         |
| Dave         | Eggers         |
| Michael      | Chabon         |
| Patti        | Smith          |
| Raymond      | Carver         |
| Don          | DeLillo        |
| John         | Steinbeck      |
| David        | Foster Wallace |
| Dan          | Harris         |
| Freida       | Harris         |
| George       | Saunders       |
+--------------+----------------+
12 rows in set (0.00 sec)
```
## ORDER BY
```
mysql> SELECT author_lname FROM books;
+----------------+
| author_lname   |
+----------------+
| Lahiri         |
| Gaiman         |
| Gaiman         |
| Lahiri         |
| Eggers         |
| Eggers         |
| Chabon         |
| Smith          |
| Eggers         |
| Gaiman         |
| Carver         |
| Carver         |
| DeLillo        |
| Steinbeck      |
| Foster Wallace |
| Foster Wallace |
| Harris         |
| Harris         |
| Saunders       |
+----------------+
19 rows in set (0.00 sec)

mysql> SELECT author_lname FROM books ORDER BY author_lname;
+----------------+
| author_lname   |
+----------------+
| Carver         |
| Carver         |
| Chabon         |
| DeLillo        |
| Eggers         |
| Eggers         |
| Eggers         |
| Foster Wallace |
| Foster Wallace |
| Gaiman         |
| Gaiman         |
| Gaiman         |
| Harris         |
| Harris         |
| Lahiri         |
| Lahiri         |
| Saunders       |
| Smith          |
| Steinbeck      |
+----------------+
19 rows in set (0.02 sec)

mysql> SELECT author_lname FROM books ORDER BY author_lname DESC;
+----------------+
| author_lname   |
+----------------+
| Steinbeck      |
| Smith          |
| Saunders       |
| Lahiri         |
| Lahiri         |
| Harris         |
| Harris         |
| Gaiman         |
| Gaiman         |
| Gaiman         |
| Foster Wallace |
| Foster Wallace |
| Eggers         |
| Eggers         |
| Eggers         |
| DeLillo        |
| Chabon         |
| Carver         |
| Carver         |
+----------------+
19 rows in set (0.00 sec)

mysql> SELECT title, released_year, pages FROM books ORDER BY released_year;
+-----------------------------------------------------+---------------+-------+
| title                                               | released_year | pages |
+-----------------------------------------------------+---------------+-------+
| Cannery Row                                         |          1945 |   181 |
| What We Talk About When We Talk About Love: Stories |          1981 |   176 |
| White Noise                                         |          1985 |   320 |
| Where I'm Calling From: Selected Stories            |          1989 |   526 |
| Interpreter of Maladies                             |          1996 |   198 |
| The Amazing Adventures of Kavalier & Clay           |          2000 |   634 |
| fake_book                                           |          2001 |   428 |
| American Gods                                       |          2001 |   465 |
| A Heartbreaking Work of Staggering Genius           |          2001 |   437 |
| Coraline                                            |          2003 |   208 |
| The Namesake                                        |          2003 |   291 |
| Oblivion: Stories                                   |          2004 |   329 |
| Consider the Lobster                                |          2005 |   343 |
| Just Kids                                           |          2010 |   304 |
| A Hologram for the King: A Novel                    |          2012 |   352 |
| The Circle                                          |          2013 |   504 |
| 10% Happier                                         |          2014 |   256 |
| Norse Mythology                                     |          2016 |   304 |
| Lincoln In The Bardo                                |          2017 |   367 |
+-----------------------------------------------------+---------------+-------+
19 rows in set (0.00 sec)

mysql> SELECT title, pages FROM books ORDER BY released_year;
+-----------------------------------------------------+-------+
| title                                               | pages |
+-----------------------------------------------------+-------+
| Cannery Row                                         |   181 |
| What We Talk About When We Talk About Love: Stories |   176 |
| White Noise                                         |   320 |
| Where I'm Calling From: Selected Stories            |   526 |
| Interpreter of Maladies                             |   198 |
| The Amazing Adventures of Kavalier & Clay           |   634 |
| fake_book                                           |   428 |
| American Gods                                       |   465 |
| A Heartbreaking Work of Staggering Genius           |   437 |
| Coraline                                            |   208 |
| The Namesake                                        |   291 |
| Oblivion: Stories                                   |   329 |
| Consider the Lobster                                |   343 |
| Just Kids                                           |   304 |
| A Hologram for the King: A Novel                    |   352 |
| The Circle                                          |   504 |
| 10% Happier                                         |   256 |
| Norse Mythology                                     |   304 |
| Lincoln In The Bardo                                |   367 |
+-----------------------------------------------------+-------+
19 rows in set (0.00 sec)

mysql> SELECT title, author_fname, author_lname
    -> FROM books ORDER BY 1;
+-----------------------------------------------------+--------------+----------------+
| title                                               | author_fname | author_lname   |
+-----------------------------------------------------+--------------+----------------+
| 10% Happier                                         | Dan          | Harris         |
| A Heartbreaking Work of Staggering Genius           | Dave         | Eggers         |
| A Hologram for the King: A Novel                    | Dave         | Eggers         |
| American Gods                                       | Neil         | Gaiman         |
| Cannery Row                                         | John         | Steinbeck      |
| Consider the Lobster                                | David        | Foster Wallace |
| Coraline                                            | Neil         | Gaiman         |
| fake_book                                           | Freida       | Harris         |
| Interpreter of Maladies                             | Jhumpa       | Lahiri         |
| Just Kids                                           | Patti        | Smith          |
| Lincoln In The Bardo                                | George       | Saunders       |
| Norse Mythology                                     | Neil         | Gaiman         |
| Oblivion: Stories                                   | David        | Foster Wallace |
| The Amazing Adventures of Kavalier & Clay           | Michael      | Chabon         |
| The Circle                                          | Dave         | Eggers         |
| The Namesake                                        | Jhumpa       | Lahiri         |
| What We Talk About When We Talk About Love: Stories | Raymond      | Carver         |
| Where I'm Calling From: Selected Stories            | Raymond      | Carver         |
| White Noise                                         | Don          | DeLillo        |
+-----------------------------------------------------+--------------+----------------+
19 rows in set (0.00 sec)

mysql> SELECT title, author_fname, author_lname
    -> FROM books ORDER BY 2;
+-----------------------------------------------------+--------------+----------------+
| title                                               | author_fname | author_lname   |
+-----------------------------------------------------+--------------+----------------+
| 10% Happier                                         | Dan          | Harris         |
| A Hologram for the King: A Novel                    | Dave         | Eggers         |
| The Circle                                          | Dave         | Eggers         |
| A Heartbreaking Work of Staggering Genius           | Dave         | Eggers         |
| Consider the Lobster                                | David        | Foster Wallace |
| Oblivion: Stories                                   | David        | Foster Wallace |
| White Noise                                         | Don          | DeLillo        |
| fake_book                                           | Freida       | Harris         |
| Lincoln In The Bardo                                | George       | Saunders       |
| Interpreter of Maladies                             | Jhumpa       | Lahiri         |
| The Namesake                                        | Jhumpa       | Lahiri         |
| Cannery Row                                         | John         | Steinbeck      |
| The Amazing Adventures of Kavalier & Clay           | Michael      | Chabon         |
| Coraline                                            | Neil         | Gaiman         |
| American Gods                                       | Neil         | Gaiman         |
| Norse Mythology                                     | Neil         | Gaiman         |
| Just Kids                                           | Patti        | Smith          |
| Where I'm Calling From: Selected Stories            | Raymond      | Carver         |
| What We Talk About When We Talk About Love: Stories | Raymond      | Carver         |
+-----------------------------------------------------+--------------+----------------+
19 rows in set (0.00 sec)

mysql> SELECT author_fname, author_lname FROM books
    -> ORDER BY author_lname, author_fname;
+--------------+----------------+
| author_fname | author_lname   |
+--------------+----------------+
| Raymond      | Carver         |
| Raymond      | Carver         |
| Michael      | Chabon         |
| Don          | DeLillo        |
| Dave         | Eggers         |
| Dave         | Eggers         |
| Dave         | Eggers         |
| David        | Foster Wallace |
| David        | Foster Wallace |
| Neil         | Gaiman         |
| Neil         | Gaiman         |
| Neil         | Gaiman         |
| Dan          | Harris         |
| Freida       | Harris         |
| Jhumpa       | Lahiri         |
| Jhumpa       | Lahiri         |
| George       | Saunders       |
| Patti        | Smith          |
| John         | Steinbeck      |
+--------------+----------------+
19 rows in set (0.00 sec)
```
## LIMIT
```
mysql> SELECT title FROM books LIMIT 3;
+-----------------+
| title           |
+-----------------+
| The Namesake    |
| Norse Mythology |
| American Gods   |
+-----------------+
3 rows in set (0.00 sec)

mysql> SELECT title, released_year FROM books
    -> ORDER BY released_year DESC LIMIT 5;
+----------------------------------+---------------+
| title                            | released_year |
+----------------------------------+---------------+
| Lincoln In The Bardo             |          2017 |
| Norse Mythology                  |          2016 |
| 10% Happier                      |          2014 |
| The Circle                       |          2013 |
| A Hologram for the King: A Novel |          2012 |
+----------------------------------+---------------+
5 rows in set (0.00 sec)

mysql> SELECT title, released_year FROM books
    -> ORDER BY released_year DESC LIMIT 0,3;
+----------------------+---------------+
| title                | released_year |
+----------------------+---------------+
| Lincoln In The Bardo |          2017 |
| Norse Mythology      |          2016 |
| 10% Happier          |          2014 |
+----------------------+---------------+
3 rows in set (0.00 sec)

mysql> SELECT title, released_year FROM books
    -> ORDER BY released_year DESC LIMIT 1,3;
+-----------------+---------------+
| title           | released_year |
+-----------------+---------------+
| Norse Mythology |          2016 |
| 10% Happier     |          2014 |
| The Circle      |          2013 |
+-----------------+---------------+
3 rows in set (0.00 sec)

mysql> SELECT * FROM tbl LIMIT 95,18446744073709551615;
ERROR 1146 (42S02): Table 'ann_arbor.tbl' doesn't exist
mysql> SELECT title FROM books LIMIT 5, 123219476457;
+-----------------------------------------------------+
| title                                               |
+-----------------------------------------------------+
| The Circle                                          |
| The Amazing Adventures of Kavalier & Clay           |
| Just Kids                                           |
| A Heartbreaking Work of Staggering Genius           |
| Coraline                                            |
| What We Talk About When We Talk About Love: Stories |
| Where I'm Calling From: Selected Stories            |
| White Noise                                         |
| Cannery Row                                         |
| Oblivion: Stories                                   |
| Consider the Lobster                                |
| 10% Happier                                         |
| fake_book                                           |
| Lincoln In The Bardo                                |
+-----------------------------------------------------+
14 rows in set (0.00 sec)
```

## LIKE
```
mysql> SELECT title, author_fname FROM books WHERE author_fname LIKE '%da%';
+-------------------------------------------+--------------+
| title                                     | author_fname |
+-------------------------------------------+--------------+
| A Hologram for the King: A Novel          | Dave         |
| The Circle                                | Dave         |
| A Heartbreaking Work of Staggering Genius | Dave         |
| Oblivion: Stories                         | David        |
| Consider the Lobster                      | David        |
| 10% Happier                               | Dan          |
| fake_book                                 | Freida       |
+-------------------------------------------+--------------+
7 rows in set (0.00 sec)

mysql> SELECT title, author_fname FROM books WHERE author_fname LIKE 'da%';
+-------------------------------------------+--------------+
| title                                     | author_fname |
+-------------------------------------------+--------------+
| A Hologram for the King: A Novel          | Dave         |
| The Circle                                | Dave         |
| A Heartbreaking Work of Staggering Genius | Dave         |
| Oblivion: Stories                         | David        |
| Consider the Lobster                      | David        |
| 10% Happier                               | Dan          |
+-------------------------------------------+--------------+
6 rows in set (0.00 sec)

mysql> SELECT title FROM books WHERE  title LIKE 'the';
Empty set (0.00 sec)

mysql> SELECT title FROM books WHERE  title LIKE '%the';
Empty set (0.00 sec)

mysql> SELECT title FROM books WHERE title LIKE '%the%';
+-------------------------------------------+
| title                                     |
+-------------------------------------------+
| The Namesake                              |
| A Hologram for the King: A Novel          |
| The Circle                                |
| The Amazing Adventures of Kavalier & Clay |
| Consider the Lobster                      |
| Lincoln In The Bardo                      |
+-------------------------------------------+
6 rows in set (0.00 sec)

mysql> SELECT title, stock_quantity FROM books;
+-----------------------------------------------------+----------------+
| title                                               | stock_quantity |
+-----------------------------------------------------+----------------+
| The Namesake                                        |             32 |
| Norse Mythology                                     |             43 |
| American Gods                                       |             12 |
| Interpreter of Maladies                             |             97 |
| A Hologram for the King: A Novel                    |            154 |
| The Circle                                          |             26 |
| The Amazing Adventures of Kavalier & Clay           |             68 |
| Just Kids                                           |             55 |
| A Heartbreaking Work of Staggering Genius           |            104 |
| Coraline                                            |            100 |
| What We Talk About When We Talk About Love: Stories |             23 |
| Where I'm Calling From: Selected Stories            |             12 |
| White Noise                                         |             49 |
| Cannery Row                                         |             95 |
| Oblivion: Stories                                   |            172 |
| Consider the Lobster                                |             92 |
| 10% Happier                                         |             29 |
| fake_book                                           |            287 |
| Lincoln In The Bardo                                |           1000 |
+-----------------------------------------------------+----------------+
19 rows in set (0.00 sec)

mysql> SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '____';
+----------------------+----------------+
| title                | stock_quantity |
+----------------------+----------------+
| Lincoln In The Bardo |           1000 |
+----------------------+----------------+
1 row in set (0.00 sec)

mysql> SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '__';
+-----------------------------------------------------+----------------+
| title                                               | stock_quantity |
+-----------------------------------------------------+----------------+
| The Namesake                                        |             32 |
| Norse Mythology                                     |             43 |
| American Gods                                       |             12 |
| Interpreter of Maladies                             |             97 |
| The Circle                                          |             26 |
| The Amazing Adventures of Kavalier & Clay           |             68 |
| Just Kids                                           |             55 |
| What We Talk About When We Talk About Love: Stories |             23 |
| Where I'm Calling From: Selected Stories            |             12 |
| White Noise                                         |             49 |
| Cannery Row                                         |             95 |
| Consider the Lobster                                |             92 |
| 10% Happier                                         |             29 |
+-----------------------------------------------------+----------------+
13 rows in set (0.00 sec)

mysql>  SELECT title FROM books;
+-----------------------------------------------------+
| title                                               |
+-----------------------------------------------------+
| The Namesake                                        |
| Norse Mythology                                     |
| American Gods                                       |
| Interpreter of Maladies                             |
| A Hologram for the King: A Novel                    |
| The Circle                                          |
| The Amazing Adventures of Kavalier & Clay           |
| Just Kids                                           |
| A Heartbreaking Work of Staggering Genius           |
| Coraline                                            |
| What We Talk About When We Talk About Love: Stories |
| Where I'm Calling From: Selected Stories            |
| White Noise                                         |
| Cannery Row                                         |
| Oblivion: Stories                                   |
| Consider the Lobster                                |
| 10% Happier                                         |
| fake_book                                           |
| Lincoln In The Bardo                                |
+-----------------------------------------------------+
19 rows in set (0.00 sec)

mysql> SELECT title FROM books WHERE title LIKE '%\%%';
+-------------+
| title       |
+-------------+
| 10% Happier |
+-------------+
1 row in set (0.00 sec)

mysql>
mysql> SELECT title FROM books WHERE title LIKE '%\_%';
+-----------+
| title     |
+-----------+
| fake_book |
+-----------+
1 row in set (0.00 sec)
```
# Refining Selections exercise
```
mysql> SELECT title FROM books WHERE title LIKE '%stories%';
+-----------------------------------------------------+
| title                                               |
+-----------------------------------------------------+
| What We Talk About When We Talk About Love: Stories |
| Where I'm Calling From: Selected Stories            |
| Oblivion: Stories                                   |
+-----------------------------------------------------+
3 rows in set (0.00 sec)

mysql> SELECT title, pages FROM books ORDER BY pages DESC LIMIT 1;
+-------------------------------------------+-------+
| title                                     | pages |
+-------------------------------------------+-------+
| The Amazing Adventures of Kavalier & Clay |   634 |
+-------------------------------------------+-------+
1 row in set (0.01 sec)

mysql> SELECT
    ->     CONCAT(title, ' - ', released_year) AS summary
    -> FROM books ORDER BY released_year DESC LIMIT 3;
+-----------------------------+
| summary                     |
+-----------------------------+
| Lincoln In The Bardo - 2017 |
| Norse Mythology - 2016      |
| 10% Happier - 2014          |
+-----------------------------+
3 rows in set (0.00 sec)

mysql> SELECT title, author_lname FROM books WHERE author_lname LIKE '% %';
+----------------------+----------------+
| title                | author_lname   |
+----------------------+----------------+
| Oblivion: Stories    | Foster Wallace |
| Consider the Lobster | Foster Wallace |
+----------------------+----------------+
2 rows in set (0.00 sec)

mysql> SELECT title, released_year, stock_quantity
    -> FROM books ORDER BY stock_quantity LIMIT 3;
+-----------------------------------------------------+---------------+----------------+
| title                                               | released_year | stock_quantity |
+-----------------------------------------------------+---------------+----------------+
| Where I'm Calling From: Selected Stories            |          1989 |             12 |
| American Gods                                       |          2001 |             12 |
| What We Talk About When We Talk About Love: Stories |          1981 |             23 |
+-----------------------------------------------------+---------------+----------------+
3 rows in set (0.00 sec)

mysql> SELECT title, author_lname
    -> FROM books ORDER BY author_lname, title;
+-----------------------------------------------------+----------------+
| title                                               | author_lname   |
+-----------------------------------------------------+----------------+
| What We Talk About When We Talk About Love: Stories | Carver         |
| Where I'm Calling From: Selected Stories            | Carver         |
| The Amazing Adventures of Kavalier & Clay           | Chabon         |
| White Noise                                         | DeLillo        |
| A Heartbreaking Work of Staggering Genius           | Eggers         |
| A Hologram for the King: A Novel                    | Eggers         |
| The Circle                                          | Eggers         |
| Consider the Lobster                                | Foster Wallace |
| Oblivion: Stories                                   | Foster Wallace |
| American Gods                                       | Gaiman         |
| Coraline                                            | Gaiman         |
| Norse Mythology                                     | Gaiman         |
| 10% Happier                                         | Harris         |
| fake_book                                           | Harris         |
| Interpreter of Maladies                             | Lahiri         |
| The Namesake                                        | Lahiri         |
| Lincoln In The Bardo                                | Saunders       |
| Just Kids                                           | Smith          |
| Cannery Row                                         | Steinbeck      |
+-----------------------------------------------------+----------------+
19 rows in set (0.00 sec)

mysql> SELECT
    ->     CONCAT(
    ->         'MY FAVORITE AUTHOR IS ',
    ->         UPPER(author_fname),
    ->         ' ',
    ->         UPPER(author_lname),
    ->         '!'
    ->     ) AS yell
    -> FROM books ORDER BY author_lname;
+---------------------------------------------+
| yell                                        |
+---------------------------------------------+
| MY FAVORITE AUTHOR IS RAYMOND CARVER!       |
| MY FAVORITE AUTHOR IS RAYMOND CARVER!       |
| MY FAVORITE AUTHOR IS MICHAEL CHABON!       |
| MY FAVORITE AUTHOR IS DON DELILLO!          |
| MY FAVORITE AUTHOR IS DAVE EGGERS!          |
| MY FAVORITE AUTHOR IS DAVE EGGERS!          |
| MY FAVORITE AUTHOR IS DAVE EGGERS!          |
| MY FAVORITE AUTHOR IS DAVID FOSTER WALLACE! |
| MY FAVORITE AUTHOR IS DAVID FOSTER WALLACE! |
| MY FAVORITE AUTHOR IS NEIL GAIMAN!          |
| MY FAVORITE AUTHOR IS NEIL GAIMAN!          |
| MY FAVORITE AUTHOR IS NEIL GAIMAN!          |
| MY FAVORITE AUTHOR IS DAN HARRIS!           |
| MY FAVORITE AUTHOR IS FREIDA HARRIS!        |
| MY FAVORITE AUTHOR IS JHUMPA LAHIRI!        |
| MY FAVORITE AUTHOR IS JHUMPA LAHIRI!        |
| MY FAVORITE AUTHOR IS GEORGE SAUNDERS!      |
| MY FAVORITE AUTHOR IS PATTI SMITH!          |
| MY FAVORITE AUTHOR IS JOHN STEINBECK!       |
+---------------------------------------------+
19 rows in set (0.00 sec)
```
