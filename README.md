# Open Table Formats
## Open Table formats - A Comparative Study
I would like to present a series of concise comparisons regarding the internals of different table formats. While I have offered detailed insights into each format, I believe it’s valuable to highlight both their commonalities and distinctions. These comparisons will emphasize objective facts about their functionalities, steering clear of personal judgments or opinions. My aim is to foster a constructive dialogue rather than engage in debates about which table format may be preferable.

***Disclaimer**: I understand that the debate over Open Table formats can be quite contentious. My intention with any discussion about them is to remain objective and help foster a deeper understanding of the technology involved. I truly appreciate the passion behind these discussions!*


### **Open Lakehouse Architecture**

* **Definition:** A data lakehouse combines the best elements of data lakes and data warehouses, enabling both cost-effective storage and efficient analytics. It offers a unified platform for various workloads like SQL analytics, data science, and machine learning.  
* **Key Characteristics:ACID Transactions:** Supports reliable data modifications, ensuring data consistency.  
* **Schema Evolution:** Allows flexible schema changes to adapt to evolving data requirements.  
* **Unified Governance:** Provides tools for managing data access, security, and auditing.  
* **Scalable Storage:** Leverages cost-effective storage solutions like cloud object stores.  
* **Open Formats:** Uses open file formats like Parquet and ORC for data storage, avoiding vendor lock-in.  
* **Support for Streaming Data:** Enables real-time data ingestion and processing.  
* **BI Support:** Supports business intelligence (BI) and reporting tools.

### **Core Components of Table Formats**

* **Data Files:** Store the actual data in columnar formats like Parquet, ORC, or Avro.  
* **Metadata:** Data that provides information about data files and their lineage and structure.  
* **Table Metadata:** Contains table schema, partitioning information, and other table-level properties.  
* **Commit Logs:** Transaction logs record changes to the table, enabling features like time travel and data versioning.  
* **Snapshots:** Represent a consistent view of the table at a specific point in time.  
* **Indexes:** Enhance query performance by providing optimized data access paths (optional).

### **Key Features and Benefits of Open Lakehouse Table Formats**

* **ACID Transactions:** Ensure data consistency during concurrent reads and writes, preventing data corruption.  
* **Time Travel:** Enables querying historical versions of the table for auditing, debugging, and data recovery.  
* **Schema Evolution:** Simplifies schema changes without requiring full table rewrites, adapting to evolving data structures.  
* **Partitioning:** Improves query performance by dividing data into smaller, more manageable segments based on specified columns.  
* **Data Compaction:** Optimizes data storage by merging small files into larger ones, improving read performance.  
* **Data Skipping:** Reduces I/O by skipping irrelevant data files based on metadata information.  
* **Data Versioning:** Provides a history of table changes, supporting rollback and reproducibility.  
* **Branching:** Lets you create lightweight, isolated branches of a table without duplicating underlying data.  
* **Tagging:** Easily manage versions, collaborate, and maintain data consistency in large-scale environments.

## **Comparative Analysis of Table Formats**

### **Apache Iceberg**

* **Key Concepts:Catalog Integration:** Supports multiple catalogs like Hive, Hadoop, and AWS Glue.  
* **Partitioning:** Allows partitioning tables based on different strategies for query optimization.  
* **Table Evolution:** Supports adding, dropping, and renaming columns without data migration.  
* **Data Management:** Includes features for data cleanup, compaction, and consistency.  
* **Branches and Tags:** Branching provides lightweight, isolated environments for data manipulation. Tags provide versioning and data management.  
* **File Organization:Metadata Location:** Stores table metadata in a metadata directory, which contains files like version-\*.json and manifest-\*.avro.  
* **Data Files:** Data is stored in Parquet, ORC, or Avro files within partition directories.  
* **Manifest Files:** Track the data files belonging to each snapshot.

### **Apache Hudi**

* **Key Concepts:**  
* **Table Types:Copy-on-Write (CoW):** Data is immutable; updates rewrite entire partitions. Simpler, more efficient for read-heavy workloads.  
* **Merge-on-Read (MoR):** Updates are written as delta logs and merged during reads. More efficient for write-heavy workloads.  
* **Data Organization:** Hudi organizes data into partitions, which contain data files. Metadata is stored in the .hoodie directory.  
* **Timeline:** Hudi's timeline tracks all actions on the table, enabling time travel and incremental processing.  
* **File Organization:**  
* **Base Path:** The root directory where the Hudi table is stored (e.g., /data/hudi\_trips/).  
* **Meta Path:** The .hoodie directory containing table metadata and logs.  
* **Partition Path:** Subdirectories within the base path that organize data by partition keys (e.g., /americas/brazil/sao\_paulo/).

