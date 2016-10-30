---
layout: post
title: "VSTS for database professionals"
date: 2006-06-16T10:12:00Z
modtime: 2006-06-16T10:12:00Z
pubdate: 2006-06-16T10:12:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2006/06/16/12511.aspx"
---


<p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">Today I had a first look at the <a href="http://msdn.microsoft.com/vstudio/teamsystem/dbpro/">Visual Studio Team System for Database professionals</a>.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">As my typical developer client contained Sql2005 developer edition after the flawless install I was stuck immediately when I tried to create a Sql2005 Project. From the READ ME it appeared that this version is dependant on SqlExpress. Those guys must be kidding. After trying to add a client alias to my Sql2005 configuration I checked the Tools|Options dialog. In the Database Tools| Data Connections node, I removed the SQLEXPRESS text, restarted VS2005 and voila, it worked.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">The first thing to start with is the Import Database schema. A simple dialog lets you select a database. This dialogs still loves SQLEXPRESS because it only showed .mdf files but clicking New Connection brings you quickly to any Sql2005 (or Sql2000) instance. Bare in mind that cross developing is not supported.</font></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">After the import the solution explorer tree is populated similar when you use the Server Explorer.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">For Adding a column to a table I tried to right-click on the columns node. It showed a nice 'Select template' dialog, it was however not populated with any items. Also right-clicking on the table didn't bring up Add Column option. I just opened the table (double-click) and added the column by hand to the script. When I saved the file, the Column was also added to the solution explorer tree. A little bit contra-intuitive but the script interpreting capabilities are ok.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">The data generation plans do as they promise: the tables in your schema gets populated with data, randomly generated. The datagenerator stores this is an xml file. The only way I could find to change the generated data was to actually edit the xml file. The grid that shows the generated data is sadly enough not editable. Using the databound generator is a better option in that case. If you need to custom generate data you can do this by handcrafting a assembly and coding the generation process. The data can be pushed into a database from the IDE.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">One of the extremely useful things is the fact that the database project is a MSBUILD project file. This enables the deployment/update of your databases during the daily/continues integration builds.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">A much needed option is schema compare and data compare. The schema compare is capable of comparing database projects and real databases (both as sources and/or targets). The result is a DDL script that will change the target database in such a way that it matches the source schema. The schema comparer doesn't take into account the retention of data in the target schema.</font></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">Therefore a second option exists: The Data Compare. It does what it promises; it compares the data from the source against the target and generates a DDL script. In the Data Compare window you can deselect certain rows which update the script, leaving that particular row untouched.</font></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">Having tools for schema- and datacompare is already great. The separation of this important functionality might not always be the most efficient way to create migration scripts from one database to another.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">Unit testing your database is a real pain. And it is still debated if you need to unit test your database if you are already running unit tests on your datalayer or buisnesslayer. Whatever the opinions are on that subject (and I will not get philosophical) the reason that it is time consuming can be skipped. Right-Click on the stored procedure gives you the option to Create a Unit Test. The IDE creates a C# Unit test project where it requires you to indicate the database connection (the real test database), the databaseproject and the datageneration file to use.</font></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">With that done you simply indicate for the Stored Procudure under test what the expected outcome is. Expected outcomes can be Rowcount, filled resultset, empty resultset, scalar value etc. All this is done without the need to program a single line of code. All is done in the outcome grid, and setting the correct values in the properties pane. In case you need to you can change the TSQL that is called during the test run.</font></p><p style="MARGIN: 0in 0in 0pt"></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">My initial thoughts on the added value where somewhat grim. But I must admit that this is going in a very good direction. The tool adds much needed functionality during data-driven development. What is missing (and I think Dennis will disagree with that) is a Designer for a graphical representation of the schema. There are cases where a picture tells you more than 1000 words. And adding some Logical modelling capabilities will tie the business-analysts to the VS2005 experience.</font></p><p style="MARGIN: 0in 0in 0pt"><font size="3" face="Times New Roman">I'm going to test-drive this stuff now for a while to see if it really stands-up to the promises.</font></p><p style="MARGIN: 0in 0in 0pt"></p>