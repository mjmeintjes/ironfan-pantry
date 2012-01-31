# flume chef cookbook

Flume: reliable decoupled shipment of logs and data.

## Overview

Cookbook to install flume on a cluster.

Use flume::master to set up a master node. Use flume::node to set up a
physical node. Currently only one physical node per machines. 

Configure logical nodes with the logical_node resource - see the test_flow.rb 
recipe for an example. This is still somewhat experimental, and some features
will not work as well as they should until chef version 0.9.14 and others until
the next release of flume.

Coming soon flume::xxx_plugin.

#### Notes

This recipe relies on cluster_discovery_services to determine which nodes 
across the cluster act as flume masters, and which nodes provide zookeeper
servers.

## Attributes

* `[:flume][:aws_access_key]`         - AWS access key used for writing to s3 buckets
* `[:flume][:aws_secret_key]`         - AWS secret key used for writing to s3 buckets
* `[:flume][:cluster_name]`           -  (default: "cluster_name")
  - The name of the cluster to participate with (masters and zookeepers...)
* `[:flume][:plugins]`                - Hash for plugin configuration
  - If you have a particular plugin to configure, you can also configure the classpath and the classes to include in the configuration file with attributes in the following forms:
    node[:flume][:plugin][{plugin_name}][:classes]
    node[:flume][:plugin][{plugin_name}][:classpath]
    node[:flume][:plugin][{plugin_name}][:java_opts]
* `[:flume][:classes]`                - 
  - classes to include as plugins
* `[:flume][:classpath]`              - list of directories and jars to add to the FLUME_CLASSPATH
* `[:flume][:java_opts]`              - list of command line parameters to add to the jvm
* `[:flume][:collector]`              - Format of node's logs
  - output_format -- Controls what format the node writes logs (using collectorSink):
     * avro - Avro Native file format. Default currently is uncompressed.
     * avrodata - Binary encoded data written in the avro binary format.
     * avrojson - JSON encoded data generated by avro.
     * default - a debugging format.
     * json - JSON encoded data.
     * log4j - a log4j pattern similar to that used by CDH output pattern.
     * raw - Event body only. This is most similar to copying a file but does not preserve any uniqifying metadata like host/timestamp/nanos.
     * syslog - a syslog like text output format.
    
    codec -- Controls what kind of compression the collector will use when writing a file.
    whether or not collected logs are gzipped before writing
    them to their final resting place (using collectorSink)
     * GZipCodec
     * BZip2Codec
    
* `[:flume][:data_dir]`               - Directory for local in-transit files (default: "/data/db/flume")
* `[:flume][:home_dir]`               -  (default: "/usr/lib/flume")
* `[:flume][:conf_dir]`               -  (default: "/etc/flume/conf")
  - default[:flume][:tmp_dir]               = '/mnt/flume/tmp'
* `[:flume][:log_dir]`                -  (default: "/var/log/flume")
* `[:flume][:pid_dir]`                -  (default: "/var/run/flume")
* `[:flume][:master][:external_zookeeper]` - false to use flume's zookeeper. True to attach to an external zookeeper. (default: "false")
  - By default, flume installs its own zookeeper instance.  With :external_zookeeper to "true", the recipe will work out which machines are in the zookeeper quorum based on cluster membership; modify node[:discovers][:zookeeper_server] to have it use an external cluster
* `[:flume][:master][:zookeeper_port]` - port to talk to zookeeper on (for external zookeeper) (default: "2181")
* `[:flume][:master][:run_state]`     -  (default: "stop")
* `[:flume][:node][:run_state]`       -  (default: "start")

## Recipes 

* `config`                   - Finalizes the config, writes out the config files
* `default`                  - Base configuration for flume
* `hbase_sink_plugin`        - Hbase Sink Plugin
* `jruby_plugin`             - Jruby Plugin
* `master`                   - Configures Flume Master, installs and starts service
* `node`                     - Configures Flume Node, installs and starts service
* `test_flow`                - Test Flow

## Integration

Supports platforms: debian and ubuntu

Cookbook dependencies:
* java
* apt
* runit
* volumes
* metachef
* hadoop_cluster


## License and Author

Author::                Chris Howe - Infochimps, Inc (<coders@infochimps.com>)
Copyright::             2011, Chris Howe - Infochimps, Inc

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

> readme generated by [cluster_chef](http://github.com/infochimps/cluster_chef)'s cookbook_munger
