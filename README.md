# Nara
SSIS, SSRS, SSAS Projects
AdventureWorksDW2017 Archive and Purge Solution:
Deployment Instructions:
Please execute below scripts in order. Please note that these scripts have been developed to work in SQL 2012 version onwards. There is still room to optimize these scripts in case of needing to deploy later versions of SQL versions.
1.	CREATE CONFIGURATION ENTRIES.sql   (Update the desired Batch Size and Retention period before execute the script)
a.	This script would clone BalyBi database and name it as “AdventureWorksDW2017_Archive” database.
b.	It also creates “[dbo].[ArchiveFactTablesConfig]”, which would help to store configurations like RETENTION_PERIOD, KEY_COLUMNS etc. of each fact table. It also would add default entries to the configuration table.
c.	It also creates “dbo.BallyArchive_Log” table under AdventureWorksDW2017 database to log entries when job executes.
d.	It also creates “[dbo].[fnSplit]” function, which would be used to stored procedure.
2.	sp_PurgeFactTablesData.sql 
a.	This script creates the new schema named  “ARCHIVE” under AdventureWorksDW2017 Database
b.	It also creates “sp_PurgeFactTablesData” stored procedure under AdventureWorksDW2017 Database.
i.	This stored procedure accepts @TABLE_NAME as a parameter
ii.	It would check whether table is available in both online and archive databases, if not it would fail.
iii.	It would also check whether column names are same between both online and archive databases, if not it would fail.
iv.	It would load the data from online to archive based on retention period and batch size and purge data from online database on configuration settings specified in “[dbo].[ArchiveFactTablesConfig]” table.
3.	“Archive Fact Data.dtsx”  
a.	Copy the package to the drive 
b.	Create a job from the SQL SERVER AGENT job to call the “Archive Fact Data.dtsx” and schedule the job if needed. 
Testing Instructions:
1.	Open Package—Check the connections are pointed to the correct server
2.	Update the desired BATCH_SIZE and RETENTION_PERIOD for each table according to the requirement
3.	Check the row count of in all the tables in both AdventureWorksDW2017 database and archive database.
4.	For further details refer below website
http://www.myknowledgesharing.com/2019/11/how-to-implement-archive-and-purge.html
