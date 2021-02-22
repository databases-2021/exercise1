# Exercise 1

**Name**: <!-- TODO: fill in your full name here, firstname and lastname -->

<!-- Remove everything down until the next comment after you finished the instructions -->

## Getting started

To get started you will first need to get a copy of this repository. Follow the steps below to get your own personal copy. This only needs to be done once.

1. Get the GitHub classroom invitation link from Toledo
1. Accept the assignment
1. Wait for your own personal copy to be created (can take up to several minutes)
1. Open the GitHub page of your repository
1. Copy the ssh clone-url (green button) that looks like `git@github.com:databases-2021/exercise1-db-<username>.git`
1. Traverse to a local directory on your system where you wish to clone the repo using Windows Explorer. Open `PowerShell` in that location by typing powershell in the location bar as shown in the screenshot below.

_Please don't choose a destination directory that is nested very deeply. The structure of this repo introduces quite a lot of subdirectories and might give problems towards maximum path length in Windows._

![Opening PowerShell in directory](./img/powershell.png)

Issue the git clone command followed by the url you copied.

```shell
git clone <place-ssh-url-here>
```

You should get the following output:

```shell
Cloning into 'exercise1-db-<username>.git'...
Warning: Permanently added the RSA host key for IP address '192.30.253.113' to the list of known hosts.
remote: Enumerating objects: 185, done.
remote: Compressing objects: 100% (109/109), done.
Receiving objects: 100% (185/185), 128.22 KiB | 625.00 KiB/s, done.
Resolving deltas: 100% (57/57), done.
```

Now you should have your local copy of the repository.

All git commands in other sections should always be executed inside of the project dir called `exercise1-db-<username>`.

## Submitting the exercise

Changes can be committed and pushed back to GitHub using the terminal.

Traverse to your local `exercise1-db-<username>` directory and open a `powershell` window.

1. Add all changed files: `git add .`
2. Commit the files and add a message: `git commit -m "My message goes here"`
3. Push your changes to GitHub: `git push origin master`
  ![Committing and pushing via PowerShell](./img/commit_push_powershell.png)
4. To make sure all is well, you can always issue the command `git status`, even in between other commands.

You can also navigate to your GitHub page of this repo and check if all went well.

Make it a habit of committing regularly, you can make multiple commits for a single exercise.

## Goals

1. Install software
1. Setup example databases
1. Creating tables

## Setup

### Install software

#### Prerequisites

1. Git (CLI)
1. Visual Studio Code

