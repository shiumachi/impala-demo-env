# impala-demo-env
Install scripts of Impala cluster for BI Demo
This is for Demo purposes only. Don't use for production.

- These scripts install and deploy the following environment automatically.
  - Cloudera Enterprise 5.14.0 (Trial) including Impala

- I only tested on the AWS ap-northeast-1 (Tokyo) region

## Requirement

- Cloudera Director 2.7
    - The simplest way to install Cloudera Director on Mac is here -> https://github.com/chezou/homebrew-cloudera
- AWS Environment
    - Setting up a VPC for Cloudera Director
    - Creating a security group for Cloudera Director
    - See https://www.cloudera.com/documentation/director/latest/topics/director_aws_setup_client.html

## How to use

1. You need to install Cloudera Director Server/Client on your localhost and accessible by localhost:7189

2. Copy `your-aws-info.conf.template` and create your own `your-aws-info.conf` with
- AWS_REGION
- AWS_SUBNET_ID
- AWS_SECURITY_GROUP
- KEY_PAIR
- AWS_AMI

3. Set `AWS_ACCESS_KEY_ID/AWS_SECRET_ACCESS_KEY` and run `cloudera-director bootstrap-remote` command. It takes about 30 minutes.

```
$ export AWS_ACCESS_KEY_ID=<your-aws-access-key>
$ export AWS_SECRET_ACCESS_KEY=<your-aws-secret>
$ cloudera-director bootstrap-remote impala-demo-cluster.conf --lp.remote.username=admin --lp.remote.password=admin --lp.remote.hostAndPort=localhost:7189
```

4. `./get_cluster_ip.sh <cluster.conf>` provides the connection information to the environment. See also the following Example section.

5. To terminate this environment, run `cloudera-director terminate-remote` command.

```
$ cloudera-director terminate-remote impala-demo-cluster.conf --lp.remote.username=admin --lp.remote.password=admin --lp.remote.hostAndPort=localhost:7189
```

