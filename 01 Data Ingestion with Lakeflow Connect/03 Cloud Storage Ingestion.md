### Data Ingestion from Cloud Storage

The goal is to convert raw files (such as csv, JSON, and parquet) into Delta tables, unlocking advanced functionality such as ACID transacts, time travel, and scheme enforcement.

The 3 primary methods for ingesting files from cloud object storage into Delta tables:
- CREATE TABLE AS (CTAS)
- COPY INTO
- Auto Loader

Ingestion from cloud object is performed using Lakeflow Connect Standard Connectors.

#### CREATE TABLE AS (CTAS)

```sql
CREATE TABLE new_table AS
SELECT *
FROM read_files(
    <path_to_file>,
    format => '<file_type>',
    <other_format_specific_options>
);
```

* **CREATE TABLE AS** creates a Delta table **by default** from files in cloud object storage
* The **read_files** function reads files under a provided location & returns the data in tabular form:
- can detect the file format automatically & infer a unified schema
- can be used in **streaming tables** to **incrementally** ingest files into Delta Lake using Auto Loader

#### COPY INTO

Incremental Batch - COPY INTO (legacy)

```sql
CREATE TABLE new_table;

COPY INTO new_table
FROM '<dir_path>'
FILEFORMAT=<file_type>
FORMAT_OPTIONS(<options>)
COPY_OPTIONS(<options>)
```

Performs a bulk load from files in cloud object storage into the table, and in this example (the code above), it will load files into the empty new_table

* COPY INTO:
- is a **retriable and idempotent operation** & will skip files that have already been loaded **(incremental**)
- supports various common files (parquet, csv, json, xml, etc)
- **FROM** specifies the path of the cloud storage
- **FORMAT_OPTIONS()** control how the source files are parsed & interpreted
- **COPY_OPTIONS()** controls the behaviour of the COPY INTO operation itself, such as schema evolution

#### AUTO LOADER

Method 3: Incremental Batch or Streaming

* **Incrementally** and **efficiently** processes new data files as they arrive in cloud storage **without any additional setup**
* has support for both **Python** and **SQL (leveraging Declarative Pipelines)**
* can be used to process billions of files
* is built upon **Spark Structured Streaming**

#### Appending Metadata Columns on Ingest

* Metadata columns like source file name and modification time can be appended using the _metadata column - capturing essential context for each row during table creation in the Lakehouse.
* This is a hidden column that is available for all input file formats.
* To include the **_metadata** column in the returned DataFrame, you must explicitly select it in the rad query when specifying the source.
* The two common fields include:
    - **_metadata.file_modification_time** which provides teh last mod timestamp
    - **_metadata.file_name** which returns the name of the source file for each row

#### Working with the Rescued Data Column

The rescued data column (_rescued_data) captures mismatched or unparseable fileds as JSON - preserving non-conforming input values in the Lakehouse tables instead of dropping them

* **read_files(), spark.read** or **Auto Loader** provides a **rescued data column** if the raw data does not match the schema

#### Ingesting Semi-Structured Data: JSON

* JSON **objects** are enclosed in brackets
* Within the curly brackets, JSON objects contain key-value pairs
* Each key is always a string enclosed in quotation marks. Each key contains a value.
* Value can be String, Number, Boolean, Array, Object

1. **Working with JSON-Formatted Column as a STRING

1.1. **STRING** Data Type

* JSON can be stored as a simple STRING
* Can hold **any JSON STRING** without constraints
* It is less **perfomant**

```sql
SELECT json_column:name
SELECT json_column:address:city
```

1.2. **STRUCT** Data Type

* You can parse JSN data into a STRUCT type, with a **defined schema**
* STRUCT **enforces** the JSON schema
* This is **more efficient** for querying 
* When converting a JSON-formatted STRING column to a STRUCT column, this is how JSON types map to Databricks SQL data types:
    - JSON String maps to STRING
    - JSON Number maps to INT, FLOAT, or DOUBLE
    - JSON Boolean maps to BOOLEAN
    - JSON Object maps to STRUCT<>
    - JSON Array maps to ARRAY<>

How to easily determine the structure of the JSON string. This can be done in 2 steps:
* First get the schema of the JSON-formatted string
    - you can use the built-in _*schema_of_json*_ function to automatically derive the schema. Simply pass the example JSON-formatted string as teh argument to this function, and it will return the inferred schema

    ```sql
    SELECT schema_of_json('sample-json-string')
    ```
* Once you have the structure of the JSON-formatted string, you can use the Spark _*from_json*_ function
    - This function takes the JSON string and the specified schema obtained in the previous step, and returns a STRUCT column

    ```sql
    SELECT from_json(json_col, 'json-struct-schema') AS struct_column
    FROM table
    ```

1.3. VARIANT Data Type

* Can store any type of data, including JSON, and **is ideal for semi-structured** data
* Highly **flexible**
* Improved **performance** over existing methods

