---
layout: article
title: 'Getting Started with RecordService Beta'
share: false
---
You can try RecordService beta in multiple ways. First, download the server via the virtual machine (VM) or by downloading the RecordServiceClient parcel or package.

RecordService beta must be installed only on a test cluster, because it is beta software.

<div class="tiles">
<div class="tile">
<h3 align="center">Using the Beta VM</h3>
<p>
The RecordService beta VM provides a single-node Hadoop cluster, including Impala, MapReduce/YARN, Spark, Sentry, and RecordService.  Using the VM is the easiest way to see RecordService in action, and it does not require that you  use an existing Hadoop cluster.
</p><p>
The VM itself does not contain RecordService Client and examples: those should be run on a separate machine.
</p>
<p>
See <a href="vm.html">Download and Install the RecordService VM</a>
</p>
</div>
<div class="tile">
<h3 align="center">Using Your Own Cluster</h3>
<p>
You can install and run RecordService on your own cluster built on CDH 5.4 or later.
</p>
<p>
See <a href="installOnCluster.html">Download and Install RecordService on Your CDH Cluster</a>.
</p>
</div>
<div class="tile">
<h3 align="center">Configuring RecordService</h3>
<p>
While you should not need to change default settings, there are several properties you can modify to customize your RecordService instance.
</p>
<p>
See <a href="rsConfig.html">Configuring RecordService</a>.
</p>
</div>
<div class="tile">
<h3 align="center">Running Examples</h3>
<p>
To integrate the RecordService into your existing MapReduce and Spark jobs, use the RecordService Client libraries. The client repository contains many example applications you can run right away.
</p>
<p>
See <a href="examples.html">RecordService Examples</a>.
</p>
</div>
</div>
