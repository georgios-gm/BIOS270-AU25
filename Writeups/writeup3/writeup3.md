# Data

## Setup

Completed the setup steps to authenticate Google Clound, the rclone config, and BigQuery Dataset. 

## Database

### 1. Create a Local SQL Database

<img width="689" height="30" alt="Screenshot 2025-12-11 at 12 45 46 AM" src="https://github.com/user-attachments/assets/6429744d-19bc-4a70-b16a-2ae4800490f8" />

- Examine `create_bacteria_db.sh`, how many tables will be created in the database? Three tables are created: gff, metadata, and protein_cluster. 
- In the `insert_gff_table.py` script you submitted, explain the logic of using `try` and `except`. Why is this necessary? This is necessary since we want to avoid multiple scripts editing the table at once. If the table is locked, we go to `except` and wait for a moment before trying again.

<img width="540" height="61" alt="Screenshot 2025-12-11 at 1 06 03 AM" src="https://github.com/user-attachments/assets/2c39156d-bebd-43da-9df4-afded7d8ad3b" />

### 2. Query the Created Database

Here are the completed TODO sections: 

<img width="601" height="321" alt="Screenshot 2025-12-11 at 4 11 25 PM 1" src="https://github.com/user-attachments/assets/1b652860-4f97-45e5-b948-6e8061167cc8" />


Running `query_bacteria_db.py` takes singificant time, with about 7 seconds per 10 record ids, totaling to ~48 minutes. 

<img width="542" height="432" alt="Screenshot 2025-12-11 at 4 22 31 PM" src="https://github.com/user-attachments/assets/a3780198-885b-47d1-a131-71eb4d170bd6" />

When uncommeting `db.index_record_ids()` in `query_bacteria_db.py`, the overall runtime decreases to 8.7 seconds. 

<img width="465" height="480" alt="Screenshot 2025-12-11 at 4 25 31 PM" src="https://github.com/user-attachments/assets/1e53da06-2631-479b-82ed-439c7e77b078" />

This represents a very singificant decrease in runtime, and it is because the indexing allows for the WHERE record_id step to be completed significantly faster, rather than needed to look through all the rows. 

### 3. Upload to Google BigQuery

The dataset you are handling is relatively small. However, for larger datasets or collaborative access, uploading to **Google BigQuery** is a practical approach.

Examine the `upload_bigquery.py` script.  
Explain the role of `CHUNK_SIZE` and why it is necessary:

```python
df = pd.read_sql_query(
    f"SELECT * FROM {table} LIMIT {CHUNK_SIZE} OFFSET {offset}",
    conn
)
```
Run the `upload_bigquery.py` script (using `bioinformatics_latest.sif` container)

```bash
python upload_bigquery.py --local_database_path <path to the bacteria.db created in Section 1> --project_id <GCP project-id> --dataset_id bacteria
```

Once your dataset has been uploaded, create a query on BigQuery that involves at least **two tables** from the dataset.  
Export the query results as a **CSV file** to **GCS**.


### 4. HDF5 Data

Review the `create_protein_h5.sh` and `create_protein_h5.py` scripts.  
Make sure you understand their functionality. You won't need to run these scripts as they take a few hours to complete.

Explain why the following chunk configuration makes sense - what kind of data access pattern is expected, and why does this align with biological use cases?

```python
chunk_size = 1000
chunks = (chunk_size, n_features)
```