### **Apache Paimon**

* **Key Concepts:**  
* **Table Types:** Paimon's key strength is its ability to be a streaming table format.  
* **Incremental Queries:** Supports querying changes between snapshot IDs or time ranges, useful for change data capture (CDC).  
* **Flink Integration:** Paimon is deeply integrated with Apache Flink, making it suitable for stream processing applications.  
* **File Organization:**  
* **Changelog Files:** Paimon materializes change data capture (CDC) files for commits, providing a record of data changes.  
* **Bucket Structure:** Data is organized into buckets, each containing changelog and data files.  
* **Snapshots View:** The table$snapshots view provides information about available snapshots.

### **Delta Lake**

* **Key Concepts:**  
* **Delta Log:** A transaction log that tracks all changes to the table.  
* **ACID Transactions:** Ensures data consistency with serializable isolation levels.  
* **Time Travel:** Supports querying previous versions of the table.  
* **Schema Enforcement:** Enforces a schema on write to ensure data quality.  
* **Change Data Feed:** Provides a mechanism for capturing changes to the table.  
* **Deletion Vectors:** Enables soft deletes by marking rows as deleted without physically removing them.  
* **File Organization:**  
* **Delta Files:** JSON files that store the transaction log.  
* **Checkpoint Files:** Periodic snapshots of the table state to accelerate recovery.  
* **Data Files:** Parquet files that store the actual data.

### **Common Operations**

* **Creating Tables:** Example: CREATE TABLE iceberg\_catalog.db.order\_h (...) PARTITIONED BY (year(Order\_ts), st)  
* Each format provides specific syntax for creating tables, defining schema, and specifying partitioning.  
* **Inserting Data:** Example: INSERT INTO iceberg\_catalog.db6.order\_h VALUES (...)  
* Data can be inserted using SQL statements or by writing DataFrames from Spark or other processing engines.  
* **Querying Data:** Example: SELECT \* FROM iceberg\_catalog.nyc.taxis\_COW  
* Standard SQL syntax is used to query data, with support for filtering, aggregation, and joins.  
* **Updating Data:** Updates depend on table type. Copy-on-write involves rewriting files; merge-on-read uses delta logs.  
* **Deleting Data:** Deletion can be physical (removing files) or logical (using deletion vectors).  
* **Time Travel:** Example: SELECT \* FROM iceberg\_catalog.db.movies VERSION AS OF 'tg\_88'  
* Querying data as of a specific version, tag, or timestamp.  
* **Branching:** Example: ALTER TABLE iceberg\_catalog.db.permits CREATE BRANCH etl\_today  
* **Tagging:** Example: ALTER TABLE iceberg\_catalog.db.movies CREATE TAG tg\_88 RETAIN 365 DAYS  
* **Compaction:** Process of merging smaller data files into larger ones to improve read performance.  
* **Cleanup:** Removing obsolete data files and metadata to reclaim storage and maintain performance.

## **Implementation and Configuration**

### **Spark Integration**

* **Configuring Catalogs:** Example:  
* spark.sql.catalog.iceberg\_catalog.type=hadoop  
* spark.sql.catalog.iceberg\_catalog.warehouse=s3://warehouse/path  
* **Spark Session Extensions:** Used to enable specific table format features within Spark.  
* Example: org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions

### **Catalog Management**

* **Hive Metastore:** A central repository for storing table metadata, commonly used with Hadoop-based systems.  
* Configuration involves setting up the Hive Metastore URI and necessary dependencies.  
* **AWS Glue Catalog:** A serverless metadata catalog service provided by AWS.  
* Integration requires configuring AWS credentials and specifying the Glue Catalog endpoint.  
* **Hadoop Catalog:** A simple catalog implementation that stores metadata on the Hadoop file system.  
* **Polaris Internal Catalog:** A proprietary catalog, sometimes used in conjunction with other lakehouse frameworks. Requires specific setup based on the vendor's documentation.  
* **Snowflake Integration:** Uses external volumes and catalogs to access data stored in object storage like S3.

### **Common Code Examples**

