<img src="/img/logo.png" style="border:none !important"/>

---

## 1. Big Data	

---

<img src="/img/bigdata_stack.png"/>

---






## 1.1. Hadoop

----

### Architecture

<img src="/img/HighLevel_hadoop_architecture.png"/>

Note: data is written to HDFS in any way

----

### HDFS

<img src="/img/hdfs-data-distribution.png"/>

----

<img src="/img/hdfs_arch.jpg"/>

 * NN (Namenode) is SPOF (Single Point of Failure)
 * Normally HA (High availability) through a standby
 * Zookeeper for coordination, bookeeping

----

#### Word count

<img src="/img/MapReduceWordCountOverview1.png"/>

----

### Mapreduce architecture

<img src="/img/mapreduce_architecture.png" />


----

### Querying

 * Initially Java

 <img src="/img/mapred.jpg"/>

----

### Querying
 
 * Apache Pig

 <img src="/img/pig.png"/>

 * Apache Hive

  ` SELECT pv_users.gender, count(DISTINCT pv_users.userid), count(*), sum(DISTINCT pv_users.userid) FROM pv_users GROUP BY pv_users.gender; ` 

----

### SQL 
 
 * Everyone knows
 * Easy for analyists
 * But there are actually alot of ways of querying HDFS
  * Cascading, Scalding, Cascalog, etc.

```
class WordCountJob(args : Args) extends Job(args) {
  TypedPipe.from(TextLine(args("input")))
    .flatMap { line => line.split("""\s+""") }
    .groupBy { word => word }
    .size
    .write(TypedTsv(args("output")))
}
```


```
(?- (stdout)
    (<- [?word ?count]
        (sentence :> ?line)
        (tokenise :< ?line :> ?word)
        (c/count :> ?count)))
```


----

### HDP

<img src="/img/hdp_stack.jpg" style="width: 1000px"/>

----

### Business needs

<img src="/img/queries.jpg"/>


Note: because consolidation

----

### Lambda architecture

<img src="/img/LA.png"/>

---


## 2. Presto

----

### Architecture


<img src="/img/presto_arch2.jpg"/>


----

### Architecture

<img src="/img/presto_arch3.jpg">

Note: Coordinator builds query plan, sends tasks to workers |  workers read through connector and run in-memory | client get results from a worker

----

### MR vs Presto

<img src="/img/mapred_vs_presto.jpg"/>

----

### Details

 * Data must fit in memory
 * Uses connectors to several backends
  * Cassandra, Hive, JMX, Kafka, Mysql, Postgres
 * Nodes are stateless

----

### Support

 * Ansi SQL
 * Approximation functions
 * JSON functions
 * PrestoML

----

### Tricks

----

### Best Ones

 * **Vectorized Reader**: read based on column vectors
 * **Predicate Pushdown**: use statistics/logic to skip data
 * **Lazy Load**: postpone loading until needed
 * **Lazy materialization**: postpone decoding until needed

----

### 2.1 Columnar store

<img src="/img/columnar_store.jpg"/>

----

### 2.2 File Size Comparison

<img src="/img/ORCFile.png"/>

Note: columns compress better because of similar values

----

### File Layout

<img src="/img/OrcFileLayout.png"/>


----

### Compression

<img src="/img/lzo_compressions.png"/>

----

### Benchmarks

<img src="/img/bench1.jpg"/>

----

### Benchmarks

<img src="/img/bench2.jpg"/>

----


### Others

 * Fast inner loops (related to CPU cache)
 * sun.misc.Unsafe (non-gc memory access)
 * Pipelining, streaming

----

### Notes

* SQL parser is being used in other projects (ex: crate.io)
* Airpal - UI for presto
* There's also Impala, Apache Drill, Apache Tajo, Redshift, Spark, etc
* PaaS - Presto as a service called Qubole


----

## Netflix

<img src="/img/netflix.jpg"/>


----

http://www.slideshare.net/treasure-data/2015-0311td-techtalkinternalsofprestoservice?qid=702c79ef-0632-476b-abb0-0aaff121cf00&v=default&b=&from_search=12