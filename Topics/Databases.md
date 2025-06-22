# Complete SQL Server Data Types and Properties Reference

## SQL Server Data Types

### Exact Numeric Types
- **bit** - 0, 1, or NULL (1 byte)
- **tinyint** - 0 to 255 (1 byte)
- **smallint** - -32,768 to 32,767 (2 bytes)
- **int** - -2,147,483,648 to 2,147,483,647 (4 bytes)
- **bigint** - -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 (8 bytes)
- **decimal(p,s)** / **numeric(p,s)** - Fixed precision and scale (5-17 bytes)
- **smallmoney** - -214,748.3648 to 214,748.3647 (4 bytes)
- **money** - -922,337,203,685,477.5808 to 922,337,203,685,477.5807 (8 bytes)

### Approximate Numeric Types
- **float(n)** - Floating point number, n = 1-53 (4 or 8 bytes)
- **real** - 4-byte floating point (-3.40E+38 to 3.40E+38)

### Date and Time Types
- **date** - Date only, 0001-01-01 to 9999-12-31 (3 bytes)
- **time(n)** - Time only with fractional seconds precision 0-7 (3-5 bytes)
- **datetime** - Date and time, 1753-01-01 to 9999-12-31 (8 bytes)
- **datetime2(n)** - Extended date and time with fractional seconds 0-7 (6-8 bytes)
- **smalldatetime** - Date and time, 1900-01-01 to 2079-06-06 (4 bytes)
- **datetimeoffset(n)** - Date, time, and time zone offset (8-10 bytes)

### Character String Types
- **char(n)** - Fixed-length non-Unicode, 1-8000 characters
- **varchar(n)** - Variable-length non-Unicode, 1-8000 characters
- **varchar(max)** - Variable-length non-Unicode, up to 2GB
- **text** - Variable-length non-Unicode (deprecated, use varchar(max))

### Unicode Character String Types
- **nchar(n)** - Fixed-length Unicode, 1-4000 characters
- **nvarchar(n)** - Variable-length Unicode, 1-4000 characters
- **nvarchar(max)** - Variable-length Unicode, up to 2GB
- **ntext** - Variable-length Unicode (deprecated, use nvarchar(max))

### Binary String Types
- **binary(n)** - Fixed-length binary data, 1-8000 bytes
- **varbinary(n)** - Variable-length binary data, 1-8000 bytes
- **varbinary(max)** - Variable-length binary data, up to 2GB
- **image** - Variable-length binary data (deprecated, use varbinary(max))

### Special Data Types
- **cursor** - Reference to a cursor
- **rowversion** / **timestamp** - Automatically generated binary numbers (8 bytes)
- **hierarchyid** - Position in a hierarchy
- **uniqueidentifier** - GUID (16 bytes)
- **sql_variant** - Stores values of various SQL Server data types
- **xml** - XML data
- **geometry** - Planar spatial data (CLR type)
- **geography** - Ellipsoidal spatial data (CLR type)
- **table** - Special data type for storing result sets
- **sysname** - System name data type (equivalent to nvarchar(128))

## Column Properties and Constraints

### Null Handling
- **NULL** - Allows null values (default behavior)
- **NOT NULL** - Does not allow null values

### Default Values
- **DEFAULT** - Specifies default value
  - Literal values: `DEFAULT 'Unknown'`, `DEFAULT 0`
  - System functions: `DEFAULT GETDATE()`, `DEFAULT NEWID()`, `DEFAULT USER_NAME()`
  - Expressions: `DEFAULT (YEAR(GETDATE()))`

### Identity Columns
- **IDENTITY(seed, increment)** - Auto-incrementing numeric column
  - Example: `IDENTITY(1,1)`, `IDENTITY(100,5)`
- **NOT FOR REPLICATION** - Identity not incremented during replication

### Key Constraints
- **PRIMARY KEY** - Unique identifier for rows
  - **CLUSTERED** / **NONCLUSTERED** - Storage organization
- **FOREIGN KEY** - References another table
  - **REFERENCES table_name(column_name)**
  - **ON DELETE CASCADE/SET NULL/SET DEFAULT/NO ACTION**
  - **ON UPDATE CASCADE/SET NULL/SET DEFAULT/NO ACTION**
  - **NOT FOR REPLICATION**
- **UNIQUE** - Ensures uniqueness across column(s)
  - **CLUSTERED** / **NONCLUSTERED**

