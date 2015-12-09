---
layout: article
title: 'RecordService Examples'
share: false
---

You can find the source code for RecordService examples in the RecordService Client GitHub repository.

[https://github.com/cloudera/RecordServiceClient/tree/master/java/examples](https://github.com/cloudera/RecordServiceClient/tree/master/java/examples) 

Instructions for running the examples are stored in the repository with the source code.

{% include toc.html %}

## Hadoop and MapReduce Examples

| Example | Description |
|:--------|:--------|
| RSCat | This example shows how you can output tabular data for any dataset readable by RecordService. It demonstrates a standalone Java application built on the core libraries without using a computation framework such as MapReduce or Spark. |
| SumQueryBenchmark | This demonstrates running a sum over a column and pushing the scan to RecordService. It shows how you can use RecordService to accelerate scan-intensive operations.
| Terasort | This is a port of the Hadoop [Terasort](https://hadoop.apache.org/docs/current/api/org/apache/hadoop/examples/terasort/package-summary.html) benchmark test ported to RecordService. See README in the Terasort package for more details. This also demonstrates how to implement a custom InputFormat using the RecordService APIs. |
| MapredColorCount / MapreduceAgeCount / MapReduceColorCount | These examples are ported from Apache Avro. They demonstrate the steps required to port an existing Avro-based MapReduce job to use RecordService. |
| RecordCount/WordCount | More MapReduce applications that demonstrate some other InputFormats included in the client library, including TextInputFormat and RecordServiceInputformat. Useful for existing MapReduce jobs already using TextInputFormat. |
|Reading data from a View, and Enforcing Sentry Permissions  (uses RecordCount) | This example demonstrates how an MR job can now read data even when the user only has permission to see part of the data in a file (table). See [https://github.com/cloudera/RecordServiceClient/tree/master/java/examples#how-to-enforce-sentry-permissions-with-mapreduce](https://github.com/cloudera/RecordServiceClient/tree/master/java/examples#how-to-enforce-sentry-permissions-with-mapreduce) |
| com.cloudera.recordservice.examples.avro | Unmodified from the Apache Avro examples, these utilities help you generate sample data. |

## RecordService Spark Examples

The following examples can be found at [https://github.com/cloudera/RecordServiceClient/tree/master/java/examples-spark](https://github.com/cloudera/RecordServiceClient/tree/master/java/examples-spark).

| Example | Description |
|:--------|:--------|
| Query1/Query2 | Examples that demonstrate RecordService native RDD Resilient Distributed Dataset (RDD) integration using RecordServiceRDD. |
| WordCount | The Hadoop WordCount example built on top of the RecordService equivalent of textFile(). |
| SchemaRDDExample | Another example of the native RDD integration, this time using SchemaRecordServiceRDD. |
| TeraChecksum | This example uses hadoopFile() with the RecordService InputFormats. This is a port of the TeraChecksum MapReduce job, written in Spark.|
| TpcdsBenchmark | This demonstrates the SparkSQL integration running a portion of the tpcds benchmark. |
| DataFrameExample | An example that demonstrates DataFrames and RecordService working together. |
| How to use RecordService with Spark shell | Examples of using spark-shell to interact with RecordService in a variety of ways.|
| Reading Data from a View and Enforcing Sentry Permissions | This example demonstrates how an MR job may now read data even when the user only has permission to see part of the data in a file (table). See [ReadMe.md](https://github.com/cloudera/RecordServiceClient/blob/master/java/examples-spark/README.md#how-to-enforce-sentry-permissions-with-spark) |

## Using RecordService to Control RecordService Access

RecordService provides column-level security. You can restrict users in a group to a subset of columns in a dataset. This allows you to maintain a single, secure dataset that can be viewed and updated by users with specific access rights to only the columns they need.

For example, this schema describes a table that stores information about employees.

| column_name | column_type|
|---|---|
| firstname | string |
| lastname | string |
| position | string |
| department | string |
| salary | long |
| phone | long |

Suppose you want your employees to have access to the names and phone numbers, but not the position or salary info. You can create a group that allows users to see only the columns you want them to see.

To assign access with column-level restrictions, you create a role, assign the role to a group, and then grant permissions to the role.

* [Download and Install the RecordService VM]({{site.baseurl}}/vm.html).

* Connect to the RecordService VM using the SSH command `ssh cloudera@quickstart.cloudera`.

* Enter the password `cloudera`.

* Restart Sentry using the following-command line instruction:
```
sudo service sentry-store restart
```
* Create a group named _demogroup_:
`sudo groupadd demogroup`

* Add the current user to the group:
`sudo usermod -a -G demogroup $USER`

* Currently, you have access to the entire table. Use Impala to select all records from the `rs.employees` table:

<pre>
$ sudo -u $USER impala-shell
[quickstart.cloudera:21000] > use rs;
Query: use rs
[quickstart.cloudera:21000] > select * from rs.employees;
Query: select * from rs.employees
+-----------+-------------+-----------------------------------------------+------------------+--------+-------------+
| firstname | lastname    | position                                      | department       | salary | phonenumber |
+-----------+-------------+-----------------------------------------------+------------------+--------+-------------+
| Peter     | Aaron       | WATER RATE TAKER                              | WATER MGMNT      | 88968  | 5551962     |
| Rufi      | Bhola       | TRAFFIC SIGNAL REPAIRMAN                      | TRANSPORTN       | 95888  | 5551543     |
| Faruk     | Bota        | POLICE OFFICER                                | POLICE           | 80778  | 5551860     |
| Mary      | Bradley     | POLICE OFFICER                                | POLICE           | 80778  | 5551638     |
| Joseph    | Chu         | ASST TO THE ALDERMAN                          | CITY COUNCIL     | 70764  | 5551218     |
| Sam       | Cinq        | CHIEF CONTRACT EXPEDITER                      | GENERAL SERVICES | 84780  | 5551969     |
| Carol     | Cloud       | CIVIL ENGINEER IV                             | WATER MGMNT      | 104736 | 5551219     |
| Mackay    | Dalford     | POLICE OFFICER                                | POLICE           | 46206  | 5551516     |
| Drake     | Desmond     | GENERAL LABORER - DSS                         | STREETS & SAN    | 40560  | 5551551     |
| Sachen    | Dhoot       | ELECTRICAL MECHANIC                           | AVIATION         | 91520  | 5551500     |
| Albert    | Encino      | FIRE ENGINEER                                 | FIRE             | 90456  | 5551834     |
| Arthur    | Excalibur   | POLICE OFFICER                                | POLICE           | 86520  | 5551485     |
| Eric      | Freeman     | FOSTER GRANDPARENT                            | FAMILY & SUPPORT | 2756   | 5551706     |
| Fred      | Gobi        | CLERK III                                     | POLICE           | 43920  | 5551597     |
| Ivan      | Gorbachev   | INVESTIGATOR - IPRA II                        | IPRA             | 72468  | 5551978     |
| Theodore  | Henry       | POLICE OFFICER                                | POLICE           | 69684  | 5551904     |
| Cranston  | Horton      | POLICE OFFICER                                | POLICE           | 80778  | 5551924     |
| Bobi      | Ingerwen    | FIREFIGHTER (PER ARBITRATORS AWARD)-PARAMEDIC | FIRE             | 98244  | 5551294     |
| Franz     | Isenglass   | POLICE OFFICER                                | POLICE           | 80778  | 5551249     |
| Jenny     | Jakuti      | FIREFIGHTER/PARAMEDIC                         | FIRE             | 87720  | 5551656     |
| Karl      | Karloff     | ENGINEERING TECHNICIAN VI                     | WATER MGMNT      | 106104 | 5551911     |
| Kathryn   | Kretchmer   | FIREFIGHTER-EMT                               | FIRE             | 91764  | 5551875     |
| Loren     | Lakshmi     | LIEUTENANT                                    | FIRE             | 110370 | 5551082     |
| Dudley    | Less        | SENIOR ENVIRONMENTAL INSPECTOR                | HEALTH           | 76656  | 5551407     |
| Mahood    | Mahmut      | CROSSING GUARD                                | POLICE           | 16692  | 5551892     |
| Wanda     | Myers       | GENERAL LABORER - DSS                         | STREETS & SAN    | 40560  | 5551644     |
| Trey      | Mystique    | ELECTRICAL MECHANIC-AUTO-POLICE MTR MNT       | GENERAL SERVICES | 91520  | 5551376     |
| Manuel    | Nickels     | POLICE OFFICER                                | POLICE           | 86520  | 5551380     |
| Sheldon   | Overton     | PARAMEDIC                                     | FIRE             | 54114  | 5551551     |
| Lana      | Park        | MOTOR TRUCK DRIVER                            | STREETS & SAN    | 71781  | 5551530     |
| Franny    | Periodico   | LIBRARY ASSOCIATE - HOURLY                    | PUBLIC LIBRARY   | 24835  | 5551413     |
| Perry     | Polite      | CIVIL ENGINEER IV                             | WATER MGMNT      | 104736 | 5551891     |
| Mike      | Processer   | SENIOR PROGRAMMER/ANALYST                     | DoIT             | 104736 | 5551139     |
| Quincy    | Quintado    | ENGINEERING TECHNICIAN V                      | BUSINESS AFFAIRS | 96672  | 5551406     |
| Richard   | Ramstadt    | POLICE OFFICER                                | POLICE           | 46206  | 5551517     |
| Chandra   | Sambrosa    | SENIOR COMPANION                              | FAMILY & SUPPORT | 2756   | 5551896     |
| Amber     | Sikh        | SUPERVISING TRAFFIC CONTROL AIDE              | OEMC             | 55800  | 5551384     |
| Thomas    | Spelt       | SANITATION LABORER                            | STREETS & SAN    | 72384  | 5551807     |
| Boli      | Tiku        | POLICE OFFICER                                | POLICE           | 92316  | 5551921     |
| Clark     | Trent       | AMBULANCE COMMANDER                           | FIRE             | 123948 | 5551998     |
| Noreen    | Umbrella    | POOL MOTOR TRUCK DRIVER                       | STREETS & SAN    | 16151  | 5551173     |
| Conrad    | Valvoly     | FIREFIGHTER-EMT                               | FIRE             | 85680  | 5551908     |
| June      | Vendi       | SEWER BRICKLAYER                              | WATER MGMNT      | 88566  | 5551022     |
| Auicula   | Ventricular | TREE TRIMMER                                  | STREETS & SAN    | 74464  | 5551512     |
| Solomon   | Wally       | POLICE OFFICER                                | POLICE           | 89718  | 5551750     |
| Winifred  | Wonderman   | ELECTRICAL MECHANIC                           | AVIATION         | 91520  | 5551990     |
| Elvin     | Yahuli      | POLICE OFFICER                                | POLICE           | 86520  | 5551675     |
| Aloysius  | Zeke        | FIREFIGHTER-EMT                               | FIRE             | 95460  | 5551601     |
| Anderson  | Zephyr      | PERSONAL COMPUTER OPERATOR III                | HEALTH           | 66684  | 5551717     |
| Zelda     | Zion        | FIREFIGHTER-EMT                               | FIRE             | 99920  | 5553970     |
+-----------+-------------+-----------------------------------------------+------------------+--------+-------------+
Fetched 50 row(s) in 3.75s
</pre>

* Use Impala to set permissions for a group of users of `rs.employees`. First,  create a role named _demorole_. Next, add the role to the _demogroup_ you created before starting Impala. Grant the _select_ privilege to demorole for only the columns `firstname`, `lastname`, and `phonenumber` from the `rs.employees` table.

<pre>
[quickstart.cloudera:21000] > create role demorole;
Query: create role demorole

Fetched 0 row(s) in 0.40s
[quickstart.cloudera:21000] > grant role demorole to group demogroup;
Query: grant role demorole to group demogroup

[quickstart.cloudera:21000] > GRANT SELECT(firstname, lastname, phonenumber) ON TABLE rs.employees TO ROLE demorole;

Fetched 0 row(s) in 0.11s
</pre>

* Exit Impala.

* From the home directory, execute the following command. This is a trivial example class that counts the number of records in the table. Since the command specifies the <i>salary</i> column, to which the $USER (cloudera) does not have access, the command fails.

<pre>
[cloudera@quickstart ~]$ hadoop jar \
./recordservice-client-0.1/lib/recordservice-examples-0.1.jar \
com.cloudera.recordservice.examples.mapreduce.RecordCount \
"select lastname, salary from rs.employees" "/tmp/count_salary_output"

15/12/08 17:41:26 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032

. . . 

RecordServiceException: TRecordServiceException(code:INVALID_REQUEST, message:Could not plan request., detail:AuthorizationException: User 'cloudera' does not have privileges to execute 'SELECT' on: rs.employees
. . .
</pre>

* Now run the same command, but specify the `firstname`, `lastname`, and `phonenumber` columns.

<pre>
[cloudera@quickstart ~]$ hadoop jar \
./recordservice-client-0.1/lib/recordservice-examples-0.1.jar \
com.cloudera.recordservice.examples.mapreduce.RecordCount \
"select firstname, lastname, phonenumber from rs.employees" \
"/tmp/count_phonelist_output"

15/12/08 17:42:25 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
. . .
File Output Format Counters 
    Bytes Written=3
</pre>

* View the results using Hadoop.
<pre>
hadoop fs -cat /tmp/count_phonelist_output/part-r-00000
</pre>

The result returned is a row count of 50.

## Controlling Row Access

You can also define a view and assign access that restricts a user to certain rows in the data set.

Create a view that restricts the rows returned. For example, where <i>position</i> is not <code>POLICE OFFICER</code>.

<pre>
[quickstart.cloudera:21000] > create view no_police as select * from rs.employees where position <> "POLICE OFFICER";
Query: create view no_police as select * from rs.employees where position <> "POLICE OFFICER"
</pre>

Assign that view to <i>demorole</i>.

<pre>
[quickstart.cloudera:21000] > grant select on table rs.no_police to role demorole;
Query: grant select on table rs.no_police to role demorole
</pre>

Select all from the no_police view.

<pre>
[quickstart.cloudera:21000] > select * from rs.no_police;
Query: select * from rs.no_police
+-----------+-------------+-----------------------------------------------+------------------+--------+-------------+
| firstname | lastname    | position                                      | department       | salary | phonenumber |
+-----------+-------------+-----------------------------------------------+------------------+--------+-------------+
| Peter     | Aaron       | WATER RATE TAKER                              | WATER MGMNT      | 88968  | 5551962     |
| Rufi      | Bhola       | TRAFFIC SIGNAL REPAIRMAN                      | TRANSPORTN       | 95888  | 5551543     |
| Joseph    | Chu         | ASST TO THE ALDERMAN                          | CITY COUNCIL     | 70764  | 5551218     |
| Sam       | Cinq        | CHIEF CONTRACT EXPEDITER                      | GENERAL SERVICES | 84780  | 5551969     |
| Carol     | Cloud       | CIVIL ENGINEER IV                             | WATER MGMNT      | 104736 | 5551219     |
| Drake     | Desmond     | GENERAL LABORER - DSS                         | STREETS & SAN    | 40560  | 5551551     |
| Sachen    | Dhoot       | ELECTRICAL MECHANIC                           | AVIATION         | 91520  | 5551500     |
| Albert    | Encino      | FIRE ENGINEER                                 | FIRE             | 90456  | 5551834     |
| Eric      | Freeman     | FOSTER GRANDPARENT                            | FAMILY & SUPPORT | 2756   | 5551706     |
| Fred      | Gobi        | CLERK III                                     | POLICE           | 43920  | 5551597     |
| Ivan      | Gorbachev   | INVESTIGATOR - IPRA II                        | IPRA             | 72468  | 5551978     |
| Bobi      | Ingerwen    | FIREFIGHTER (PER ARBITRATORS AWARD)-PARAMEDIC | FIRE             | 98244  | 5551294     |
| Jenny     | Jakuti      | FIREFIGHTER/PARAMEDIC                         | FIRE             | 87720  | 5551656     |
| Karl      | Karloff     | ENGINEERING TECHNICIAN VI                     | WATER MGMNT      | 106104 | 5551911     |
| Kathryn   | Kretchmer   | FIREFIGHTER-EMT                               | FIRE             | 91764  | 5551875     |
| Loren     | Lakshmi     | LIEUTENANT                                    | FIRE             | 110370 | 5551082     |
| Dudley    | Less        | SENIOR ENVIRONMENTAL INSPECTOR                | HEALTH           | 76656  | 5551407     |
| Mahood    | Mahmut      | CROSSING GUARD                                | POLICE           | 16692  | 5551892     |
| Wanda     | Myers       | GENERAL LABORER - DSS                         | STREETS & SAN    | 40560  | 5551644     |
| Trey      | Mystique    | ELECTRICAL MECHANIC-AUTO-POLICE MTR MNT       | GENERAL SERVICES | 91520  | 5551376     |
| Sheldon   | Overton     | PARAMEDIC                                     | FIRE             | 54114  | 5551551     |
| Lana      | Park        | MOTOR TRUCK DRIVER                            | STREETS & SAN    | 71781  | 5551530     |
| Franny    | Periodico   | LIBRARY ASSOCIATE - HOURLY                    | PUBLIC LIBRARY   | 24835  | 5551413     |
| Perry     | Polite      | CIVIL ENGINEER IV                             | WATER MGMNT      | 104736 | 5551891     |
| Mike      | Processer   | SENIOR PROGRAMMER/ANALYST                     | DoIT             | 104736 | 5551139     |
| Quincy    | Quintado    | ENGINEERING TECHNICIAN V                      | BUSINESS AFFAIRS | 96672  | 5551406     |
| Chandra   | Sambrosa    | SENIOR COMPANION                              | FAMILY & SUPPORT | 2756   | 5551896     |
| Amber     | Sikh        | SUPERVISING TRAFFIC CONTROL AIDE              | OEMC             | 55800  | 5551384     |
| Thomas    | Spelt       | SANITATION LABORER                            | STREETS & SAN    | 72384  | 5551807     |
| Clark     | Trent       | AMBULANCE COMMANDER                           | FIRE             | 123948 | 5551998     |
| Noreen    | Umbrella    | POOL MOTOR TRUCK DRIVER                       | STREETS & SAN    | 16151  | 5551173     |
| Conrad    | Valvoly     | FIREFIGHTER-EMT                               | FIRE             | 85680  | 5551908     |
| June      | Vendi       | SEWER BRICKLAYER                              | WATER MGMNT      | 88566  | 5551022     |
| Auicula   | Ventricular | TREE TRIMMER                                  | STREETS & SAN    | 74464  | 5551512     |
| Winifred  | Wonderman   | ELECTRICAL MECHANIC                           | AVIATION         | 91520  | 5551990     |
| Aloysius  | Zeke        | FIREFIGHTER-EMT                               | FIRE             | 95460  | 5551601     |
| Anderson  | Zephyr      | PERSONAL COMPUTER OPERATOR III                | HEALTH           | 66684  | 5551717     |
| Zelda     | Zion        | FIREFIGHTER-EMT                               | FIRE             | 99920  | 5553970     |
+-----------+-------------+-----------------------------------------------+------------------+--------+-------------+
Fetched 38 row(s) in 0.74s
</pre>