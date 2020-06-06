# Hadoop

## What is Hadoop?

In simple words, Hadoop is the combination of HDFS (Hadoop distributed file system) and the MapReduce structure. In Hadoop 2, Yarn is introduced.

## Why use Hadoop?

### Compare with traditional "one-disk" system

Mainly because of the boost in read and write time, because it is usually the bottleneck of a disk.

### Compare with RAID system

MapReduce provided the functionality to combine files from multiple sources into one meaning data.

### Compare with RDBMS (Relational Database Management System)

1. Seek time is improving more slowly than transfer rate. Seeking is the process of moving the disk's head to a particular place on the disk to read or write data. Traditional RDBMS uses B-tree as data structure, which is limited by the rate at which it can perform seeks. In general, RDBMS works better for datasets that are continually updated and many read and writes are needed, while MapReduce suits applications where the data is written once and read many times.
2. Hadoop works well on unstructured data because it is designed to interpret the data at processing time (so-called scheme-on-read, bascially just a file-copy). On the other hand, RDBMS supports structured data (XML, database tables).
3. MapReduce generally scales linearly, but other SQL queries usually are not.

### Compare with Volunteer Computing

SETI@home is a project which volunteers donate idle CPU time from their computer to analyze telescope data.

1. Volunteer computing focuses highly on computation. Bandwidth is not a precious resource.

## Evolution of Hadoop

Due to Hadoop's lack of "interactiveness" because of the slow query speed, it usually only serves as a batch processing system (which might be ran weekly or monthly). Apache has since developed several other systems to compensate this weakness.

- Interactive SQL: a low-latency response for SQL quieries on HDFS (Impala, Hive on Tez)
- Iterative processing: Saving intermediate data in memory for rapid access (Spark)
- Stream processing: Real-time, distributed computation on unbounded streams of data (Storm, Spark Streaming, Samza)
- Search: Indexingdata and serve search query from indexes stored in HDFS (Solr)

## Key features of Hadoop

- Data locality: Data is co-lated with compute nodes, therefore data access is fast
- Shared-nothing architecture: Data have no dependence on one other, therefore order and failure of compute nodes is not of progammer's concern.