### Check Constraints
- **CHECK** - Validates data against conditions
  - Example: `CHECK (Age >= 18 AND Age <= 120)`
  - **NOT FOR REPLICATION** - Skip during replication

### Computed Columns
- **AS expression** - Column value computed from other columns
- **PERSISTED** - Stores computed values physically
- **NOT NULL** - Can be applied to computed columns

### Column Storage Options
- **SPARSE** - Optimized storage for columns with many null values
- **FILESTREAM** - Stores large binary data in file system
- **ROWGUIDCOL** - Indicates column contains row GUID values

### Collation
- **COLLATE collation_name** - Specifies sorting and comparison rules
  - Example: `COLLATE SQL_Latin1_General_CP1_CI_AS`
  - Case sensitivity: CS (case sensitive) / CI (case insensitive)
  - Accent sensitivity: AS (accent sensitive) / AI (accent insensitive)

### Encryption (Always Encrypted)
- **ENCRYPTED WITH** - Column-level encryption
  - **COLUMN_ENCRYPTION_KEY**
  - **ENCRYPTION_TYPE = {DETERMINISTIC | RANDOMIZED}**
  - **ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'**

## Index Properties and Options

### Fill Factor and Padding
- **FILLFACTOR = n** - Percentage of space on index pages (1-100)
- **PAD_INDEX = {ON | OFF}** - Applies fill factor to intermediate pages

### Index Options
- **IGNORE_DUP_KEY = {ON | OFF}** - Ignores duplicate key errors
- **STATISTICS_NORECOMPUTE = {ON | OFF}** - Automatic statistics updates
- **ALLOW_ROW_LOCKS = {ON | OFF}** - Row-level locking
- **ALLOW_PAGE_LOCKS = {ON | OFF}** - Page-level locking
- **SORT_IN_TEMPDB = {ON | OFF}** - Sort operations in tempdb
- **ONLINE = {ON | OFF}** - Online index operations
- **MAXDOP = n** - Maximum degree of parallelism
- **DROP_EXISTING = {ON | OFF}** - Drop and recreate existing index
- **RESUMABLE = {ON | OFF}** - Resumable index operations (SQL Server 2017+)
- **MAX_DURATION = n** - Maximum duration for resumable operations

### Compression Options
- **DATA_COMPRESSION = {NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE}**
- **XML_COMPRESSION = {ON | OFF}** - XML data compression (SQL Server 2022+)

## Table-Level Properties

### Storage Options
- **ON filegroup_name** - Primary storage filegroup
- **TEXTIMAGE_ON filegroup_name** - Large object storage filegroup

### Partitioning
- **ON partition_scheme_name (column_name)** - Partition scheme
- **PARTITION FUNCTION** - Defines partitioning logic

### Table Options
- **ANSI_NULLS = {ON | OFF}** - Null comparison behavior
- **QUOTED_IDENTIFIER = {ON | OFF}** - Identifier quoting rules
- **LOCK_ESCALATION = {TABLE | AUTO | DISABLE}** - Lock escalation behavior
- **ALLOW_SNAPSHOT_ISOLATION = {ON | OFF}** - Snapshot isolation support
- **READ_COMMITTED_SNAPSHOT = {ON | OFF}** - Read committed snapshot isolation

### Change Tracking and Auditing
- **CHANGE_TRACKING = {ON | OFF}** - Change tracking
  - **CHANGE_RETENTION = n {DAYS | HOURS | MINUTES}**
  - **AUTO_CLEANUP = {ON | OFF}**
- **CHANGE_CAPTURE = {ON | OFF}** - Change data capture

### Temporal Tables (System-Versioned Tables)
- **SYSTEM_VERSIONING = {ON | OFF}** - Temporal table functionality
- **HISTORY_TABLE = schema_name.table_name** - Associated history table
- **PERIOD FOR SYSTEM_TIME (start_column, end_column)** - System time period
- **GENERATED ALWAYS AS ROW START/END** - System time columns
- **HIDDEN** - Hide system time columns from SELECT *

### Memory-Optimized Tables (In-Memory OLTP)
- **MEMORY_OPTIMIZED = ON** - In-memory table
- **DURABILITY = {SCHEMA_ONLY | SCHEMA_AND_DATA}** - Durability level
- **BUCKET_COUNT = n** - Hash index bucket count

### External Tables (PolyBase)
- **EXTERNAL** - External table definition
- **DATA_SOURCE = external_data_source_name**
- **FILE_FORMAT = external_file_format_name**
- **LOCATION = 'folder_or_filepath'**

