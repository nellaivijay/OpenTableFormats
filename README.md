# OpenTableFormats
##Open Table formats - A Comparative Study
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


**Reference**:

[Apache Iceberg (v2)](https://iceberg.apache.org/spec/#version-2-row-level-deletes)

[Apache Iceberg (v3)](https://iceberg.apache.org/spec/#version-3-extended-types-and-capabilities) 

[Delta Lake (v2.4+)](https://github.com/delta-io/delta/blob/master/PROTOCOL.md#delta-table-specification)

[Apache Hudi (v5](https://hudi.apache.org/tech-specs/)) 

[Apache Paimon (0.8)](https://paimon.apache.org/docs/0.8/concepts/specification/)

