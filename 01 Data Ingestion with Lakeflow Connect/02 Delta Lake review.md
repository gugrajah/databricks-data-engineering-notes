### Delta Lake Review

Delta Lake (DLake) delivers open, reliable, and scalable data management for the Lakehouse, empowers engineers to ingest data from external sources and efficiently manage it across Bronze (raw), Silver (cleaned), and Godl (curated) layers - all with:
- ACID transacts (atomicity, consistency, isolation,a nd durability)
- time travel schema enforcement 
- and support for batch and streaming workloads
- suports Data Manipulation Language ops, enabling flexible data management

DLake is an open-source protocol for **reading and writing files** to cloud storage (as delta tables). **Delta tables** offer an open format that supports the Lakehouse architecture

Delta tables store data w/in a folder directory. W/in the directory, data is stored as parquet files, and what delat adds is delta logs stored as JSON files. The Delta logs keep track of all of the transacts on the data (parquet files) and table versions.