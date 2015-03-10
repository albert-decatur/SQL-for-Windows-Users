# SQL for Windows (and Mac!) Users
### a visual introduction

SQL is a query language for tables.
It's almost English.
It's like Excel but **easier** and **better**.
You're about to learn it.

# the good stuff - pictures

#### we'll get around to [the wordy part](#the-wordy-part) later

#### OK but how do I actually get started?  that's why we have . . .


# the wordy part


The basic SQL workflow is:

1. get a [CSV](https://en.wikipedia.org/wiki/Comma-separated_values), TSV, or other [plain text](https://en.wikipedia.org/wiki/Plain_text) table
2. make an empty SQL database
3. import your table into the database
4. perform a query
5. export the output of your query

SQL is much more of course, but we won't go further than importing and joining three tables, and exporting a query about them.
You can do a **lot** with those skills.

Find an actual SQL tutorial [here](https://github.com/tthibo/SQL-Tutorial).
It's good.

## inputs

SQL is the easy part.
It can be hard to prepare your input tables before you even get them into your database.
All the cool kids make sure that their input tables meet these conditions:

  1. [machine readable](http://webarchive.okfn.org/okfn.org/201404/opendata/glossary/#machine-readable)
    1. start with plain text. A PDF is not a machine readable table!  HTML is not a machine readable table! XLS is not a machine readable table! 
      1. hint: save .xls and .xlsx as CSV
    1. the whole input file is just a single table
    1. the first line is a header with the name of each column
    1. each column has a name, and no name is repeated
    1. each column has internally consistent contents
      1. for example, don't put commas, periods, or '$' in columns that have amounts of money. It's for numbers!
    1. there is an ID column with unique identifiers for all of the rows in the table. Preferably it's the first column!
  1. no cases of [delimiter collision](https://en.wikipedia.org/wiki/Delimiter#Delimiter_collision)
    1. for example, if your input is CSV, then every time you see a comma it better mean there's a new column immediately to the right.
      1. it's ok to not follow this rule if your delimiters are properly escaped, for example the offending "cell" is surrounded by double quotes
  1. character encoded as [UTF-8](https://en.wikipedia.org/wiki/UTF-8)

See examples of what **not** to do [here](http://okfnlabs.org/bad-data/).

## let's do this thing 

1. Go to [sqlitebrowser.org](http://sqlitebrowser.org/) and download the right executable for your operating system. Should be .exe for Windows and .dmg for Mac.
1. install it!
1. open it!
1. now follow our screenshots above.  we'll be using AidData's geocoded World Bank development aid research release.
  1. the original data can be found [here](http://aiddata.org/geocoded-datasets).
  1. find these three tables as tab separated values (TSV) under our data folder.
1. and follow along with the pictures above!  good luck and happy querying
