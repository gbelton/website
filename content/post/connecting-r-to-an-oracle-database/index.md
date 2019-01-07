---
title: Connecting R to an Oracle Database
author: ''
date: '2017-05-22'
slug: index
categories:
  - R
tags:
  - R
image:
  caption: ''
  focal_point: ''
---
<p>R is a very popular language for doing analytics, and particularly statistics, on your data. There are a number of R functions for reading in data, but most of them take a delimited text file (such as .CSV) for input. That's great if your existing data is in a spreadsheet, but if you have large amounts of data, it's probably stored in a<a href="https://en.wikipedia.org/wiki/Relational_database"> relational database</a>. If you work for a large company, chances are that it is an Oracle database.</p>

<p>The most efficient way to access an Oracle database from R is using the RODBC package, available from CRAN.&nbsp;If the RODBC package is not installed in your R environment, use the <i>install.packages("RODBC")</i> command to install it. ODBC stands for Open DataBase Connectivity, an open standard application programming interface (API) for databases.&nbsp;ODBC was created by the SQL Access Group and first released in September, 1992. Although Microsoft Windows was the first to provide an ODBC product, versions now exist for Linux&nbsp;and Macintosh platforms as well. ODBC is built-in to current versions of Windows. If you are using a different operating system, you'll need to install on OBDC driver manager.</p>
<p>Before you can access a database from R, you'll need to create a Data Source Name, or DSN. This is an alias to the database, which provides the connection details. In Windows, you create the DSN using the ODBC Source Administrator. This tool can be found in the Control Panel. In Windows 10, it's under System and Security -&gt; Administrative Tools -&gt; ODBC Data Sources. Or you can just type "ODBC" in the search box. On my system, it looks like this:</p>

![](/post/connecting-r-to-an-oracle-database/ODBC-1.png)

<p>As you can see, I already have a connection to an Oracle database. To set one up, click Add, and you'll get this box:</p>

![](/post/connecting-r-to-an-oracle-database/ODBC2.png)

<p>Select the appropriate driver (in my case, Oracle in OraDB12Home1) and click the Finish button. A Driver Configuration box opens:</p>

![](/post/connecting-r-to-an-oracle-database/ODBC3.png)


<p>For "Data Source Name," you can put in almost anything you want. This is the name you will use in R when you connect to the database.</p>
<p>The "Description" field is optional, and again, you can put in whatever you want.</p>
<p>TNS Service Name is the name that you (or your company data base administrator) assigned when configuring the Oracle database. And "User ID" is your ID that you use with the database.</p>
<p>After you fill in these fields, click the "Test Connection" button. Another box pops up, with the TNS Service Name and User ID already populated, and an empty field for your password. Enter your password and click "OK." You should see a "Connection Successful" message. If not, check the Service Name, User ID, and Password.</p>
<p>Now you are ready to connect R to the database.</p>
<p>Here's the R code that you need:

<pre>
library(RODBC)

# Create a connection to the database called "channel"
channel <- odbcConnect("DATABASE", uid="USERNAME", pwd="PASSWORD")

# Query the database and put the results into the data frame
# "dataframe"

 dataframe <;- sqlQuery(channel, "
 SELECT *
 FROM
 SCHEMA.DATATABLE")
# When finished, it's a good idea to close the connection
odbcClose(channel)
</pre>

<p>A couple of comments about this code are in order:</p>
<p>First, I don't like the idea of having a password appear, unencrypted, in the R program. One possible solution is to prompt the user for the password before creating the connection:
</p><pre>pswd&nbsp;&lt;- readline("Input Password: ")
channel &lt;- odbcConnect("DATABASE", uid="USERNAME", &nbsp;pwd=pswd)</pre>
<p>This will enable the connection to be made without compromising the security of the password.</p>
<p>Second, the sqlQuery will pass to Oracle whatever is inside the quotation marks. This is the workhorse function of the RODBC package.&nbsp;The term ‘query’ includes any valid SQL statement including table creation, updates, etc, as well as ‘SELECT’s.</p>
<p>Finally, I should mention that R works with data that is loaded into the computer's memory. If you try to load a really huge database into memory all at once, it will a) take a very long time, and b) possibly fail due to exceeding your computer's memory capacity. Of course, relational database systems like Oracle are the natural habitat of very large data sets, so that may be your motivation for connecting R to Oracle in the first place. Carefully constructed SQL Queries will let Oracle do the work of managing the data, and return just the data that R needs for performing analytics.</p>
<p>Writing SQL Queries is beyond the scope of this blog post. If you need help with that, there are plenty of free tutorials on the web, or you might find this book helpful: <a href="http://amzn.to/2qGuFYa">Oracle 12c for Dummies</a></p>
<p><a href="https://www.amazon.com/Oracle-12c-Dummies-Chris-Ruel/dp/1118745310/ref=as_li_ss_il?ie=UTF8&amp;qid=1495311290&amp;sr=8-12&amp;keywords=oracle+database&amp;linkCode=li2&amp;tag=gerald09-20&amp;linkId=4d4a8cc765a27634d6aaedd20e8c74b1" target="_blank" rel="noopener noreferrer"><img src="//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&amp;ASIN=1118745310&amp;Format=_SL160_&amp;ID=AsinImage&amp;MarketPlace=US&amp;ServiceVersion=20070822&amp;WS=1&amp;tag=gerald09-20" border="0"></a><img style="border: none !important; margin: 0px !important;" src="https://ir-na.amazon-adsystem.com/e/ir?t=gerald09-20&amp;l=li2&amp;o=1&amp;a=1118745310" alt="" width="1" height="1" border="0"></p>