# Apache Cassandra Admin Exam Notes
Apache Cassandra 3.x Administrator Associate Certification Exam Notes


### DS201: DataStax Enterprise 6 Foundations of Apache Cassandra - https://academy.datastax.com/resources/ds201-datastax-enterprise-6-foundations-of-apache-cassandra ###
* Partitions
  * Group rows physically together on disk based on the partition key.
  * The partitioner hashes the partition key values to create a partition token.
  * Used to place data on the ring.
  * https://www.datastax.com/dev/blog/the-most-important-thing-to-know-in-cassandra-data-modeling-the-primary-key
  * https://docs.datastax.com/en/archived/cql/3.3/cql/cql_using/useCompositePartitionKeyConcept.html
* Clustering Columns
  * Non-partition section of the PK.
  * Clustering columns order data within a partition. https://docs.datastax.com/en/dse/5.1/cql/cql/cql_using/whereClustering.html
  * Equality or range queries can be performed on clustering columns.
  * Default ascending order.
  * https://docs.datastax.com/en/dse/6.7/cql/cql/cql_using/useCompoundPrimaryKeyConcept.html
  * https://www.bmc.com/blogs/cassandra-clustering-columns-partition-composite-key/
* Application Connectivity
  * Drivers - https://docs.datastax.com/en/driver-matrix/doc/driver_matrix/common/driverMatrix.html
* Node
  * 6K-12K transactions/seconds/core
  * 2-4TB
* Ring
  * Notes can have four statuses in the ring UP / DOWN / JOINING / LEAVING.
  * https://academy.datastax.com/units/2017-ring-dse-foundations-apache-cassandra?resource=ds201-datastax-enterprise-6-foundations-of-apache-cassandra
* Peer to Peer
  * No one is a leader, no one is a follower. All nodes are equal.
* Vnodes
  * Vnodes help keep a cluster balanced as new Apache Cassandra nodes are introduced to the cluster and old nodes are decommissoned, and also automate token range assignment. With vnodes, each node is responsible for several smaller slices of the ring, instead of just one large slice.
  * default number of vnodes = 128.
  * Can be configured in the num_tokens parameter in cassandra.yaml.
  * https://docs.datastax.com/en/archived/cassandra/3.0/cassandra/architecture/archDataDistributeVnodesUsing.html
  * https://www.youtube.com/watch?v=G4SMNU1aOJg
* Gossip
  * A Gossip round is initiated by a Node every second.
  * Picks 1-3 Nodes to gossip with.
  * Can gossip with any Node but favours (slightly) seeds and downed Nodes.
  * https://www.edureka.co/blog/gossip-protocol-in-cassandra/
  * https://www.linkedin.com/pulse/gossip-protocol-inside-apache-cassandra-soham-saha/
* Snitch
  * GossipingPropertyFileSnitch is the default. Configured in cassandra-rackdc.properties.
* Replication
* Consistency
  * ANY - Store a hint.
  * ONE, TWO, THREE - Closest to Coordinator.
  * QUORUM - Majority vote.
  * LOCAL_ONE - Limit to local DC.
  * LOCAL_QUORUM Majority vote in local DC.
  * EACH_QUORUM - Majority vote in each DC.
  * ALL - All nodes must participate.
* Hinted Handoff
  * Default 3 hours.
  * Enabled by default.
* Read Repair
  * Always occurs when consistency level = ALL.
  * read_repair_chance - Sets the probability which Cassandra will perform a read repair with a consistency level less than ALL.
* Node Sync
   DataStax Enterprise 6 feature.
* Write Path
* Read Path
  * Bloom filter > Key Cache > Partition Summary > Partition Index > SSTable.
  * Partition Summary - In memory data structure storing byte offsets into the partition index.
  * The key cache - Stores the byte offset of the most recently accessed records.
  * Bloom Filter - A Bloom filter is a space-efficient probabilistic data structure, conceived by Burton Howard Bloom in 1970, that is used to test whether an element is a member of a set. False positive matches are possible, but false negatives are not – in other words, a query returns either "possibly in set" or "definitely not in set".
  * http://cassandra.apache.org/doc/latest/operating/bloom_filters.html
  * https://en.wikipedia.org/wiki/Bloom_filter
* Compaction
  * Process used to remove stale data from existing sstables.
  * Compaction Strategies
    * SizeTiered Compaction - Default. Triggers when multiple sstables of a similar sire are present. Good for high writes.
    * Leveled Compaction - Groups sstables into levels. Each level has a fixed size limit which is 10 times larger than the previous level. Good to read heavy use-cases.
    * TimeWindow Compaction - Create time windowed buckets of sstables that are compacted using the Size Tiered compaction strategy.
* Advanced Performance

