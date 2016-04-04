#SQL Workshop Part 1
##Outcomes
* Create a simple sql database using the Firefox SQLite Manager
* Use the INSERT and SELECT commands to modify and query the database
* Discuss data normalization, including 1NF, 2NF, and 3NF
##Getting started
I this workshop we'll use SQLite, a powerful and easy to install database engine. Conveniently, it's built in to the Firefox web browser. If you don't have Firefox installed, please install it now. Once it's installed, you'll need to also install the [SQLite Manager extension](https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager/).

![install sqlite](images/install_sqlite.png)

Once you've installed SQLite Manager, you can start it from the Firefox tools menu.

![tools menu](images/tools_menu.png)
## Create a database and a table
* Start the SQLite Manager
* Choose "New Database" from the Database menu
* Name the new database "Baseball" and save it in a project directory.
* Choose "Create Table" from the Table menu.
* Fill in the form like this:

![create table](images/create_table.png)

* Click OK, and note the code that pops up in the confirmation box. It contains a SQL command for creating a new table in a database. We could also have created this table by typing this command into the "Enter SQL" box and running it.

###Challenge
Write a SQL command to create a table called FavoritePlayer containing fields called Fan_name and PlayerID.

## Add data
* Type or cut and paste the following line into the command box:
```sql
INSERT INTO Players (playerID, birthcountry, birthstate, firstname, lastname, weight, height, bats, throws)
VALUES('poseybu01','USA','GA','Buster','Posey',220,73,'R','R');
```