* **Creating a Database:** spark.sql("CREATE DATABASE IF NOT EXISTS iceberg\_catalog.nyc")  
* **Showing Tables:** spark.sql("SHOW TABLES").show()  
* **Setting Table Properties:** spark.sql("ALTER TABLE iceberg\_catalog.db.taxis\_COW SET TBLPROPERTIES ('write.wap.enabled' \= 'true')

### **Data Migration**

* **Copying Data:** Moving data from Hive tables to object storage like S3 in Parquet format.  
* Example: aws s3 cp \<local\_path\> s3://\<bucket\_name\>/\<path\>  
* **Creating Iceberg Tables from Migrated Data:** Defining Iceberg tables that point to the migrated data location.  
* Ensuring the metadata is correctly configured to reflect the data layout.

## **Advanced Topics**

### **Streaming Ingestion**

* **Reading Streams:** Reading data from streaming sources like Kafka or Kinesis using spark.readStream.  
* **Writing Streams:** Writing streaming data to table formats using df.writeStream.  
* **Schema Definition:** Defining schema for incoming data streams to ensure compatibility with the table format.

### **Security and Governance**

* **Access Control:** Implementing access control policies to restrict data access based on user roles or permissions.  
* **Data Masking:** Protecting sensitive data by masking or redacting information.  
* **Encryption:** Encrypting data at rest and in transit to prevent unauthorized access.

### **Performance Optimization**

* **Partitioning Strategies:** Selecting appropriate partition keys to optimize query performance.  
* Considerations for cardinality, data skew, and query patterns.  
* **Data Skipping Techniques:** Leveraging metadata information to skip irrelevant data files during query execution.  
* **Compaction and Vacuuming:** Managing data files and metadata to maintain optimal storage and query performance.  
* Regular compaction to consolidate small files.  
* Vacuuming to remove obsolete data and metadata.  
* **File Formats:** Using Parquet for its columnar storage, compression, and encoding schemes, optimized for analytical queries.  

**Reference**:

[Apache Iceberg (v2)](https://iceberg.apache.org/spec/#version-2-row-level-deletes)

[Apache Iceberg (v3)](https://iceberg.apache.org/spec/#version-3-extended-types-and-capabilities) 

[Delta Lake (v2.4+)](https://github.com/delta-io/delta/blob/master/PROTOCOL.md#delta-table-specification)

[Apache Hudi (v5](https://hudi.apache.org/tech-specs/)) 

[Apache Paimon (0.8)](https://paimon.apache.org/docs/0.8/concepts/specification/)

[Table formats TLA+ and Fizzbee](https://github.com/Vanlightly/table-formats-tlaplus/tree/main)


## **Glossary of Key Terms**

* **ACID Transactions:** A set of properties that guarantee reliable processing of database transactions (Atomicity, Consistency, Isolation, Durability).  
* **Base Path:** The root directory where data is stored (e.g., in Apache Hudi).  
* **Branching:** Allows users to create lightweight, independent branches of a table without duplicating the underlying data.  
* **Change Data Capture (CDC):** The process of identifying and tracking changes to data in a database or data warehouse.  
* **Commit Log:** A transaction log that records all changes to a table.  
* **Compaction:** The process of merging smaller data files into larger ones to optimize storage and query performance.  
* **Copy-on-Write (CoW):** A table type where updates rewrite entire data files.  
* **Data Catalog:** A metadata management tool that stores table schemas, locations, and other metadata.  
* **Data Lakehouse:** A data management architecture that combines the best elements of data lakes and data warehouses.  
* **Data Skipping:** A technique to reduce I/O by skipping irrelevant data files based on metadata.  
* **Delta Lake:** An open-source storage layer that brings ACID transactions to Apache Spark and big data workloads.  
* **Deletion Vector:** A data structure used to mark rows as deleted without physically removing them.  
* **File Format:** The format in which data is stored, such as Parquet, ORC, or Avro.  
* **Hadoop Catalog:** A simple catalog implementation that stores metadata on the Hadoop file system.  
* **Hive Metastore:** A central repository for storing metadata about Hive tables, schemas, and partitions.  
* **Apache Hudi:** An open-source data lake platform that provides support for incremental data processing and data management.  
* **Apache Iceberg:** An open table format for large analytic datasets.  
* **Incremental Queries:** Supports querying changes between snapshot IDs or time ranges.  
* **Merge-on-Read (MoR):** A table type where updates are written as delta logs and merged during reads.  
* **Apache Paimon:** A streaming data lake platform that provides a unified architecture for batch and stream processing.  
* **Partitioning:** The division of a table into smaller, more manageable parts based on column values.  
* **Polaris Internal Catalog:** A proprietary catalog used in conjunction with lakehouse frameworks.  
* **Schema Evolution:** The ability to modify a table's schema without rewriting the entire table.  
* **Snapshot:** A consistent view of a table at a specific point in time.  
* **Tagging:** Easily manage versions, collaborate, and maintain data consistency in large-scale environments.  
* **Time Travel:** The ability to query historical versions of a table.  