[More information](https://devbit-git-course.netlify.app/git-tools)

#### XAMPP

XAMPP is a software package that bundles different software tools for webdeveloment into a single application.

XAMPP stands for:

* **X**: works on any operating system \(Windows, Linux and OSX\)
* **A**: Apache HTTP server
* **M**: MariaDB Database Management System \(~ MySQL\)
* **P**: PHP programming language
* **P**: Perl programming language

XAMPP can be download for free on the website of Apachefriends: [https://www.apachefriends.org](https://www.apachefriends.org)

[Installation guide](https://vives.gitbook.io/software-installation-guide/xampp)

#### Extra

1. [MySQL Workbench](https://www.mysql.com/products/workbench/) is a set of tools that is mainly used to create and manage MySQL or MariaDB databases in a graphical interface. 
1. [SQLite Browser](http://sqlitebrowser.org/) is a graphical user interface to create and manage SQLite databases.

### Setting up the example databases

In this course we use the forta database. This database is made by Ben Forta \([http://forta.com/](http://forta.com/)\). On this page we will go step by step how to create this database.

You will need a mysql server \(XAMPP\) and a mysql client\(command prompt, Bash or PowerShell will do\).

You will also need to download the files [https://forta.com/wp-content/uploads/books/0672327120/mysql_scripts.zip](https://forta.com/wp-content/uploads/books/0672327120/mysql_scripts.zip) and unzip them.

::: tip SQL Keywords in capital letters

In this course we will put all the SQL-code in capital letters, this is not necessary but it will make clear to you what is static SQL-code and what are names that you can change.
:::

### Connecting to the database

First start up your database server (XAMPP). Now open your mysql client in the folder where you unzipped the files. You can open the client by typing `PowerShell` in the address bar in your windows explorer. Now type `mysql -u root` in your client.

* `mysql`: this tells your command prompt or powershell to start the mysql client.
* `-u root`: the -u tells the client to log with the given name, in this case 'root'.
* `-p`: the -p tells the client to ask for a password after you pressed enter.
* `-h 127.0.0.1`: this tells the client to connect on ip-address 127.0.0.1, can be used for connection on remote servers, when not specified it will use localhost.

You should be connected now and see

```text
MariaDB [(none)]>
```

MariaDB is the name of the database server that is used in xampp. Between the `[ ]`is the name of the selected database, for the moment we don't have a database selected so it says `(none)`.

Before we create a new database, let's see what is already in here with:

```sql
SHOW databases;
```

This will show a list of available databases.

```text
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| phpmyadmin         |
| test               |
+--------------------+
```

We can see a database named test, let's have a look. Do `USE name;` to select the database with that name. you see that the name of the selected database now `test` is. Do `show tables;`to ask the list of tables in the database. `Empty set (0.00 sec)`, it looks like this database is completely empty, you can leave it or delete it with `DROP DATABASE test;`

### Creating the database

First we will create a completely empty database with:

```sql
CREATE DATABASE forta;
```

Now an empty database with name forta has been created and we will now use that database with:

```sql
USE forta;
```

Once you see that the prompt has changed to: `MariaDB [forta]>`, you can continue. We are now going to create tables and columns in those tables but we are not doing it ourself. To execute a script, use the `SOURCE scriptname;` command, we want to execute the create.sql file we got from the zip folder first.

```sql
SOURCE create.sql;
```

If you get an error like this;

```text
ERROR: Failed to open file 'create.sql', error: 2
```

Check if the create.sql file is in the folder where you started you PowerShell or command prompt.

Now wait until the prompt is back to its normal state. There should be a table in this database now, let's check with `SHOW TABLES;`.

```text
+-----------------+
| Tables_in_forta |
+-----------------+
| customers       |
| orderitems      |
| orders          |
| productnotes    |
| products        |
| vendors         |
+-----------------+
```

To see how a table was made, use `DESC tablename;`so let's test it with `DESC products;`

```text
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| prod_id    | char(10)     | NO   | PRI | NULL    |       |
| vend_id    | int(11)      | NO   | MUL | NULL    |       |
| prod_name  | char(255)    | NO   |     | NULL    |       |
| prod_price | decimal(8,2) | NO   |     | NULL    |       |
| prod_desc  | text         | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
```

In this table we can see what the name, the type, if it can be empty\(Null\), if it is a key and what the default value is for every column. This one doesn't have any default values so it says NULL. To see all of the information inside of a table, you can always use:

```sql
SELECT* FROM tablename;
```

So now we are going to look inside the products table to know what products this company sells.

```sql
SELECT * FROM products;
```

We get this as answer: `Empty set (0.00 sec)`. This means that the company doesn't have any products yet, so let's put some in.

## Filling the database

Again, we are not going to do this ourself but use another script for this:

```sql
SOURCE populate.sql;
```

If we try to execute the command again now we can see the following:

```text
MariaDB [forta]> select * from products;
+---------+---------+----------------+------------+----------------------------------------------------------------+
| prod_id | vend_id | prod_name      | prod_price | prod_desc                                                      |
+---------+---------+----------------+------------+----------------------------------------------------------------+
| ANV01   |    1001 | .5 ton anvil   |       5.99 | .5 ton anvil, black, complete with handy hook                  |
| ANV02   |    1001 | 1 ton anvil    |       9.99 | 1 ton anvil, black, complete with handy hook and carrying case |
| ANV03   |    1001 | 2 ton anvil    |      14.99 | 2 ton anvil, black, complete with handy hook and carrying case |
| DTNTR   |    1003 | Detonator      |      13.00 | Detonator (plunger powered), fuses not included                |
| FB      |    1003 | Bird seed      |      10.00 | Large bag (suitable for road runners)                          |
| FC      |    1003 | Carrots        |       2.50 | Carrots (rabbit hunting season only)                           |
| FU1     |    1002 | Fuses          |       3.42 | 1 dozen, extra long                                            |
| JP1000  |    1005 | JetPack 1000   |      35.00 | JetPack 1000, intended for single use                          |
| JP2000  |    1005 | JetPack 2000   |      55.00 | JetPack 2000, multi-use                                        |
| OL1     |    1002 | Oil can        |       8.99 | Oil can, red                                                   |
| SAFE    |    1003 | Safe           |      50.00 | Safe with combination lock                                     |
| SLING   |    1003 | Sling          |       4.49 | Sling, one size fits all                                       |
| TNT1    |    1003 | TNT (1 stick)  |       2.50 | TNT, red, single stick                                         |
| TNT2    |    1003 | TNT (5 sticks) |      10.00 | TNT, red, pack of 10 sticks                                    |
+---------+---------+----------------+------------+----------------------------------------------------------------+
```

### Game Reviews database

Download the [gamereviews_example.zip](./files/gamereviews_example.zip) file and import the `gamereviews_example.sql` file in your database to get access to the tables for this exercise.

Execute the following command in this project directory with the `mysql` client:

```sql
source gamereviews_example.sql
```

### SpaceX database

Download the [spacex.zip](./files/spacex.zip) file and import the `spacex.sql` file in your database to get access to the tables for this exercise.

Execute the following command in this project directory with the `mysql` client:

```sql
source spacex.sql
```

### A larger test database

Some of the examples use a larger testdatabase, a fake database with employees to be exact. If you want test on this bigger database you can get it over at [datacharmer/test\_db](https://github.com/datacharmer/test_db) on Github. Download the repository as zip or use `git clone`. Select the folder where you saved the repository and start your mysql client here. Now reconnect to your database using the following command:

```sql
mysql -u root
```

We don't need to create a database ourselves because that's what the script does so we only need to run

```sql
SOURCE employees.sql
```

This will take a little longer because this script is way bigger. You can open another client and connect to continue this course while the other client is still running.

So now you have some databases to test on.

**You can now safely remove the getting started, submission and setup instructions. If you would need them, they are forever stored in your git repository history**

<!-- Remove everything until here, including the comment messages -->

## Assignment

### Creating Tables

Create a database called `exercise_03` and create the tables as described below.
Give the queries that will create those tables, and verify there structure with a `DESCRIBE [table]` query.

- Choose the names of the databases and columns. Make there names logical and descriptive.
- Pick the best datatype that will allow only correct data in the tables.
- Don't forget to select or create a primary key for each table.

#### Twitter Messages

Create a table that can contain Twitter messages. The messages have the following properties:

- Username
- Tweet message
- Date and time

Create table query:

```sql

```

`DESCRIBE` result:

```text

```

#### Webshop

Create a table that can store data for a webshop. The webshop sells different types of products using the following properties:

- Product name
- Description
- Price
- Stock (false value if out of stock)

Create table query:

```sql

```

`DESCRIBE` result:

```text

```

#### Clients

Create a table that can contain information about customers or clients. The clients have the following properties:

- First name
- Last name
- Birthday
- Premium (if true, the value could be used to calculate premium discounts)

Create table query:

```sql

```

`DESCRIBE` result:

```text

```

#### Components

When working on electronics projects, a lot of components are used. It is handy to have an overview of what types you have on your shelf. The components have the following properties:

- Type (can only be: resistor, capacitor or coil)
- Value
- Manufacturer
- Stock (a number, the amount of stock on the shelf)

Create table query:

```sql

```

`DESCRIBE` result:

```text

```

#### Computers

Create a table that can manage custom computer configurations. The systems have the following properties:

- Cpu brand (Intel, AMD, ARM or Other)
- Cpu model
- RAM size (in GigaBytes)
- Interfaces (each computer could have _any_ combination of interfaces)
  - Ethernet
  - Wifi
  - USB 3.0
  - USB 2.0
  - Bluetooth
  - Audio jack
- Price
- Release date
- Title
- Description

Create table query:

```sql

```

`DESCRIBE` result:

```text

```

### Report

Don't forget to fill in the [REPORT.md](REPORT.md) at the end of the exercise.
