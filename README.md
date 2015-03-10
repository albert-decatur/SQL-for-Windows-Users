# SQL for Windows (and Mac!) Users
### a visual introduction

SQL is a query language for tables.
It's almost English.
It's like Excel but **easier** and **better**.
You're about to learn it.

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
Note for nerds: SQLite is a bit of a speciality flavor of SQL.  Kind of like chunky monkey.  Not for everybody, but it suites this exercise well because it has no configuration or permissions to deal with and is small.

## inputs

SQL is the easy part.
It can be hard to prepare your input tables before you even get them into your database.
All the cool kids make sure that their input tables meet the following conditions.
Luckily our inputs under the data folder do already!

  1. [machine readable](http://webarchive.okfn.org/okfn.org/201404/opendata/glossary/#machine-readable)
    1. start with plain text. A PDF is not a machine readable table!  HTML is not a machine readable table! XLS is not a machine readable table! 
      1. hint: save .xls and .xlsx as CSV
    1. the whole input file is just a single table
    1. the first line has the name of each column, and nothing else.
      1. this is called a header.  there is only one header line (no merge cells allowed!)
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
1. now follow our screenshots.  we'll be using AidData's geocoded World Bank development aid research release.
  1. the original data can be found [here](http://aiddata.org/geocoded-datasets).
  1. find these three tables as tab separated values (TSV) under our data folder.
1. follow along with the pictures!  good luck and happy querying.


# the good stuff - pictures

![example](img/2015-03-10_16_07_12.png)
![](img/2015-03-10_16_07_49.png)
![](img/2015-03-10_16_12_12.png)
![](img/2015-03-10_16_14_35.png)
![](img/2015-03-10_16_14_46.png)
![](img/2015-03-10_16_18_25.png)
![](img/2015-03-10_16_19_10.png)
![](img/2015-03-10_16_19_35.png)
![](img/2015-03-10_16_20_47.png)
![](img/2015-03-10_16_21_11.png)
![](img/2015-03-10_16_21_29.png)
![](img/2015-03-10_16_22_09.png)
![](img/2015-03-10_16_22_21.png)
![](img/2015-03-10_16_47_05.png)
![](img/2015-03-10_16_47_18.png)
![](img/2015-03-10_16_47_31.png)
![](img/2015-03-10_16_49_07.png)
![](img/2015-03-10_16_49_32.png)
![](img/2015-03-10_16_49_51.png)
![](img/2015-03-10_16_50_09.png)
![](img/2015-03-10_16_53_16.png)
![](img/2015-03-10_16_53_46.png)
![](img/2015-03-10_16_54_09.png)
![](img/2015-03-10_16_55_47.png)
![](img/2015-03-10_16_58_20.png)
![](img/2015-03-10_16_58_48.png)
![](img/2015-03-10_17_05_07.png)
![](img/2015-03-10_17_06_01.png)
![](img/2015-03-10_17_15_46.png)
![](img/2015-03-10_17_16_53.png)
![](img/2015-03-10_17_28_38.png)
![](img/2015-03-10_17_29_58.png)
![](img/2015-03-10_17_39_05.png)

```SQL
SELECT
	p.status,
	sum(t.transaction_value) as sum_commitments_USD_2011
FROM
	projects as p
	INNER JOIN transactions as t
	ON p.project_id = t.project_id
WHERE
	p.country = 'Cambodia'
	AND t.transaction_value_code = 'C'
GROUP BY
	p.status;

```

```SQL
SELECT
	p.status,
	sum(t.transaction_value) as sum_commitments_USD_2011
FROM
	projects as p
	INNER JOIN transactions as t
	ON p.project_id = t.project_id
WHERE
	p.country = 'Cambodia'
	AND t.transaction_value_code = 'C'
	AND p.sector_value LIKE '%health%'
GROUP BY
	p.status;
```

```SQL
SELECT
	p.project_id,
	p.country,
	p.project_title,
	t.transaction_isodate,
	t.transaction_value
FROM
	projects as p
	INNER JOIN transactions as t
	ON p.project_id = t.project_id
ORDER BY
	t.transaction_value DESC
LIMIT 10;
```

## tidbits

* what's another amazing way to easily join plain text tables and do so much more?
  * answer: [CSVKit](https://csvkit.readthedocs.org/en/0.9.0/)
