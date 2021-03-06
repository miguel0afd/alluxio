---
layout: global
title: MapR-FS
nickname: MapR-FS
group: Under Stores
priority: 10
---

* Table of Contents
{:toc}

This guide describes how to configure Alluxio with [MapR-FS](https://www.mapr.com/products/mapr-fs)
as the under storage system.

## Compiling Alluxio with MapR Version

Alluxio must be [compiled]({{ '/en/contributor/Building-Alluxio-From-Source.html' | relativize_url }})
with the correct MapR distribution to integrate with MapR-FS. Here are some values of
`hadoop.version` for different MapR distributions:

<table class="table table-striped">
<tr><th>MapR Version</th><th>hadoop.version</th></tr>
<tr>
  <td>5.2</td>
  <td>2.7.0-mapr-1607</td>
</tr>
<tr>
  <td>5.1</td>
  <td>2.7.0-mapr-1602</td>
</tr>
<tr>
  <td>5.0</td>
  <td>2.7.0-mapr-1506</td>
</tr>
<tr>
  <td>4.1</td>
  <td>2.5.1-mapr-1503</td>
</tr>
<tr>
  <td>4.0.2</td>
  <td>2.5.1-mapr-1501</td>
</tr>
<tr>
  <td>4.0.1</td>
  <td>2.4.1-mapr-1408</td>
</tr>
</table>

## Configuring Alluxio for MapR-FS

Once you have compiled Alluxio with the appropriate `hadoop.version` for your MapR distribution, you
may have to configure Alluxio to recognize the MapR-FS scheme and URIs. Alluxio uses the HDFS client
to access MapR-FS, and by default is already configured to do so. However, if the configuration has
been changed, you can enable the HDFS client to access MapR-FS URIs by adding the URI prefix
`maprfs:///` to the configuration variable `alluxio.underfs.hdfs.prefixes` like below:

```properties
alluxio.underfs.hdfs.prefixes=hdfs://,maprfs:///
```

This configuration parameter should be set for all the Alluxio servers (masters, workers). Please
read how to [configure Alluxio]({{ '/en/basic/Configuration-Settings.html' | relativize_url }}). For
Alluxio processes, this parameter can be set in the property file `alluxio-site.properties`. For
more information, please read about
[configuration of Alluxio with property files]({{ '/en/basic/Configuration-Settings.html' | relativize_url }}#configuration-sources).

## Configuring Alluxio to use MapR-FS as Under Storage

There are various ways to configure Alluxio to use MapR-FS as under storage. If you want to
mount MapR-FS to the root of Alluxio, add the following to `conf/alluxio-site.properties`:

```properties
alluxio.master.mount.table.root.ufs=maprfs:///<path in MapR-FS>/
```

You can also mount a directory in MapR-FS to a directory in the Alluxio namespace.

```console
$ ${ALLUXIO_HOME}/bin/alluxio fs mount /<path in Alluxio>/ maprfs:///<path in MapR-FS>/
```

## Running Alluxio Locally with MapR-FS

Start up Alluxio locally to see that everything works.

```console
$ ./bin/alluxio format
$ ./bin/alluxio-start.sh local
```

This should start one Alluxio master and one Alluxio worker locally. You can see the master UI at
[http://localhost:19999](http://localhost:19999).

Run a simple example program:

```console
$ ./bin/alluxio runTests
```

Visit MapR-FS web UI to verify the files and directories created by
Alluxio exist. For this test, you should see files named like:
`/default_tests_files/BASIC_CACHE_CACHE_THROUGH`

Stop Alluxio by running:

```console
$ ./bin/alluxio-stop.sh local
```
