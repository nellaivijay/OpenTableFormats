# Understanding Open Table Formats: Step-by-Step Guide to Data Representation and File Deletion



Open table formats like Delta Lake, Apache Iceberg, Apache Hudi, and Paimon are pivotal in modern data architecture, each offering a unique approach to representing the canonical list of data and managing deletions. Understanding how each format operates provides insight into their capabilities and use cases. Let’s delve deeper into the mechanisms employed by these formats, accompanied by practical examples.

### 1. Delta Lake

**Canonical List of Data:**
Delta Lake employs a sophisticated transaction log known as the Delta Log. This log meticulously tracks every operation—whether it’s an insertion, update, or deletion—performed on the data. The essence of Delta Lake lies in its ability to manage data through multiple versions stored as Parquet files. This architecture allows for intuitive time travel queries, where users can easily query previous states of the data.

**Delete Files Management:**
When a delete operation is executed, such as removing a specific record, Delta Lake generates a new version of the dataset that excludes the deleted entry while preserving the original data files for historical reference. This dual-versioning ensures historical accuracy and data integrity.

**Example:**
```sql
DELETE FROM my_table WHERE id = 1;
```
Executing this command triggers the creation of a new Parquet file, effectively omitting the record with `id = 1`. The Delta Log contains a detailed entry capturing this deletion, ensuring the current state of the dataset is consistently updated while the original data remains accessible for reference.

### 2. Apache Iceberg

**Canonical List of Data:**
Iceberg elevates data management with its layered metadata architecture. It diligently retains a comprehensive record of all files associated with the table. Each snapshot of the dataset is crafted using columnar formats like Parquet or ORC, which optimizes both storage efficiency and query performance. The design allows for schema evolution without disrupting existing queries.

**Delete Files Management:**
In the case of deletions, Iceberg employs a systematic approach by generating a dedicated delete file. This file maps out the keys of the records that have been removed, allowing users to maintain a complete history of data changes while ensuring efficient access to the current state.

**Example:**
```sql
DELETE FROM iceberg_table WHERE id = '123';
```
This command prompts the system to produce a delete file, clearly documenting the removal of the record with `id = '123'`. The metadata is subsequently updated to reflect this change, ensuring that the original data files remain intact and accessible.

### 3. Apache Hudi

**Canonical List of Data:**
Hudi revolutionizes data storage with its innovative approach of utilizing both base and log files. By catering to different workloads through its Copy-On-Write (COW) and Merge-On-Read (MOR) strategies, Hudi optimizes data access and update processes, making it particularly adept at handling diverse data ingestion scenarios.

**Delete Files Management:**
For deletions, Hudi introduces a delete marker mechanism. When a record is marked for deletion, a special marker is created that indicates which records should be treated as deleted, thus preserving the ability to maintain an accurate historical log of all transactions.

**Example:**
```sql
DELETE FROM hudi_table WHERE id = '456';
```
This action results in the generation of a delete marker for the record with `id = '456'`. The marker acts as a flag, ensuring that while the record technically still exists in the system, it is excluded from query results, thereby maintaining a clean and efficient dataset state.

### 4. Paimon

**Canonical List of Data:**
Paimon is designed with a unique focus on bridging the gap between batch and streaming data. Its architecture enables seamless integration, allowing for real-time processing while effectively managing historical data versions. This dual capability enhances flexibility, catering to a wide array of data consumption patterns.

**Delete Files Management:**
When records need to be deleted, Paimon employs a deletion marker strategy reminiscent of Hudi’s approach. A specific marker indicates which records are deemed deleted, ensuring that they do not appear in any subsequent queries, which is crucial for accurate data retrieval and reporting.

**Example:**
```sql
DELETE FROM paimon_table WHERE user_id = '789';
```
Upon executing this command, a deletion marker is incorporated into the dataset, referencing `user_id '789'` as deleted. This systematic approach helps maintain a clear and accessible data representation, free from unwanted entries.

### Summary

In conclusion, each of these open table formats—Delta Lake, Apache Iceberg, Apache Hudi, and Paimon—presents a refined method for managing the canonical list of data while effectively handling deletions. Through their unique architectures and strategies, they ensure data integrity and historical accuracy:

- **Delta Lake** relies on transaction logs and Parquet file versions.
- **Apache Iceberg** utilizes layered metadata and separate delete files for clear change management.
- **Apache Hudi** embraces delete markers alongside a combination of base and log files.
- **Paimon** effectively unifies streaming and batch data with deletion markers for seamless data access.

This structured approach helps organizations maintain high-quality data while enabling flexible querying and robust operational performance.
