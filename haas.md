

#This document: https://git.io/vwBnm
## Outcomes
* learn inner join usage and syntax
* learn outer join usage and syntax
* learn group by usage and syntax
* quick intro to sql in R and Python

## Getting started
I this workshop we'll use SQLite, a powerful and easy to install database engine. Conveniently, it's built in to the Firefox web browser. If you don't have Firefox installed, please install it now. Once it's installed, you'll need to also install the [SQLite Manager extension](https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager/).

![install sqlite](images/install_sqlite.png)

Once you've installed SQLite Manager, you can start it from the Firefox tools menu.

![tools menu](images/tools_menu.png)

## Quick review and introduction to SQL queries
[Slides](https://docs.google.com/presentation/d/1RRZoSelHpEGcKapa6E2MAn3NHXVlrbMMbimb2lW8UkA/edit?usp=sharing)

## Import the example database
* Load the example database by following these steps:
 * Download the sqlite database file [bball.sqlite](https://berkeley.box.com/s/xp4bhbkebhhaf74z09540bpzjrgfht2a)
 * Start the SQLite Manager from FireFox.
 * Use the Database|Connect Database option and open bball.sqlite.
* Alternative 
 * Navigate to my github site: https://github.com/uc-data-services/dlab-sql-spr16# Either clone the site or download the file sql/mlb2010-2013.sql
 * Save it where you can find it!
 * Start the SQLite Manager from FireFox.
 * Import the file you just downloaded.
## Basic queries
* Data queries require at least two components, SELECT and FROM
* For example, all the data in Players can be retrieved like this:
```sql
SELECT * 
FROM Players;
```
* SELECT can contain specific field names.

```sql
SELECT playerID, firstname, lastname
FROM Players;
```
* Results can be ordered

```sql
SELECT playerID, firstname, lastname
FROM Players
ORDER By lastname ASC;
```

* Results can be formatted. Here's and example of concatenating the first and last name, and changing the label.

```sql
SELECT playerID, firstname || lastname as name
FROM Players
ORDER By lastname ASC;
```
### Challenge
Can you figure out how to add a space between the the first and last names?

* It's possible to calculate aggregate statistics like averages from our table -- some other aggregate functions are max(), min(), count(), and sum()

```sql
SELECT avg(weight) as avgweight
FROM Players;
```

* The DISTINCT clause is used when you need a set of the unique members of a field or combination of fields.

```sql
SELECT DISTINCT birthcountry
FROM Players;
```
* To filter results, a WHERE clause is used. to list all players that bat lefthanded we can do this:

```sql
SELECT firstname || lastname as lefties
FROM Players
WHERE bats='L';
```
* And it's possible to add multiple WHERE conditions using AND and OR
```sql
SELECT firstname || lastname as lefties
FROM Players
WHERE bats='L' OR bats='B';
```


## Join queries
### Inner join example:

Identify the names of all player who bat left and throw right.

```sql
SELECT firstname,lastname, salary
FROM Players INNER JOIN TeamMembers
ON Players.playerID = TeamMembers.playerID
AND bats = 'L'
AND throws = 'R'
```

Identify the names and team names of all players who bat left and throw right.

```sql
SELECT firstname,lastname, name
FROM Players INNER JOIN TeamMembers
ON Players.playerID = TeamMembers.playerID
INNER JOIN TeamNames
ON TeamMembers.teamID = TeamNames.teamID
AND bats = 'L'
AND throws = 'R'
```
How do we get rid of duplicates?

Challenge:
Write a query to create an alphabetical list of player names and salaries for the 2013 San Francisco Giants.

### Outer join example:
Determine if there are any players in the TeamMembers table without corresponding data in the Players table.

```sql
SELECT DISTINCT tm.playerID, yearID
FROM TeamMembers TM LEFT OUTER JOIN  Players P
ON TM.playerID = P.playerID
AND P.playerID IS null
```

## Group by queries
Example:
Determine the average salary for players by handedness.

Here is how to do it by one grouping

```sql
SELECT AVG(salary)
FROM Players INNER JOIN TeamMembers
ON Players.playerID = TeamMembers.playerID
WHERE bats = L
AND throws = R
```

Hint: To make the output nicer, use the ROUND() function

To compare over all combinations:
```sql 
SELECT bats, throws, ROUND(AVG(salary))
FROM Players INNER JOIN TeamMembers
ON Players.playerID = TeamMembers.playerID
GROUP BY bats, throws
```

It is possible to filter on a calculated value by using "HAVING"

```sql 
SELECT bats, throws, ROUND(AVG(salary)) as avg_salary
FROM Players INNER JOIN TeamMembers
ON Players.playerID = TeamMembers.playerID
GROUP BY bats, throws
HAVING avg_salary < 3000000
```

## RStudio example

```R
install.packages("RSQLite")
library(RSQLite)
dbname <- "D:/Users/hdekker/DLAB_SQL_workshop/db/mlb2010-2013.sqlite"
driver <- dbDriver("SQLite")
connect <- dbConnect(driver, dbname=dbname)
df <- dbGetQuery(connect, "select * from Players")
```

## Python example

```python
import sqlite3

# Connecting to the database file
dbname = "D:/Users/hdekker/DLAB_SQL_workshop/db/mlb2010-2013.sqlite"
conn = sqlite3.connect(dbname)
c = conn.cursor()

c.execute('SELECT * FROM Players')
all_rows = c.fetchall()
```
