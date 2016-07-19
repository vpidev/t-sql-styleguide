# t-sql-styleguide
Style Guide for SQL Server Development for VPIDEV databases

## Table of Contents

1. [Naming](#naming)
1. [Standard Field Sizes](#standard-field-sizes)
1. [Comments](#comments)
1. [Table Design](#table-design)
1. [Security](#security)


## Naming

### Naming Guidelines

###### [Style [S001](#style-s001)]

- Do not abbreviate names - use full english word

*Why?*: Abbreviations are ambigious, hard to read, and difficult for new developers to understand

###### [Style [S002](#style-s002)]

- Capitalize the first letter of each word, and separate words by underscores

*Why?*: SQL code is not case sensitive, and underscores ensure words are readable

### Table Names

###### [Style [S003](#style-s003)]

- All tables should include the schema, dbo is not to be used
 in the table name

*Why?*: dbo schema tends to collect junk objects. Never using the dbo schema makes these stand out so they can be removed

*Why?*: Schema names keep logic areas organized

###### [Style [S004](#style-s004)]

- Table names should be singlular, e.g. Production_Batch instead of Production_Batches

*Why?*: Consistency - Querying a Purchase_Order, should not need to think about whether this table is singluar or plural

### Field Names

###### [Style [S005](#style-s005)]

- Primary Key Fields should include _PK at the end of the field name

*Why?*: Easily identify the column that uniquely identifies a record

*Why?*: unambigous column names during joins

###### [Style [S006](#style-s006)]

- Foreign Key Fields should match the primary key name, include _FK at the end of the field name in place of _PK

*Why?*: unambigous column names during joins

###### [Style [S007](#style-s007)]

- Special Foreign keys may be referenced as _XK when 
    - multiple foreign key columns point to the same primary key, e.g. `Home_Number_XK`,  `Work_Number_XK`
    - the primary field name doesn't make sense semantically

*Why?*: Referencing as _FK may confused confuse developers when there is no matching _PK in the related table

###### [Style [S008](#style-s008)]

- Bit fields should be prefixed with `Flag_`

*Why?*: easily identify boolean/bit fields when viewing / querying tables

###### [Style [S009](#style-s009)]

- Datetime fields should be prefixed with `Date_`

*Why?*: Most date name naturally include "date" in the name, and this style ensure the date is always in the same location

### Sproc, Function Names

###### [Style [S010](#style-s010)]

- Sproc format should be `<schema>.<entity name>_<action_name>`
    - Entity Name is the primary table being queried/modified by the sproc
    - Use the full table name
    - Action name
        - Insert
        - Update
        - Delete
        - Get_By_<descriptor> - secondary key as main sproc parameter
        - Get_By_Key -primary key is the main sproc paramenter
        - List - return full list of items, common with lookup tables

###### [Style [S011](#style-s011)]

- Sprocs for auto-complete operations should use the format `<schema>.<table name>_<field_name>_Autocomplete`

*Why?*: Autocomplete operations are specialized and must be fast

*Why?*: There may be a number of possible autocomplete operations, and this keeps things tidy


### View Names

###### [Style [S012](#style-s012)]

- View names must begin with `View_`

*Why?*: Keep view names distinct from sprocs, table names

### Schema Names

###### [Style [S013](#style-s013)]

- Schema names should be three-letter abbreviations of a functional area e.g. Accounting -> ACT

*Why?*: Brevity

### Constraint names

###### [Style [S014](#style-s014)]

- Primary Keys must begin with `PK_`

###### [Style [S015](#style-s015)]

- Foreign Keys must begin with `FK_`

###### [Style [S016](#style-s016)]

- Unique Indexes must begin with `UX_`

###### [Style [S017](#style-s017)]

- Non-Unique Indexes must begin with `IX_`

### Job names

###### [Style [S018](#style-s018)]

- Job Name should aim to use existing table, sproc, or SSIS Package names

*Why?*: Make deployment, debugging, administration easier by being consisent and mimnimze the number of handles in the system.  

###### [Style [S019](#style-s019)]

- Jobs running SSIS packages should follow the format `SSIS_<job name>`

*Why?*: Identify packages moving data in/out of server


###### [Style [S020](#style-s020)]

- Jobs running SSRS subscriptions should follow the format `SSRS_<Subscription_Name`

*Why?*: Group reporting operations together

###### [Style [S021](#style-s021)]

- Jobs running Sprocs should follow the format `Data_Processing_<sproc name>`

*Why?*: Easily identify the sproc without having to open the job

###### [Style [S022](#style-s022)]

- Jobs running Maintenance plans should follow the format `Maintenance_Plan<maintenance plan name>`

*Why?*: Maintenance Plans use SQL Agent, and need to isolate these

###### [Style [S023](#style-s023)]

- jobs executing system commands or sprocs for server operations should follow the format `Utility_<descriptor`

*Why?*: group misc system functions together

### SSIS Package names

###### [Style [S024](#style-s024)]

- Project/solution names should indicate the external source of data e.g. `SalesForce` or `Epicor`

*Why?*: Organize a potentially large number of external sources into separate areas

###### [Style [S025](#style-s025)]

- All SSIS Packages connecting to an external source should be under the same project/solution

*Why?*: Easily locate all packages connected with a given external source

###### [Style [S026](#style-s026)]

- individual packages (*.dtsx files) should follow the format `<descriptor/table><direction>_<database>`
    - descriptor identifies the specific table(s) being accessed. use CamelCase for the descriptor
    - direction indicates whether this is an import or an export. POV is from the server running the SSIS package
        - an import is moving data into the server hosting the package
        - an export is moving data to another server (not hosting the package)

*Why?*: Quickly assess the primary purpose of the package by reading the name

*Why?*: When hosted inside of a job, the ssis package name becomes part of the descriptor of the job name

**[Back to top](#table-of-contents)**


## Standard Field Sizes

### Decimal

###### [Style [S027](#style-s027)]

- Use `numeric(12,5)` for all decimal values

*Why?* 5 Decimal places is almost alway more than enough precision

*Why?* Reduce unnecessary migrations to widen fields from 1 to 2 decimal places


### String

###### [Style [S028](#style-s028)]

- Use `varchar(25)` for strings 1-25 charcters

*Why?* Reduce unnecessary migrations to widen fields


###### [Style [S029](#style-s029)]

- use `varchar(100)` for medium length fields

*Why?* Reduce unnecessary migrations to widen fields

###### [Style [S030](#style-s030)]

- use `varchar(max)` for multi-line text input / storage

*Why?* Trying to guess 100,200,300 fields is limiting, and can be a source of unexpected truncation / errors

### Boolean

###### [Style [S031](#style-s031)]

- use `bit` for boolean values

### Date

###### [Style [S032](#style-s032)]

- use `datetime2` for all date, time values

*Why?* Greatest amount of flexibility for date storage and manipulation


**[Back to top](#table-of-contents)**


## Comments

### Sprocs

###### [Style [S033](#style-s033)]

 - All sprocs must include a comment header in the following format

```sql
 --********************************************************************************
 --Created By:   Brian Vander Plaats
 --Create Date:  7/18/16
 --Case:                 10000
 --Description:  Retrieve List of Units of Measure
 --
 --********************************************************************************
```

*Why?*  Identify source of sproc without relying on audting mechanisms 

*Why?* Place to provide summary information about the sproc

###### [Style [S034](#style-s034)]

 - Changes to sprocs after deployment **must** include a change note in the format:

```sql
 --********************************************************************************
 --Created By:   Brian Vander Plaats
 --Create Date:  7/18/16
 --Case:                 10000
 --Description:  Retrieve List of Units of Measure
 --
 -- 7/19/2016 10001 bvp - Add filter parameter
 --********************************************************************************
```

*Why?* Providing a comment makes it much simpler to compare database schemas between servers

*Why?* Quickly diagnose issues - can see quickly if problem is likely due to a recent change

**[Back to top](#table-of-contents)**


1. [Table Design](#table-design)


## Table Design

### Standard Fields

###### [Style [S035](#style-s035)]

- All tables must contain a integer primary key field

*Why?* Enforce Data integrity through PK-FK relationships

*Why?* Provide a reliable mechanism for querying / deleting a single record

*Why?* Candidate Keys are likely to change in the future

###### [Style [S036](#style-s036)]

- Tables should be clustered on the primary key field

*Why?* display of records from new -> old based on integer keys is the easiest default

*Why?* The primary key becomes a pseudo "record number" which is useful in a variety of contexts - not necessarily in application logic, but in adminstration tasks e.g. select the next 10 records after key 10000

###### [Style [S037](#style-s037)]

- Each table must include a Date_Created field, with a default of GetDate()

*Why?* Record creation time is always useful information to have

*Why?* Using a default means there is no situation where the program administrator forgets to set the field values

###### [Style [S038](#style-s038)]

- Each table must include a Date_Modified field, set via an update trigger

*Why?* Record modifciation time is always useful information to have

*Why?* Using a trigger means there is no situation where the program administrator forgets to set the field values

### Normalization

###### [Style [S039](#style-s039)]

- OLTP Tables should generally be in [3rd Normal Form](https://en.wikipedia.org/wiki/Third_normal_form) or higher

*Why?* Reduce duplication of data and update/deletion anomalies

###### [Style [S040](#style-s040)]

- Enforce referential integrity (on update/insert/delete) between all related tables

*Why?* Requires database to be in a consistent state at all times.  

*Why?* Once data inconsistency errors are introduced they are unlikly to be fixed without a great amount of effort.  

*Why?* lack of RF will cause side effects throughout the system, and very likely long after the change was made...

### Auditing

###### [Style [S041](#style-s041)]

- Establish audit tables for each table
    - Table should be a structure copy of the source table
    - Each update/insert/delete should store an entire copy of the current record values
    - Audit tables should be populate via triggers
    - Format:  AUD.<source schema>_<Source Table Name>
*Why?* Important to have a history of table changes for administration purposes

*Why?* Provides protection against user error, administrator error

*Why?*  Backup devices are too cumbersome for targeted restore/debugging - much simpler to perform a select against the audit table

### Patterns

###### [Style [S042](#style-s042)]

- Establish lookup tables for all dropdown / type information

*Why?* Want a reliable source of information for populating list elements for user selection


###### [Style [S043](#style-s043)]

- Keep Table Columns narrow, ideally under 10-20 columns

*Why?* Many columns is a code smell - usally a sign that proper normalization hasn't taken place

*Why?* Excessive columns by design usually leads to additional database migrations, as new fields are frequently added

###### [Style [S044](#style-s044)]

- Break complex single entities into several 1:1 tables

*Why?* Entities often have several discrete sections that are only related by the primary key. Keeping these separate keeps things tidy and the database simpler

*Why?* Makes the database less sparse - if no information for the given entity, do not need to create a placeholder record. (e.g. if no address for a customer, do need to add null values to the address table...)

###### [Style [S045](#style-s045)]

- Use key-value structures for highly detailed minutiae in the form of <entity table> - > <entity - type detail> -> <Type definition> 
    - For example, if we want to record 25 custom properties for an entity, add 25 detail records to the key/value detail table

*Why?* Many business situations call for storing a large number of domain-specific data points. 

*Why?* Add new data points is simply a matter of adding a record to the type table

*Why?* Encourages design of flexible user interfaces

###### [Style [S046](#style-s046)]

- use a common structure for phone, email, address data
    - All entities requring a phone number will share the same table

*Why?* DRY - Do Not Repeat yourself.  

*Why?* Consistent format for phone numbers, addresses in database

###### [Style [S047](#style-s047)]

- Set up a standard view for core system entities, e.g. Customer, Product, etc.
    - should include all relevant lookup table data

*Why?* saves time looking up basic information by avoiding writing joins


**[Back to top](#table-of-contents)**


## Security

### Security

###### [Style [S048](#style-s048)]

- Non-adminstrators should not have direct update/delete/insert permissions to database

*Why?* Prevents accidental / unintentional updates by novice users

*Why?* Prevents malicious/intentional damage to the database

###### [Style [S049](#style-s049)]

- Non-developers should not have select permission to OLTP database

*Why?* Users could cause blocking operations

*Why?* Users writing inefficient queries can slow down entire server

*Why?* Gives direct knowledge of table structure / internals to the user, making future migrations more difficult (user may need to update dozens of their personal queries reports)

###### [Style [S050](#style-s050)]

- users should be provided an OLAP / Datawarehouse for manual queries

*Why?* slow queries will not impact OLTP operations

*Why?* Provides a natural interface that can change slowly as to not break user queries


###### [Style [S051](#style-s051)]

- OLTP access should be handled thru stored procedures, running under user credentials

*Why?* Hides database details

*Why?* Allows logging of user details via credentials

*Why?* Stored procedures make database migrations simpler as the procedures themselves serve as an interface

###### [Style [S052](#style-s052)]

- Third-Party software should *never* access or push data directly to the primary OLTP system

*Why?* Must be able to change the OLTP database without breaking interface to external system

*Why?* Often difficult, expensive, or impossible to modify third-party ETL tasks.  Better to assume that the interface designed *never* changes, and this is impossible to achieve when direct OLTP access is given

###### [Style [S053](#style-s053)]

- Special databases should be established for application / situations that need direct access to OLTP data

*Why?* Many systems are unable to work with views, sprocs properly and will "require" direct table access

*Why?* Prevents third party app from slowing down the primary OLTP database

*Why?* Allows flexibility in deployment - can be offline, at a remote site, on local computer connected to machinary, etc. 

###### [Style [S054](#style-s054)]

- Employ a test / staging server for testing all administration work

*Why?* Verify expected operation on real data that isn't live

*Why?* Allow any developer to fix a data issue - the developer simply needs to verify the update operations and send a script to an administrator
