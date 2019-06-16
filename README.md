# Apache Cassandra Admin Exam Notes
Apache Cassandra 3.x Administrator Associate Certification Exam Notes


### DS201: DataStax Enterprise 6 Foundations of Apache Cassandra - https://academy.datastax.com/resources/ds201-datastax-enterprise-6-foundations-of-apache-cassandra ###
* Partitions
* Clustering Columns
* Application Connectivity
* Node
* Ring
* Peer to Peer
* Vnodes
* Gossip
* Snitch
* Replication
* Consistency
* Hinted Handoff
* Read Repair
* Node Sync
* Write Path
* Read Path
* Compaction
* Advanced Performance

### DS210: DataStax Enterprise 6 Operations with Apache Cassandra -https://academy.datastax.com/resources/ds210-datastax-enterprise-6-operations-with-apache-cassandra ###
* Clusters
* Cluster Sizing
* cassandra-stress
* top
* dstat
* nodetool
* JVM
* Adding Nodes
* Removing Nodes
* Bootstrapping
* Replacing a Downed Node
* Size Tiered Compaction
  * Default compaction strategy.
  * Good for insert heavy and general workloads.
* Leveled Compaction
  * Groups sstables into levels of a fixed size limit.
  * Each level is 10 times larger than the previous.
  * Best for read heavy workloads or where there are more updates than inserts.
* Time Window Compaction
  * Designed to work on time-series data.
  * One sstable for each time window.
* Repair
* Nodesync
* sstablesplit
* Backup
* JVM
* Garbage Collection
* Heap Dump
* Kernel Tuning
* Hardware
* Cloud
* Security
