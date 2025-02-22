Apache Iceberg is a high-performance table format for large-scale data sets, and it provides features like versioning, schema evolution, and partitioning. Here's a step-by-step explanation of how Iceberg represents the canonical list of data and delete files, along with an example.

### Step 1: Understanding the Table Structure

Iceberg tables consist of several components:
- **Data Files**: These are the actual files that contain the data, typically stored in formats like Parquet or ORC.
- **Manifest Files**: These files keep track of data files and have metadata about them.
- **Snapshot**: A snapshot is a point-in-time representation of the table. It includes references to the manifest files.
- **Delete Files**: These files record deletions, allowing for modification of the data while preserving the ability to query historical versions.

### Step 2: Inserting Data

When you insert data into an Iceberg table, it creates data files that contain the new records. For example:

- **Initial Data Insertion**:
    - You insert the following records:
      ```
      ID | Name
      1  | Alice
      2  | Bob
      ```
    - Iceberg writes these records to a data file (e.g., `data-file-1.parquet`).

### Step 3: Creating a Snapshot

When a write operation (like an insert) happens, Iceberg takes a snapshot of the table's current state:
- The snapshot contains references to the manifest files of the data files created. For example, it might generate Snapshot ID `snapshot-1`, referencing `data-file-1.parquet`.

### Step 4: Deleting Data

Suppose you want to delete a record:
- You decide to delete the record where `ID = 2` (Bob).
- Instead of altering the original data file, Iceberg creates a delete file (e.g., `delete-file-1.parquet`) that contains the information about the deletion, which could look like:
    ```
    ID
    2
    ```

### Step 5: Creating a New Snapshot

When the delete operation occurs, Iceberg creates another snapshot. 
- For instance, after the deletion, you would have a new snapshot, `snapshot-2`, that includes not only the reference to `data-file-1.parquet` but also to `delete-file-1.parquet`.

### Step 6: Querying the Data

When you perform queries, Iceberg considers both data files and delete files to provide the correct view of the data:
- If you query for all records now, Iceberg will look at `snapshot-2`:
    - It finds `data-file-1.parquet`, but because `delete-file-1.parquet` indicates that `ID = 2` must be ignored, it will return only:
      ```
      ID | Name
      1  | Alice
      ```

### Conclusion

Apache Iceberg maintains a canonical list of data by uniquely managing data and delete files through snapshots. This strategy allows it to efficiently track changes to the data while preserving historical versions for time travel. The architecture ensures that querying the most current state, while also allowing backtracking to earlier states, is both performant and manageable.
