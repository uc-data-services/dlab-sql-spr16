#SQL Workshop Part 2
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
# Navigate to my github site: https://github.com/uc-data-services/dlab-sql-spr16# Either clone the site or download the file sql/mlb2010-2013.sql
# Save it where you can find it!
# Start the SQLite Manager from FireFox.
# Import the file you just downloaded.

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
Create a list of players in the database who did not play in every year in the sample.

```sql
SELECT DISTINCT firstname,lastname, yearID
FROM  Players P  LEFT OUTER JOIN  TeamMembers TM
ON P.playerID = TM.playerID
AND yearID is NULL
ORDER BY lastname, firstname
```

## Group by queries
Example:
Determine the average salary for players by handedness.

Here is how to do it by one grouping

```sql
SELECT AVG(salary)
FROM Players INNER JOIN TeamMembers
ON Players.playerID = TeamMembers.playerID
WHERE bats = ‘L’
AND throws = ‘R’
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