### Graph Tables (SQL Server 2017+)
- **AS NODE** - Graph node table
- **AS EDGE** - Graph edge table

### Ledger Tables (SQL Server 2022+)
- **LEDGER = ON** - Ledger table with cryptographic proof
- **APPEND_ONLY = ON** - Append-only ledger table
- **SYSTEM_VERSIONING = ON** - System-versioned ledger table

## Advanced Properties

### Replication Options
- **NOT FOR REPLICATION** - Skip during replication (constraints, identity, triggers)

### Service Broker
- **ENABLE_BROKER** - Enable Service Broker on database
- **DISABLE_BROKER** - Disable Service Broker

### Full-Text Search
- **FULLTEXT** - Full-text index properties
- **KEY INDEX** - Unique key index for full-text
- **FILEGROUP** - Full-text filegroup

### Columnstore Indexes
- **COLUMNSTORE** - Columnstore index
- **CLUSTERED COLUMNSTORE INDEX** - Clustered columnstore
- **NONCLUSTERED COLUMNSTORE INDEX** - Non-clustered columnstore
- **COMPRESSION_DELAY = n** - Compression delay in minutes

## Example: Complete Table Definition

```sql
CREATE TABLE [dbo].[CompleteExample] (
    -- Identity primary key with custom increment
    [ID] int IDENTITY(1000,1) NOT NULL,
    
    -- Unicode string with collation and default
    [Name] nvarchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL 
           DEFAULT 'Unknown',
    
    -- Email with unique constraint
    [Email] varchar(255) NULL,
    
    -- Check constraint with multiple conditions
    [Age] tinyint NULL CHECK ([Age] >= 0 AND [Age] <= 150),
    
    -- Default with system function
    [CreatedDate] datetime2(3) NOT NULL DEFAULT GETDATE(),
    [CreatedBy] nvarchar(128) NOT NULL DEFAULT SUSER_SNAME(),
    
    -- Money with default
    [Salary] money NULL DEFAULT 0.00,
    
    -- Sparse column for rarely populated data
    [Notes] nvarchar(max) SPARSE NULL,
    
    -- Computed column (persisted)
    [FullName] AS ([FirstName] + ' ' + [LastName]) PERSISTED,
    
    -- GUID with default
    [RowGUID] uniqueidentifier ROWGUIDCOL NOT NULL DEFAULT NEWID(),
    
    -- Automatic row version
    [RowVersion] rowversion NOT NULL,
    
    -- System time columns for temporal table
    [SysStartTime] datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    [SysEndTime] datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    
    -- Large binary data with FILESTREAM
    [Document] varbinary(max) FILESTREAM NULL,
    
    -- Spatial data
    [Location] geography NULL,
    
    -- XML data
    [Settings] xml NULL,
    
    -- Period definition for system versioning
    PERIOD FOR SYSTEM_TIME ([SysStartTime], [SysEndTime]),
    
    -- Primary key with custom fill factor
    CONSTRAINT [PK_CompleteExample] PRIMARY KEY CLUSTERED ([ID]) 
    WITH (FILLFACTOR = 90, PAD_INDEX = ON),
    
    -- Unique constraint on email
    CONSTRAINT [UK_CompleteExample_Email] UNIQUE NONCLUSTERED ([Email])
    WITH (FILLFACTOR = 95),
    
    -- Foreign key with cascading options
    CONSTRAINT [FK_CompleteExample_Department] 
    FOREIGN KEY ([DepartmentID]) REFERENCES [Department]([ID])
    ON DELETE SET NULL ON UPDATE CASCADE
    
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY] FILESTREAM_ON [FileStreamGroup]
WITH (
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = [dbo].[CompleteExample_History]),
    DATA_COMPRESSION = ROW
);

-- Non-clustered index with multiple options
CREATE NONCLUSTERED INDEX [IX_CompleteExample_Name_Age] 
ON [dbo].[CompleteExample] ([Name], [Age])
INCLUDE ([Email], [Salary])
WITH (
    FILLFACTOR = 80,
    PAD_INDEX = ON,
    STATISTICS_NORECOMPUTE = OFF,
    SORT_IN_TEMPDB = ON,
    ALLOW_ROW_LOCKS = ON,
    ALLOW_PAGE_LOCKS = ON,
    DATA_COMPRESSION = PAGE,
    ONLINE = ON,
    MAXDOP = 4
);
```