## Example
```
$ export AWS_ACCESS_KEY_ID=<your-aws-access-key>
$ export AWS_SECRET_ACCESS_KEY=<your-aws-secret>

$ cloudera-director bootstrap-remote impala-demo-cluster.conf --lp.remote.username=admin --lp.remote.password=admin --lp.remote.hostAndPort=localhost:7189
Process logs can be found at /usr/local/Cellar/cloudera-director-client/2.7.0/libexec/logs/application.log
Plugins will be loaded from /usr/local/Cellar/cloudera-director-client/2.7.0/libexec/plugins
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=256M; support was removed in 8.0
Cloudera Director 2.7.0 initializing ...
Connecting to http://localhost:7189
Current user roles: [ROLE_READONLY, ROLE_ADMIN]
Found warnings in cluster configuration:
* WarningInfo{code=UNKNOWN_SERVICE_TYPE, properties={serviceType=KUDU}}
Configuration file passes all validation checks.
Creating a new environment...
Creating external database servers if configured...
Creating a new Cloudera Manager...
Creating a new CDH cluster...
* Requesting an instance for Cloudera Manager ......... done
* Installing screen package (1/1) .... done
* Running bootstrap script #1 (crc32: 5e2d4d51) ....... done
* Waiting until 2018-01-26T21:25:35.124+09:00 for SSH access to [10.0.0.225, ip-10-0-0-225.ap-northeast-1.compute.internal, 52.193.225.32, ec2-52-193-225-32.ap-northeast-1.compute.amazonaws.com], default port 22 ... done
* Inspecting capabilities of 10.0.0.225 .... done
* Normalizing 8d2f8894-0355-4249-9ad7-acaaff0287d4 ... done
* Installing ntp package (1/5) .... done
* Installing curl package (2/5) .... done
* Installing nscd package (3/5) .... done
* Installing rng-tools package (4/5) .... done
* Installing gdisk package (5/5) ...................................... done
* Resizing instance root partition ........ done
* Mounting all instance disk drives ........ done
* Installing repositories for Cloudera Manager ....... done
* Installing yum-utils package (1/3) .... done
* Installing cloudera-manager-daemons package (2/3) .... done
* Installing cloudera-manager-server package (3/3) .... done
* Setting up embedded PostgreSQL database for Cloudera Manager ... done
* Installing cloudera-manager-server-db-2 package (1/1) .... done
* Starting embedded PostgreSQL database ..... done
* Starting Cloudera Manager server ... done
* Waiting for Cloudera Manager server to start ..... done
* Setting Cloudera Manager License ... done
* Enabling Enterprise Trial ... done
* Configuring Cloudera Manager ... done
* Deploying Cloudera Manager agent ..... done
* Waiting for Cloudera Manager to deploy agent on 10.0.0.225 ..... done
* Setting up Cloudera Management Services .......... done
* Backing up Cloudera Manager Server configuration ...... done
* Inspecting capabilities of 10.0.0.225 ... done
* Running Deployment post create scripts ... done
* Starting ... done
* Done ...
Cloudera Manager ready.
* Waiting for Cloudera Manager installation to complete ...... done
* Installing Cloudera Manager agents on all instances in parallel (20 at a time) .................................................................................................................................................................................................................................................................. done
* Creating CDH5 cluster using the new instances .... done
* Creating cluster: impala-demo-cluster ....... done
* Downloading parcels: CDH-5.14.0-1.cdh5.14.0.p0.24 ..................................................................................................................................... done
* Raising rate limits for parcel distribution to 256000KB/s with 5 concurrent uploads ... done
* Distributing parcels: CDH-5.14.0-1.cdh5.14.0.p0.24 .............................................................................................................................................................................................................................................................................................................................................................................................................. done
* Switching parcel distribution rate limits back to defaults: 51200KB/s with 25 concurrent uploads .... done
* Activating parcels: CDH-5.14.0-1.cdh5.14.0.p0.24 ........................................ done
* Creating cluster services ..... done
* Assigning roles to instances ...... done
* Automatically configuring services and roles ..... done
* Applying custom configurations of services .... done
* Configuring HIVE database .... done
* Configuring HUE database ... done
* Configuring OOZIE database ... done
* Creating role config groups, applying custom configurations and moving roles to created role config groups ... done
* Configuring role config groups of type DATANODE ... done
* Configuring role config groups of type NODEMANAGER ... done
* Configuring role config groups of type IMPALAD ... done
* Configuring role config groups of type KUDU_TSERVER ... done
* Renaming role config group from Tablet Server Default Group to KUDU_TSERVER worker Group z2ElXWVn ... done
* Configuring role config groups of type NAMENODE ... done
* Configuring role config groups of type SECONDARYNAMENODE ... done
* Renaming role config group from SecondaryNameNode Default Group to SECONDARYNAMENODE master Group MuzK24xQ ... done
* Renaming role config group from ResourceManager Default Group to RESOURCEMANAGER master Group ieRwAWkS ... done
* Renaming role config group from Master Default Group to KUDU_MASTER master Group SHaZ6inA ..... done
* Preparing cluster impala-demo-cluster ... done
* Creating Hive Metastore Database ............................... done
* Waiting for firstRun on cluster impala-demo-cluster .......................................................................................................................................................................................................................................... done
* Running instance post create scripts in parallel (20 at a time) ..... done
* Running 1 cluster post creation script(s) ................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................... done
* Adjusting health thresholds to take into account optional instances. ............. done
* Done ...
Cluster ready.


$ ./get_cluster_ip.sh impala-demo-cluster.conf
[Cloudera Manager]
Public IP: 13.113.180.45    Private IP: 10.0.0.155
CM URL: http://10.0.0.155:7180

[Nodes]
    worker    Public IP:     13.114.68.48    Private IP:        10.0.0.17
    worker    Public IP:    54.238.154.87    Private IP:       10.0.0.235
    master    Public IP:    13.115.129.96    Private IP:       10.0.0.247
    worker    Public IP:     13.114.58.95    Private IP:        10.0.0.32

[SSH Tunnel for Dynamic Port Forwarding]
ssh -i your-aws-sshkey.pem -D 8157 -q centos@13.113.180.45

SSH Tunnel to Impalad
ssh -i your-aws-sshkey.pem -L:21050:10.0.0.17:21050 -q centos@13.113.180.45
```

## Connecting to Cloudera Manager

```
ssh -i your-aws-sshkey.pem -D 8157 -q centos@13.113.180.45
```

This ssh connection creates a SOCKS Proxy to your VPC.
You can access to Cloudear Manager, http://10.0.0.155:7180 - (IP 10.0.0.155 changes every time), from your web browser via SSH SOCKS Proxy (See https://www.cloudera.com/documentation/director/latest/topics/director_security_socks.html).

## Connecting to Impala from BI Tools

```
ssh -i your-aws-sshkey.pem -L:21050:10.0.0.17:21050 -q centos@13.113.180.45
```

This ssh connection creates a tunnel to Impala.
You can access to Impala using 127.0.0.1:21050 (local port forwarding) from your favorite BI tools.


## Users and Authentication

This script creates a non-secure cluster.
No users are created and no need to authenticate.

## EC2 Instances

If you change instance type, you can modify first section of `impala-demo-cluster.conf`.

```
INSTANCE_TYPE_CM:        t2.xlarge    #vCPU 4, RAM 16G
INSTANCE_TYPE_MASTER:    t2.large     #vCPU 2, RAM 8G
INSTANCE_TYPE_WORKER:    r3.xlarge    #vCPU 4, RAM 30.5G, SSD 80Gx1

WORKER_NODE_NUM:         3            #Number of Worker Nodes
```
