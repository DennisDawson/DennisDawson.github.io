---
layout: article
title: 'RecordService Beta Release Notes'
share: false
---

This release of RecordService is a public beta and should not be run on production clusters. During the public beta period, RecordService is supported through the mailing list <a href="mailto:RecordService-user@googlegroups.com">RecordService-user@googlegroups.com</a>, not through the Cloudera Support Portal.

As you use RecordService during the public beta period, keep in mind the following:

* The RecordService team responds to beta issues as quickly as possible, but cannot commit to issue-resolution or bug-fix delivery times during the public beta period.

* There is no guarantee that a bug will be fixed in a future release.

* The RecordService team does not provide patches for beta releases, and cannot guarantee upgrades from this release to later releases.

* Although multiple releases of beta code might be planned, the contents are not guaranteed. There is no schedule for future beta code releases. Any releases are announced to the user group as they occur.

{% include toc.html %}

## RecordService VM Requirements

RecordService VM requires VirtualBox version 4.3 or 5. You can download a free copy of VirtualBox at [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads).

## Platform and Hardware Support

RecordService supports the following software and hardware configurations when running on your own Hadoop cluster:

* CDH 5.4
* Server support: RHEL5 and RHEL6, Ubuntu LTS, SLES, and Debian
* Intel Nehalem (or later) or AMD  Bulldozer (or later) processor
* 64 GB memory
* For optimal performance, run with 12 or more disks, or use SSD.

## Storage and File Format Support

RecordService supports reading HDFS or S3 of the following file formats:

* Parquet
* Text
* Sequence file
* RC
* Avro

## Data Type Support
RecordService supports the following data types:

* INT (8-64 bits)
* CHAR/VARCHAR
* BOOL
* FLOAT
* DOUBLE
* DECIMAL
* STRING
* TIMESTAMP

RecordService does not support the following data types:

* BLOB/CLOB
* Nested Types


## Known Issues

### RecordService client configurations are not properly propagated to Spark jobs

RecordService configuration options are not propagated to Spark jobs using the RecordService custom service descriptor (CSD). All configuration options must be specified in the job or through Cloudera Manager safety valves for Spark.

#### Workaround:

* Apply configuration options using the Spark configuration safety valve: 
    **Spark** -> **Configuration** -> **Spark (Standalone) Client Advanced Configuration Snippet (Safety Valve) for spark-conf/spark-defaults.con**

```
spark.recordservice.planner.hostports=<comma separated list of planner host:ports>
```

* If the cluster is Kerberized, also set:

```
spark.recordservice.kerberos.principal=<Kerberos principal> 
```

* Save changes and deploy the client configuration.

### digest-md5 library not installed on cluster, breaking delegation tokens

The digest-md5 library is not installed by default in parcel deployments.

#### Workaround
To install the library on RHEL 6, use the following command-line instruction:

```
sudo yum install cyrus-sasl-md5
```

### Short circuit reads not enabled

#### Workaround

In Cloudera Manager, open the HDFS configuration page and search for _shortcircuit_. There are two configurations named **Enable HDFS Short Circuit Read**. One defaults to _true_ and one to _false_. Set both values to _true_.

### Path requests do not work with multiple planners

The RecordService planner creates a temporary table. The name of the table collides between RecordService planners. On a single planner, it is properly protected by a lock. On the Hive Metastore Server, collisions are likely to occur.

#### Workaround

For the beta release, run only one instance of the RecordService planner.

## Limitations

### Security Limitations

* RecordService only supports simple single-table views (no joins or aggregations).
* SSL support has not been tested.
* Oozie integration has not been tested.

### Storage/File Format Limitations

* No support for write path.
* Unable to read from Kudu or HBase.

### Operation and Administration Limitations

* No diagnostic bundle support.
* No metrics available in Cloudera Manager.

### Application Integration Limitations

* Spark DataFrame is not well tested.


[overview]: {{site.baseurl}}/