### **1. Table Elements**

* **Columns**: Represent the fields in a table.
* **Rows**: Contain the actual data entries (records).
* **Primary Key**: A unique identifier for each row.
* **Foreign Key**: References a primary key in another table.
* **Constraints**:

  * **NOT NULL**: Ensures the column cannot have a NULL value.
  * **UNIQUE**: Ensures all values in the column are distinct.
  * **CHECK**: Ensures values satisfy a specific condition.
  * **DEFAULT**: Assigns a default value to a column.
  * **PRIMARY KEY**: Combines NOT NULL and UNIQUE for a column or set of columns.
  * **FOREIGN KEY**: Maintains referential integrity between tables.
* **Indexes**: Speed up searches and queries.

  * **Clustered Index**: Reorders the physical data.
  * **Non-clustered Index**: Does not affect the data order.
  * **Full-text Index**: For text-search purposes.
  * **Fill Factor**: Determines the percentage of space allocated for data and future growth in an index page.
  * **Filtered Index**: Indexes only rows that meet a specific condition.

---

### **2. Data Types**

* **Numeric**:

  * `INT`, `BIGINT`, `SMALLINT`, `TINYINT`
  * `DECIMAL`, `NUMERIC`, `FLOAT`, `REAL`
* **Character**:

  * `CHAR`, `VARCHAR`
  * `TEXT`, `NCHAR`, `NVARCHAR`, `NTEXT`
* **Date and Time**:

  * `DATE`, `DATETIME`, `TIMESTAMP`
  * `TIME`, `YEAR`
* **Binary**:

  * `BINARY`, `VARBINARY`
  * `IMAGE` (deprecated in some systems)
* **JSON and XML**:

  * `JSON` (supported in modern systems like MySQL, PostgreSQL)
  * `XML`
* **Specialized**:

  * `GEOMETRY`, `GEOGRAPHY` (spatial data types)
  * `ENUM`, `SET` (MySQL-specific)

---

### **3. Table Relationships**

* **One-to-One**
* **One-to-Many**
* **Many-to-Many**
* **Self-Join**: A table referencing itself.
* **Cascading Actions**:

  * ON DELETE CASCADE
  * ON UPDATE CASCADE

---

### **4. Storage and Optimization Features**

* **Partitions**: Divide tables for improved performance.
* **Views**: Virtual tables based on queries.
* **Temporary Tables**: Session-specific tables.
* **Materialized Views**: Precomputed and stored query results.
* **CTEs (Common Table Expressions)**: Temporary result sets within queries.
* **Triggers**: Actions performed automatically on INSERT, UPDATE, or DELETE.
* **Stored Procedures**: Predefined sets of SQL commands.
* **Functions**: Return values based on SQL operations.
* **Tablespaces**: Physical storage locations for tables and indexes.

---

### **5. Table-Level Operations**

* **CRUD Operations**:

  * Create, Read, Update, Delete data.
* **Joins**:

  * INNER, OUTER (LEFT/RIGHT), CROSS, FULL OUTER.
* **Union/Intersect/Except**:

  * Combine or filter results from multiple queries.
* **Transactions**:

  * BEGIN TRANSACTION, COMMIT, ROLLBACK.
* **Fill Factor**: Optimizes index space usage by reserving space for updates.

---

### **6. Metadata and Security**

* **Schemas**: Logical grouping of tables and objects.
* **Privileges**:

  * GRANT, REVOKE (permissions for users).
* **Audit Columns**: `Created_at`, `Updated_at`, `Created_by`, etc.
* **Generated Columns**: Automatically derived values.

  * Virtual/Computed columns.
* **Row-Level Security**: Access control on individual rows.

---

### **7. Advanced Features**

* **Sharding**: Distribution of table data across multiple servers.
* **Replication**: Copying data across servers.
* **Foreign Data Wrappers**: Accessing external data sources.
* **Sequence/Auto-Increment**: Automatic value generation for primary keys.
* **Partition Pruning**: Optimize queries by skipping irrelevant partitions.

---

### **8. Miscellaneous**

* **Collation**: Defines how strings are compared.
* **Charset**: Specifies the character encoding.
* **Inheritance**: Supported in some databases like PostgreSQL.
* **Database Link**: Accessing data from remote databases (Oracle, PostgreSQL).
* **Compression**: Storing data in a compressed format.

---