### DS210: DataStax Enterprise 6 Operations with Apache Cassandra -https://academy.datastax.com/resources/ds210-datastax-enterprise-6-operations-with-apache-cassandra ###
* Configuring Clusters
  * cassandra.yaml - Main configuration file.
  * cluster_name - Default "Test Cluster".
  * listen_address - Default localhost.
  * native_transport_address - Default localhost.
  * seeds - Default "127.0.0.1".
  * endpoint_snitch - Default SimpleSnitch.
  * initial_token - Default 128 (or is it 256?? Some confusion https://datastaxacademy.slack.com/archives/C4ZQ1EWNM/p1560929813009900).
  * commitlog_directory - Default /var/lib/cassandra/commitlog.
  * data_file_directories - Default /var/lib/cassandra/data.
  * hints_direcory - Default /var/lib/cassandra/hints.
  * saved_caches_directory - Default /var/lib/cassandra/saved_caches.
* Cluster Sizing
  * Throughput.
  * Growth Rate.
  * Latency.
* cassandra-stress
  * Benchmarking tool used to determine schema performance, scaling and determine production capacity.
  * Configured through a yaml file.
  * Example here - https://github.com/justinbreese/dse-cassandra-stress/blob/master/stress.yaml
* top
* dstat
  * Versatile tool for generating system resource statistics.
  * https://linux.die.net/man/1/dstat
  * dstat -am - All default stats and memory.
* nodetool
  * A command line interface for managing a cluster.
  * http://cassandra.apache.org/doc/latest/tools/nodetool/nodetool.html
  * https://docs.datastax.com/en/archived/cassandra/3.0/cassandra/tools/toolsNodetool.html
  * https://www.youtube.com/watch?v=0qr9z0lsbuk
  * https://www.youtube.com/watch?v=Sz7OiUWgs5U
* JVM
* Adding Nodes
  * Add a single node at a time.
  * https://docs.datastax.com/en/archived/cassandra/3.0/cassandra/operations/opsAddNodeToCluster.html
* Removing Nodes
  * nodetool decommission - Causes a live node to decommission itself, streaming its data to the next node on the ring to replicate appropriately.
  * nodetool removenode - Shows the status of current node removal; forces completion of pending removal, or removes identified node. Use when the node is down and nodetool decommission cannot be used. If the cluster does not use vnodes, adjust the tokens before running this command.
  * nodetool assassinate - Just make the node go away. Doesn't redistribute any data.
  * https://docs.datastax.com/en/archived/cassandra/3.0/cassandra/operations/opsRemoveNode.html
* Bootstrapping
  * The process of a new node joining a cluster.
  * http://cassandra.apache.org/doc/latest/operating/topo_changes.html
  * https://thelastpickle.com/blog/2017/05/23/auto-bootstrapping-part1.html
  * https://thelastpickle.com/blog/2018/08/02/Re-Bootstrapping-Without-Bootstrapping.html
  * https://de.slideshare.net/ArunitGupta1/boot-strapping-in-cassandra
* Replacing a Downed Node
  * Replace better than remove and add.
  * In jvm.options add replace_address or (better) replace_address_first_boot.
  * Monitor with nodetool netstats.
* Size Tiered Compaction
  * Default compaction strategy.
  * Good for insert heavy and general workloads.
  * Groups similarly sized tables together.
  * Tiers with less than min_threshold (four) sstables are not considered for compaction.
* Leveled Compaction
  * Groups sstables into levels of a fixed size limit.
  * Each level is 10 times larger than the previous.
  * Best for read heavy workloads or where there are more updates than inserts.
  * Very IO intensive.
  * Compacts more frequently than size tiered.
* Time Window Compaction
  * Designed to work on time-series data.
  * One sstable for each time window.
  * size tiered within a time window.
* Repair
  * Synchronizing replicas.
  * Occurs when detected by reads, randomly with non_quorum reads (read_repair_chance) or manually with nodetool repair.
  * Repairs should be run if a node is down for a long time.
  * Regular repair should also be run to ensure the integrity of cluster data.
* Nodesync
* sstablesplit
  * Splits SSTable files into multiple SSTables of a maximum designated size.
  * Do not execute when Cassandra is running.
  * We might sometimes wish to split a very large sstable to ensure compaction occurs on it.
  * sstablesplit -m 500 /path/to/massive/sstable-Data.db
* Backup
* JVM
* Garbage Collection
* Heap Dump
* Kernel Tuning
  * https://docs.datastax.com/en/dse/6.7/dse-dev/datastax_enterprise/config/configRecommendedSettings.html
  * https://tobert.github.io/pages/als-cassandra-21-tuning-guide.html
* Hardware
* Cloud
* Security

### Quiz Trivia ###

* What is the node that handles a request called? Coordinator node.
* Write Path Order Commitlog > MemTable > SSTable.
* Cassandra does not do any writes or deletes in place.
* SSTables are immutable.
* Compaction is the progress of taking small SSTables and merges them into bigger ones.
* Last writes wins - based on Timestamps.
*
