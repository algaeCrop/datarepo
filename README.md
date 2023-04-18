# Overview

The AlgaeCrop SQLite database contains information on microalgae and their metabolites, such as lipid content, protein content, fatty acids, amino acids, vitamins, and more. Data has been normalized to display common unit, and data point have been extrapulated from existing data when necessary.


The database consists of two types of tables:

Sources Table: Contains information on the sources of data used in the AlgaeCrop project, such as the reference ID, DOI, Harvard reference, title, link, input data, version, and keywords.

Data Tables: Contains information from various sources on microalgae and their metabolites. Each data table is named by an ID indicating the type of data it contains, such as lipid for lipid content data. Each data table has a column indicating the source it is from (source_id) and the sample number within that source (run_id). Depending on the type of data, the table may contain additional columns for information on the metric unit, method of acquiring data, and data points.

Accessing the Database

The AlgaeCrop SQLite database is available on GitHub as a .sqlite file. To access the database, you will need to download the file and open it using an SQLite client. Here's an example of how to open the database using the SQLite command-line client:

Example Code (load data in to a pandas table in python):

```python
import sqlite3
import pandas as pd

# Establish a connection to the database
conn = sqlite3.connect('your_database_name.db')

# Write your SQL query
query = '''
SELECT sources.*, lipid.*, protein.*
FROM sources
LEFT JOIN lipid ON sources.ref_id = lipid.ref_id AND sources.source_id = lipid.source_id AND sources.run_id = lipid.run_id
LEFT JOIN protein ON sources.ref_id = protein.ref_id AND sources.source_id = protein.source_id AND sources.run_id = protein.run_id
'''

# Execute the query and load the results into a DataFrame
df = pd.read_sql_query(query, conn)

# Close the connection
conn.close()

# View the DataFrame
print(df.head())
