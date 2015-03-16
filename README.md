# SQL for Windows (and Mac!) Users
### a visual introduction

SQL is a query language for tables.
It's almost English.
It's like Excel but with SQL it's **easier** to do multi-step tasks, and SQL is much **better** at handling larger datasets.
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
Note for nerds: SQLite is a bit of a specialty flavor of SQL.  Kind of like chunky monkey.  Not for everybody, but it suites this exercise well because it has no configuration or permissions to deal with and is small.

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
      1. it's OK to not follow this rule if your delimiters are properly escaped, for example the offending "cell" is surrounded by double quotes
  1. character encoded as [UTF-8](https://en.wikipedia.org/wiki/UTF-8)

See examples of what **not** to do [here](http://okfnlabs.org/bad-data/).

## let's do this thing 

1. Go to [sqlitebrowser.org](http://sqlitebrowser.org/) and download the right executable for your operating system. Should be .exe for Windows and .dmg for Mac.
1. install it!
1. open it!
1. now follow our screen shots.  we'll be using AidData's geocoded World Bank development aid research release.
  1. the original data can be found [here](http://aiddata.org/geocoded-datasets).
  1. find these three tables as tab separated values (TSV) under our data folder.
1. follow along with the pictures!  good luck and happy querying.


# the good stuff - pictures

Open SQLite Database Browser

![](img/2015-03-10_16_07_12.png)

Make a new database - pick a location to save it in.  Use the extension ".sqlite".

![](img/2015-03-10_16_07_49.png)

New databases need at least one table. We're not ready to make one yet so we'll just fake it and get rid of this one later.

![](img/2015-03-10_16_12_12.png)

New tables need at least one column.  We'll fake this too.

![](img/2015-03-10_16_14_35.png)

You can see your useless new table in the Database Structure tab.

![](img/2015-03-10_16_14_46.png)

Let's import a table we actually care about!

![](img/2015-03-10_16_18_25.png)

You'll get interrupted when you try to import. Go ahead and save that useless table. It'll be gone soon but whatever.

![](img/2015-03-10_16_19_10.png)

OK, back to importing a table that __matters__. Go to this repo's folder and navigate to the data folder, or download the raw data from github directly.

![](img/2015-03-10_16_19_35.png)

Choose the projects.txt table to start. This is a plain text, machine readable table like we talked about earlier.

![](img/2015-03-10_16_20_47.png)

At first it will look like the table is broken.  That's because the default column delimiter is assumed to be commas.  Just change it to tabs.

![](img/2015-03-10_16_21_11.png)

That looks much better! Also, extract field names from the header.  That's the first line of the plain text table we're importing.

![](img/2015-03-10_16_21_29.png)

Just like it Excel, go ahead and drag out your columns to be wider and give them a look, or double click the column divider lines to do it automatically. Get a sense for your data!

![](img/2015-03-10_16_22_09.png)

Name this table "projects" after the input.

![](img/2015-03-10_16_22_21.png)

Delete that pesky starter table. Making it in the first place is not necessary when doing SQL at the command line, or in some other graphical systems. Just a quirk of this particular program.

![](img/2015-03-10_16_47_05.png)

Back in the Database Structure tab, expand your projects table to look at all the columns. We didn't specify data types (like TEXT, NUMERIC) but that's OK for now.

![](img/2015-03-10_16_47_18.png)

Go over to the Browse tab to get a look at the data in this table.

![](img/2015-03-10_16_47_31.png)

Now it's time to learn SQL **for real**.  You may have used it before without knowing, like in ArcGIS's "select by attributes."  SQL is easy!  The statement shown in the picture below just says "show me the first 10 project_id column values from the projects table that you find whose country is Cambodia."
Check out the output in the "Data returned" box below.
Here's the actual SQL, written in a more legible style:

```SQL
SELECT
	project_id
FROM
	projects
WHERE
	country = 'Cambodia'
LIMIT
	10
;
```
Notice that SQL is typically written with functions (the commands) in capital letters to distinguish them from table names, column names, and values that come from the table that you might refer to.
A few tricky things here:

* the order of clauses matters
  * for example, the WHERE clause must come after the FROM clause
* values that come from your data are typically quoted, like 'Cambodia'
* you really do need a ';' at the end
  * software that lets you not put the ';' at the end just promotes bad habits. this is the programming equivalent of not brushing your teeth before bed.

![](img/2015-03-10_16_49_07.png)

Now let's get just **3** project_ids where the recipient country is Cambodia with this code:

```SQL
SELECT
	project_id
FROM
	projects
WHERE
	country = 'Cambodia'
LIMIT
	3
;
```

Spot the difference?
Now you know what the LIMIT function does.

![](img/2015-03-10_16_49_32.png)

How many projects are in this dataset where the recipient country is Cambodia?
Let's find out using COUNT with this code:

```SQL
SELECT
	COUNT(project_id)
FROM
	projects
WHERE
	country = 'Cambodia'
;
```

![](img/2015-03-10_16_49_51.png)

This table is supposed to have a single row (aka "record") per project.
Let's make sure that our project_ids in the projects table do not repeat.
If that were not the case IDs would repeat and our COUNT(project_id) would be higher than our COUNT(DISTINCT(project_id)).
Let's get the count of unique project_ids with the DISTINCT function:

```SQL
SELECT
	COUNT(DISTINCT(project_id))
FROM
	projects
WHERE
	country = 'Cambodia'
;
```

OK it's the same!
That means there is only one project_id per row.

![](img/2015-03-10_16_50_09.png)

What about the dates that projects occur?
Let's limit our query according to projects that started 2001 or after:

```SQL
SELECT
	COUNT(DISTINCT(project_id))
FROM
	projects
WHERE
	country = 'Cambodia'
	AND start_actual_isodate > '2001-01-01'
;
```
There is a date data type but we didn't use it here.
We didn't use the date type, but it doesn't matter because we've used ISO dates in the form YYYY-MM-DD so they sort numerically.

There are only 25 projects this time.
That means that 3 projects had start dates before the date we specified.

![](img/2015-03-10_16_53_16.png)

What if we added to the WHERE clause by demanding even more constraints?
Let's ask for everything we did before, but this time add that the status must be "Closed".
Note that case sensitivity matters, so "Closed" is different from "closed".
Spelling matters too!
Computers don't actually understand human speech and writing.
They only follow the rules you give them.
OK, let's add that status constraint to our query:

```SQL
SELECT
	COUNT(DISTINCT(project_id))
FROM
	projects
WHERE
	country = 'Cambodia'
	AND start_actual_isodate > '2001-01-01'
	AND status = 'Closed'
;
```

Only 20 projects this time!
That means that 5 of the previously selected projects had a status other than Closed.

![](img/2015-03-10_16_53_46.png)

Let's go for our old constraint **minus** the demand that the recipient be Cambodia.
Should be lots more projects:

```SQL
SELECT
	COUNT(DISTINCT(project_id))
FROM
	projects
WHERE
	start_actual_isodate > '2001-01-01'
	AND status = 'Closed'
;
```

2344 projects!
Clearly there are tons of projects outside Cambodia.

![](img/2015-03-10_16_54_09.png)

Time to add the transactions table so we can really get cooking.
This will allow us to join information about projects with their transactions, date by date.
Aid transactions in this case are commitments (promises of money or services) and disbursments (actually giving out those money or services).

Add the transactions table the same way you did the projects table, but give it the name "transactions".

![](img/2015-03-10_16_55_47.png)

Check out what fields you got in that transactions table.

![](img/2015-03-10_16_58_20.png)

Browse the data in the transactions table.

![](img/2015-03-10_16_58_48.png)

Time for your first JOIN!
It's amazing we've come this far.
This is where the real power lies.
Are you excited?
More importantly, are oyu **ready**??
Let's find out.

This query will return a single joined transaction_id and its corresponding project_id.
Remember, there are many transactions per project.
The kind of relationship is crucial to understanding the implications of your query.
Kinds of relationships:

* one-to-one
  * for example, one transaction_id per transactions
* one-to-many
  * for example, one project to many transactions
* many-to-many
  * for example, many recipients to many donors (each donor can have many recipients, each recipient can have many donors)

The way you build tables in a database, or even just plain text tables like CSV, should be with these relationships in mind.
Here's our SQL to find one example transaction_id along with its corresponding project_id.
Note that the join is able to happen because **both** tables have project_id columns.

```SQL
SELECT
	projects.project_id,
	transactions.transaction_id
FROM
	projects
	INNER JOIN transactions
	ON projects.project_id = transactions.project_id
LIMIT
	1
;
```

![](img/2015-03-10_17_05_07.png)

Awesome!
You did a SQL join.
This is powerful.
But let's write that last query a little simpler.
We'll alias the table names to letters so we don't have to type as much:

```SQL
SELECT
	p.project_id,
	t.transaction_id
FROM
	projects as p
	INNER JOIN transactions as t
	ON p.project_id = t.project_id
LIMIT
	1
;
```

![](img/2015-03-10_17_16_53.png)

Now let's get serious by learning the GROUP BY function.
Move over Excel.

GROUP BY lets you aggregate your fields according to a field.
It's kind of like pivot tables in Excel.
Be sure to include the column you grouped by in your output so you know what you were grouping by!
For exampe, you can group transaction sums by year.
You can even GROUP BY multiple columns at once!
That way you can easily get things like recipient/years given by a donor.
That would answer questions like these in a single statement: "How much money did every donor give every recipient in every year in the dataset?"

In this example we'll GROUP BY the status column and get counts of Cambodia commitment transactions according to their status.
This is really brining it all together!

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

The output tells us that Cambodia recieved $114,500,000 2011 USD from World Bank in commitment transactions for Active projects over the entire dataset, and $436,580,000 USD 2011 for Closed projects.
One crucial idea is the **scope** of your dataset.
Our input data is just for the World Bank as a donor, and only for some years!
Another obvious issue with a column like status is **when** your dataset was compiled!
If most of the data is from a long time ago you should expect most projects to be Closed.
This is not a sign that the World Bank has given up on any funding.
Always keep the scope in mind.
Good metadata will tell you the scope, when the dataset was made, who made it, what the input data was, and many other crucial concerns.

![](img/2015-03-10_17_28_38.png)

Let's get the same info as before, but just for health sector projects.
We're going to cheat on this a bit and use the LIKE operator to find sector names that mention the word "health".
LIKE is actually case sensitive (upper and lower case) but you can potentially use the LOWER() function on the sector column to match more instances of "health".

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

Remember, we used LIKE because the sector field is not based on a known list of values.
It's free text - in other words, a total free for all!
But LIKE can help get us an **idea** of what's out there.
The percent signs around "health" mean that any number of any characters can come to the left or the right of the word.
So examples like "suphealth" and "healthyo" are counted as matches.

Another similar operator is MATCH.
To get truly precise matches read about the REGEXP operator.
Remember, other implementations of SQL exist and will have slightly different rules from SQLite.

![](img/2015-03-10_17_29_58.png)

Finally, let's get real.
Find the project_id, recipient country name, transaction ISO date, and transaction value for the top 10 largest transactions.
Bonus: is this query limited to just commitment transactions?
How do you know?

Super bonus: how would you use GROUP BY to modify this query to show the top 10 recipient countries by sum of commitments instead of the top 10 transactions?

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


![](img/2015-03-10_17_39_05.png)


## tidbits

* what's another amazing way to easily join plain text tables and do so much more?
  * answer: [CSVKit](https://csvkit.readthedocs.org/en/0.9.0/)
