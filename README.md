# t-sql-styleguide
Style Guide for SQL Server Development for VPIDEV databases

## Table of Contents

1. [Naming](#naming)
1. [Standard Field Sizes](#standard-field-sizes)
1. [Comments](#comments)


## Naming

### Naming Guidelines
###### [Style [Y001](#style-y001)]

- Do not abbreviate names - use full english word

*Why?*: Abbreviations are ambigious, hard to read, and difficult for new developers to understand

###### [Style [Y001](#style-y001)]

- Capitalize the first letter of each word, and separate words by underscores

*Why?*: SQL code is not case sensitive, and underscores ensure words are readable

### Table Names
###### [Style [Y002](#style-y002)]

- All tables should include the schema, dbo is not to be used
 in the table name

*Why?*: dbo schema tends to collect junk objects. Never using the dbo schema makes these stand out so they can be removed

*Why?*: Schema names keep logic areas organized

###### [Style [Y002](#style-y002)]

- Table names should be singlular, e.g. Production_Batch instead of Production_Batches

*Why?*: Consistency - Querying a Purchase_Order, should not need to think about whether this table is singluar or plural

### Field Names
###### [Style [Y003](#style-y003)]

- Primary Key Fields should include _PK at the end of the field name

*Why?*: Easily identify the column that uniquely identifies a record

*Why?*: unambigous column names during joins

###### [Style [Y003](#style-y003)]

- Foreign Key Fields should match the primary key name, include _FK at the end of the field name in place of _PK

*Why?*: unambigous column names during joins

###### [Style [Y003](#style-y003)]

- Special Foreign keys may be referenced as _XK when 
    - multiple foreign key columns point to the same primary key, e.g. `Home_Number_XK`,  `Work_Number_XK`
    - the primary field name doesn't make sense semantically

*Why?*: Referencing as _FK may confused confuse developers when there is no matching _PK in the related table

###### [Style [Y003](#style-y003)]

- Bit fields should be prefixed with `Flag_`

*Why?*: easily identify boolean/bit fields when viewing / querying tables

###### [Style [Y003](#style-y003)]

- Datetime fields should be prefixed with `Date_`

*Why?*: Most date name naturally include "date" in the name, and this style ensure the date is always in the same location

### Sproc, Function Names
###### [Style [Y004](#style-y004)]

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

###### [Style [Y004](#style-y004)]

- Sprocs for auto-complete operations should use the format `<schema>.<table name>_<field_name>_Autocomplete

*Why?*: Autocomplete operations are specialized and must be fast

*Why?*: There may be a number of possible autocomplete operations, and this keeps things tidy


### View Names
###### [Style [Y005](#style-y005)]

- View names must begin with `View_`

*Why?*: Keep view names distinct from sprocs, table names

### Schema Names
###### [Style [Y006](#style-y006)]

### Constraint names
###### [Style [Y007](#style-y007)]

- Primary Keys must begin with `PK_`

###### [Style [Y007](#style-y007)]

- Foreign Keys must begin with `FK_`

###### [Style [Y007](#style-y007)]

- Unique Indexes must begin with `UX_`

###### [Style [Y007](#style-y007)]

- Non-Unique Indexes must begin with `IX_`

### Job names

###### [Style [Y007](#style-y007)]

- Job Name should aim to use existing table, sproc, or SSIS Package names

*Why?*: Make deployment, debugging, administration easier by being consisent and mimnimze the number of handles in the system.  

###### [Style [Y007](#style-y007)]

- Jobs running SSIS packages should follow the format `SSIS_<job name>`

*Why?*: Identify packages moving data in/out of server


###### [Style [Y007](#style-y007)]

- Jobs running SSRS subscriptions should follow the format `SSRS_<Subscription_Name`

*Why?*: Group reporting operations together

###### [Style [Y007](#style-y007)]

- Jobs running Sprocs should follow the format `Data_Processing_<sproc name>`

*Why?*: Easily identify the sproc without having to open the job

###### [Style [Y007](#style-y007)]

- Jobs running Maintenance plans should follow the format `Maintenance_Plan<maintenance plan name>`

*Why?*: Maintenance Plans use SQL Agent, and need to isolate these

###### [Style [Y007](#style-y007)]

- jobs executing system commands or sprocs for server operations should follow the format `Utility_<descriptor`

*Why?*: group misc system functions together

### SSIS Package names

###### [Style [Y007](#style-y007)]

- Project/solution names should indicate the external source of data e.g. `SalesForce` or `Epicor`

*Why?*: Organize a potentially large number of external sources into separate areas

###### [Style [Y007](#style-y007)]

- All SSIS Packages connecting to an external source should be under the same project/solution

*Why?*: Easily locate all packages connected with a given external source

###### [Style [Y007](#style-y007)]

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

###### [Style [Y007](#style-y007)]

- Use `numeric(12,5)` for all decimal values

*Why?* 5 Decimal places is almost alway more than enough precision

*Why?* Reduce unnecessary migrations to widen fields from 1 to 2 decimal places


### String

###### [Style [Y007](#style-y007)]

- Use `varchar(25)` for strings 1-25 charcters

*Why?* Reduce unnecessary migrations to widen fields


###### [Style [Y007](#style-y007)]

- use `varchar(100)` for medium length fields

*Why?* Reduce unnecessary migrations to widen fields

###### [Style [Y007](#style-y007)]

- use `varchar(max)` for multi-line text input / storage

*Why?* Trying to guess 100,200,300 fields is limiting, and can be a source of unexpected truncation / errors

### Boolean

###### [Style [Y007](#style-y007)]

- use `bit` for boolean values

### Date

###### [Style [Y007](#style-y007)]

- use `datetime2` for all date, time values

*Why?* Greatest amount of flexibility for date storage and manipulation


**[Back to top](#table-of-contents)**


## Comments

### Sprocs

###### [Style [Y007](#style-y007)]

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

###### [Style [Y007](#style-y007)]

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

*Why?*  Quickly diagnose issues - can see quickly if problem is likely due to a recent change




**[Back to top](#table-of-contents)**