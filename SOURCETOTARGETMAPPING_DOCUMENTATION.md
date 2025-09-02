# Documentation for `SOURCETOTARGETMAPPING.ipynb`

This notebook is designed to trace column-level data lineage in Snowflake databases. It contains Python code that interacts with Snowflake to fetch metadata, lineage information, and manage lineage mapping tables. Below is an overview of the key components and functions within the notebook:

---

## 1. **Import Packages**
Imports essential Python libraries for Snowflake connectivity, data manipulation, and argument parsing.

```python
import snowflake.connector
from snowflake.connector import DictCursor
import pandas as pd
from snowflake.connector.pandas_tools import write_pandas
import sys
```

---

## 2. **Parameters**
Sets up user, database, schema, table, and warehouse parameters. These are typically provided via command-line arguments; defaults are set if arguments are missing.

```python
user = sys.argv[0].split(',')[0] if sys.argv[0].count(',')>=2 else 'BASUK'
database = sys.argv[0].split(',')[2] if sys.argv[0].count(',')>=2 else 'DEV_P_CMSC_CMO_DB'
schema = sys.argv[0].split(',')[3] if sys.argv[0].count(',')>=2 else 'PUBLIC'
table = sys.argv[0].split(',')[4] if sys.argv[0].count(',')>=2 else 'SOURCE_TO_TARGET_MAPPING'
warehouse = sys.argv[0].split(',')[5] if sys.argv[0].count(',')>=2 else 'MY_WH'
```

---

## 3. **Lineage Columns Definition**
Defines the columns used for lineage mapping.

```python
LINEAGE_COLUMNS = [
    'ROOT_DATABASE',
    'ROOT_SCHEMA',
    'ROOT_TABLE',
    'ROOT_COLUMN',
    'DISTANCE',
    'SOURCE_OBJECT_DOMAIN',
    'SOURCE_OBJECT_DATABASE',
    'SOURCE_OBJECT_SCHEMA',
    'SOURCE_OBJECT_NAME',
    'SOURCE_OBJECT_TYPE',
    'SOURCE_OBJECT_COLUMN',
    'SOURCE_OBJECT_ROLE',
    'SOURCE_OBJECT_OWNER',
    'SOURCE_OBJECT_GRANTEE',
    'SOURCE_OBJECT_GRANT_TYPE',
]
```

---

## 4. **Get Snowflake Cursor**
Returns a dictionary cursor for executing queries and fetching results as dictionaries.

```python
def get_cursor(conn):
    return conn.cursor(DictCursor)
```

---

## 5. **Fetch Column Metadata**
Fetches metadata for columns in specified schemas within a database.

```python
def get_columns(conn, database, schemas) -> list:
    # Executes a SQL query to retrieve table and column information
```

---

## 6. **Generate Lineage SQL**
Constructs a SQL query to retrieve column-level lineage for a given column.

```python
def get_column_lineage_sql(column, with_order_by=True):
    # Builds the query based on column information
```

---

## 7. **Column Metadata Caching**
Caches metadata queries to avoid redundant database calls.

```python
column_metadata_cache = {}

def get_column_metadata(conn, database, schema, table, column, type):
    # Checks cache, fetches metadata if not present
```

---

## 8. **Fetch Column Lineage**
Executes the lineage SQL and fetches results.

```python
def get_column_lineage(conn, column):
    # Executes the generated lineage SQL and returns the results
```

---

## 9. **Fetch Lineage Results from Async Query**
Retrieves results from an asynchronously executed Snowflake query using its SFQID.

```python
def get_column_lineage_results(conn, sfqid):
    # Uses get_results_from_sfqid to fetch results
```

---

## 10. **Check Query Completion**
Checks whether an async query has completed.

```python
def check_if_query_complete(conn, sfqid):
    # Returns True if the query has finished running
```

---

## 11. **Execute Lineage Query Asynchronously**
Executes lineage queries in async mode and returns the query ID.

```python
def get_column_lineage_async(conn, column):
    # Executes the lineage SQL asynchronously and returns the SFQID
```

---

## 12. **Drop Existing Mapping Table**
Drops an existing mapping table in Snowflake to allow recreation.

```python
def drop_table(user, database, schema, table, warehouse):
    # Connects to Snowflake and executes a DROP TABLE statement
```

---

## 13. **Chunk Utility**
Splits a list into smaller chunks for batch processing.

```python
def chunks(lst, n):
    """Yield successive n-sized chunks from lst."""
    for i in range(0, len(lst), n):
        yield lst[i:i + n]
```

---

## 14. **Main Routine**
The entry point for running the notebook as a script. Handles argument parsing, orchestrates lineage fetching and table management.

```python
if __name__ == "__main__":
    # Parses arguments, fetches lineage, manages mapping tables
```

---

## **Usage Overview**
- **Connect to Snowflake:** Establish connection using credentials and parameters.
- **Fetch Metadata:** Retrieve schemas, tables, and columns for lineage analysis.
- **Fetch Lineage:** For each column, generate and execute lineage queries (sync or async).
- **Manage Mapping Table:** Drop and recreate the mapping table as needed.
- **Batch Processing:** Use chunk utility to process large numbers of columns efficiently.

---

## **Dependencies**
- `snowflake-connector-python`
- `pandas`
- `argparse` (commented out, could be activated for improved argument parsing)

---

## **Notes**
- The notebook is designed for modular execution: each code cell defines a distinct part of the lineage extraction process.
- Error handling and logging are minimal; production usage may require enhancements.
- Asynchronous query execution improves performance for large lineage extraction jobs.

---

**Author:** BASUK  
**Email:** basuk@vrtx.com
