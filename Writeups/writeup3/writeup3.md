# Data

## Setup

Completed the setup steps to authenticate Google Clound, the rclone config, and BigQuery Dataset. 

## Database

### 1. Create a Local SQL Database

<img width="689" height="30" alt="Screenshot 2025-12-11 at 12 45 46 AM" src="https://github.com/user-attachments/assets/6429744d-19bc-4a70-b16a-2ae4800490f8" />

- Examine `create_bacteria_db.sh`, how many tables will be created in the database? Three tables are created: gff, metadata, and protein_cluster. 
- In the `insert_gff_table.py` script you submitted, explain the logic of using `try` and `except`. Why is this necessary? This is necessary since we want to avoid multiple scripts editing the table at once. If the table is locked, we go to `except` and wait for a moment before trying again.

<img width="540" height="61" alt="Screenshot 2025-12-11 at 1 06 03 AM" src="https://github.com/user-attachments/assets/2c39156d-bebd-43da-9df4-afded7d8ad3b" />


