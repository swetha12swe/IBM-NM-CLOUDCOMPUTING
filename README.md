# IBM-NM-cloudcomputing
from google.cloud import bigquery

# Replace the following placeholder with your actual GCP project ID
PROJECT_ID = 'your_project_id'

# Create a BigQuery client
client = bigquery.Client(project=PROJECT_ID)

# Example: Create a dataset
dataset_id = 'your_dataset_name'
dataset_ref = client.dataset(dataset_id)
dataset = bigquery.Dataset(dataset_ref)
client.create_dataset(dataset)

# Example: Create a table schema
schema = [
    bigquery.SchemaField('id', 'INTEGER', mode='NULLABLE'),
    bigquery.SchemaField('column1', 'STRING', mode='NULLABLE'),
    bigquery.SchemaField('column2', 'STRING', mode='NULLABLE'),
]

# Example: Create a table
table_id = 'your_table_name'
table_ref = dataset_ref.table(table_id)
table = bigquery.Table(table_ref, schema=schema)
client.create_table(table)

# Example: Insert data into the table
rows_to_insert = [
    (1, 'value1', 'valueA'),
    (2, 'value2', 'valueB'),
    (3, 'value3', 'valueC'),
]

errors = client.insert_rows(table, rows_to_insert)
if errors:
    print(f"Error inserting rows: {errors}")

# Example: Query data
query = f"SELECT * FROM `{PROJECT_ID}.{dataset_id}.{table_id}`"
query_job = client.query(query)
rows = query_job.result()

for row in rows:
    print(row)

