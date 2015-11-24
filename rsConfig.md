---
layout: article
title: 'Configuring RecordService'
share: false 
---

{% include toc.html %}

## Client Configurations

While it should not be necessary to change the default configuration, you have the option of modifying any of these settings.

<table border="1">    
<tr><th>CATEGORY</th><th>PARAMETER</th><th>DESCRIPTION</th><th> DEFAULT VALUE </th></tr>
<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.planner.hostports</td><td style="vertical-align:top">Comma separated list of planner service host/ports.</td><td style="vertical-align:top">localhost:12050</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.kerberos.principal</td><td style="vertical-align:top">Kerberos principal for the planner service. Required if using Kerberos.</td><td style="vertical-align:top"></td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.planner.retry.attempts</td><td style="vertical-align:top">Maximum number of attempts to retry RecordService RPCs with Planner.</td><td style="vertical-align:top">3</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.planner.retry.sleepMs</td><td style="vertical-align:top">Sleep between retry attempts with Planner in milliseconds.</td><td style="vertical-align:top">5000</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.planner.connection.timeoutMs</td><td style="vertical-align:top">Timeout when connecting to the Planner service in milliseconds.</td><td style="vertical-align:top">30000</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.planner.rpc.timeoutMs</td><td style="vertical-align:top">Timeout for Planner RPCs in milliseconds.</td><td style="vertical-align:top">120000</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.worker.retry.attempts</td><td style="vertical-align:top">Maximum number of attempts to retry RecordService RPCs with a Worker.</td><td style="vertical-align:top">3</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.worker.retry.sleepMs</td><td style="vertical-align:top">Sleep in milliseconds between retry attempts with Worker.</td><td style="vertical-align:top">5000</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.worker.connection.timeoutMs</td><td style="vertical-align:top">Timeout when connecting to the Worker service in milliseconds.</td><td style="vertical-align:top">10000</td></tr>

<tr align="left"><td style="vertical-align:top">Connectivity</td><td style="vertical-align:top">recordservice.worker.rpc.timeoutMs</td><td style="vertical-align:top">Timeout for Worker RPCs in milliseconds.</td><td style="vertical-align:top">120000</td></tr>

<tr align="left"><td style="vertical-align:top">Performance</td><td style="vertical-align:top">recordservice.task.fetch.size</td><td style="vertical-align:top">Configures the maximum number of records returned when fetching results from the RecordService. If not set, the server default is used. <br/><br/>
Note: This might need to be adjusted according to the type of workloads (MR, Spark, etc), due to the differences in the data processing speed.</td><td style="vertical-align:top">5000</td></tr>

<tr align="left"><td style="vertical-align:top">Resource Management</td><td style="vertical-align:top">recordservice.task.memlimit.bytes</td><td style="vertical-align:top">Maximum memory the server should use per task. Tasks exceeding this limit are aborted.  If not set, the server process limit is used.</td><td style="vertical-align:top">-1 (Unlimited)</td></tr>

<tr align="left"><td style="vertical-align:top">Resource Management</td><td style="vertical-align:top">recordservice.task.plan.maxTasks</td><td style="vertical-align:top">Hint for maximum number of tasks to generate per PlanRequest. This is not strictly enforced by the server, but is used to determine if task combining should occur. This value might need to be set for large datasets.</td><td style="vertical-align:top">-1 (Unlimited)</td></tr>

<tr align="left"><td style="vertical-align:top">Resource Management (Advanced)</td><td style="vertical-align:top">recordservice.task.records.limit</td><td style="vertical-align:top">Maximum number of records returned per task.</td><td style="vertical-align:top">-1 (Unlimited)</td></tr>

<tr align="left"><td style="vertical-align:top">Logging (Advanced)</td><td style="vertical-align:top">recordservice.worker.server.enableLogging</td><td style="vertical-align:top">Enable server logging (logging level from Log4j).</td><td style="vertical-align:top">FALSE</td></tr>
</table>

## Server Configurations

The properties listed on the Cloudera Manager RecordService Configuration page are the ones Cloudera considers the most reasonable to change. However, adjusting these values should not be necessary. Very advanced administrators might consider making minor adjustments.

### Kerberos Configuration

No special configuration is required via Cloudera Manager. Enabling Kerberos on the cluster configures everything.

### Sentry Table Configuration

Sentry is configured for you in the RecordService VM. This section describes how to configure Sentry in a  non-VM deployment.

#### Prerequisite

Follow CDH documentation to install Sentry and enable it for Hive (and Impala, if applicable).

See [http://www.cloudera.com/content/cloudera/en/documentation/core/latest/topics/sg_sentry_service_install.html](http://www.cloudera.com/content/cloudera/en/documentation/core/latest/topics/sg_sentry_service_install.html).

#### Configure Sentry with RecordService

1. Enable RecordService to read policy metadata from Sentry:
    * In Cloudera Manager, navigate to the **Sentry Configuration** page.
    * In **Admin Groups**, add the user *recordservice*.
    * In **Allowed Connecting Users**, add the user *recordservice*.
1. Save changes.
1. Download the Cloudera Manager generated `sentry-site.xml` configuration file (same config as HiveServer2)
    * In Cloudera Manager, navigate to **Hive** -> **HiveServer2** -> **Processes** -> `sentry-site.xml`
1. Deploy configurations to cluster: copy `sentry-site.xml` to the `/etc/sentry/conf/` directory on all nodes running RecordService Planner or RecordService Planner and Worker. Nodes that are only running only the RecordService Worker role do not need this configuration option.
1. Enable Sentry for RecordService
    * In Cloudera Manager, navigate to **RecordService Configuration**.
    * Select the **Sentry** service.
    * In the **Sentry Configuration File** field, enter `/etc/sentry/conf/sentry-site.xml.
1. Save changes.
1. Restart the Sentry and RecordService services. 

### Delegation Token Configuration

No special configuration is required with Cloudera Manager. This is enabled automatically if the cluster is kerberized.

RecordService persists state in Zookeeper, by default, under the /recordservice Zookeeper directory. If this directory is already in use, you can configure the directory with `recordservice.zookeeper.znode`. This is a Hadoop style XML configuration that you can add to the advanced service configuration snippet.
