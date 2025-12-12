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

The presence of `CHUNK_SIZE` is necessary for large tables that require a huge amount of memory. By using chunking, there is a limit to the amount of memory used so that the read operation can scale to very large tables that considered all at once would crash the job. 

Here is the completion of the data uploading: 

<img width="779" height="282" alt="Screenshot 2025-12-11 at 4 48 15 PM" src="https://github.com/user-attachments/assets/72735e2d-5384-4523-8f8f-7485d7a2450b" />

Here are the results of a toy BigQuery with the output exported as a csv file to my bucket storage: 

<img width="1117" height="1055" alt="Screenshot 2025-12-11 at 4 58 23 PM" src="https://github.com/user-attachments/assets/2b4ca695-ed35-4a41-8055-568683d31d8d" />
<img width="1082" height="225" alt="Screenshot 2025-12-11 at 4 59 35 PM" src="https://github.com/user-attachments/assets/622a22e6-0030-48aa-ac92-5ae21e2f5d87" />


### 4. HDF5 Data

The below chunking size makes sense because we are likely to want all of the features for a given protein at once, but we would rarely want the value for a specific feature of all the proteins at once. Biologically, this makes sense since we are interested in making biological conclusions about our proteins and reading them in batches with all the features. 

```python
chunk_size = 1000
chunks = (chunk_size, n_features)
```







