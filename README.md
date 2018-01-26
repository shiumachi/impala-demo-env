# impala-demo-env
Install scripts of Impala cluster for BI Demo
This is for Demo purposes only. Don't use for production.

- These scripts install and deploy the following environment automatically.
  - Cloudera Enterprise 5.13.1 (Trial) including Impala

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
* Running bootstrap script #1 (crc32: 5e2d4d51) ........ done
* Waiting until 2018-01-26T22:39:57.538+09:00 for SSH access to [10.0.0.49, ip-10-0-0-49.ap-northeast-1.compute.internal, 46.51.229.198, ec2-46-51-229-198.ap-northeast-1.compute.amazonaws.com], default port 22 ... done
* Inspecting capabilities of 10.0.0.49 .... done
* Normalizing aed9ab13-0960-4188-bf92-43fb25c04ea6 ... done
* Installing ntp package (1/5) .... done
* Installing curl package (2/5) .... done
* Installing nscd package (3/5) .... done
* Installing rng-tools package (4/5) .... done
* Installing gdisk package (5/5) ..................................... done
* Resizing instance root partition ....... done
* Mounting all instance disk drives ......... done
* Installing repositories for Cloudera Manager ...... done
* Installing yum-utils package (1/3) .... done
* Installing cloudera-manager-daemons package (2/3) .... done
* Installing cloudera-manager-server package (3/3) .... done
* Installing cloudera-manager-server-db-2 package (1/1) .... done
* Starting embedded PostgreSQL database ..... done
* Starting Cloudera Manager server ... done
* Waiting for Cloudera Manager server to start .... done
* Enabling Enterprise Trial ... done
* Configuring Cloudera Manager ... done
* Deploying Cloudera Manager agent ..... done
* Waiting for Cloudera Manager to deploy agent on 10.0.0.49 ..... done
* Setting up Cloudera Management Services ........ done
* Backing up Cloudera Manager Server configuration ...... done
* Inspecting capabilities of 10.0.0.49 ... done
* Running Deployment post create scripts ... done
* Done ...
Cloudera Manager ready.
* Waiting for Cloudera Manager installation to complete ............. done
* Installing Cloudera Manager agents on all instances in parallel (20 at a time) ............................................................................................................................................................................................................................................................................................. done
* Creating CDH5 cluster using the new instances .... done
* Creating cluster: impala-demo-cluster ....... done
* Downloading parcels: CDH-5.13.1-1.cdh5.13.1.p0.2 ........................................................................... done
* Distributing parcels: CDH-5.13.1-1.cdh5.13.1.p0.2 ...................................................................................................................................................................... done
* Switching parcel distribution rate limits back to defaults: 51200KB/s with 25 concurrent uploads .... done
* Activating parcels: CDH-5.13.1-1.cdh5.13.1.p0.2 ........................................................................................ done
* Creating cluster services .... done
* Assigning roles to instances ...... done
* Automatically configuring services and roles ..... done
* Applying custom configurations of services .... done
* Configuring HIVE database .... done
* Configuring HUE database ... done
* Configuring OOZIE database ... done
* Creating role config groups, applying custom configurations and moving roles to created role config groups ... done
* Renaming role config group from DataNode Default Group to DATANODE worker Group pAOP11tL ... done
* Renaming role config group from NodeManager Default Group to NODEMANAGER worker Group zTVx0bxB ... done
* Renaming role config group from Impala Daemon Default Group to IMPALAD worker Group eHIaSx6W ... done
* Renaming role config group from Tablet Server Default Group to KUDU_TSERVER worker Group yQ2FEbxY ... done
* Renaming role config group from NameNode Default Group to NAMENODE master Group pHgZeLlH ... done
* Renaming role config group from SecondaryNameNode Default Group to SECONDARYNAMENODE master Group cl4Pyo1W ... done
* Renaming role config group from ResourceManager Default Group to RESOURCEMANAGER master Group nROwF1qp ... done
* Renaming role config group from Master Default Group to KUDU_MASTER master Group Rais5Phu .... done
* Preparing cluster impala-demo-cluster .... done
* Creating Hive Metastore Database .............................. done
* Waiting for firstRun on cluster impala-demo-cluster .......................................................................................................................................................................................................................................... done
* Running instance post create scripts in parallel (20 at a time) ..... done
* Running 1 cluster post creation script(s) ......................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................... done
* Shrinking away any more failed instances before continuing ... done
* Adjusting health thresholds to take into account optional instances. .......................... done
* Done ...
Cluster ready.


$ ./get_cluster_ip.sh impala-demo-cluster.conf
[Cloudera Manager]
Public IP: 46.51.229.198    Private IP: 10.0.0.49
CM URL: http://10.0.0.49:7180

[Nodes]
    master    Public IP:    13.231.102.18    Private IP:       10.0.0.135
    worker    Public IP:     54.65.217.87    Private IP:       10.0.0.150
    worker    Public IP:   52.193.251.235    Private IP:       10.0.0.224
    worker    Public IP:    175.41.232.28    Private IP:        10.0.0.73

[SSH Tunnel for Cloudera Manager and Impala]
ssh -i your-aws-sshkey.pem -L:21050:10.0.0.150:21050 -D 8157 -q centos@46.51.229.198
```

## Creating SSH tunnel for Cloudera Manager and Impala 

```
ssh -i your-aws-sshkey.pem -L:21050:10.0.0.150:21050 -D 8157 -q centos@46.51.229.198
```

This ssh connection creates a SOCKS Proxy to your VPC.
You can access to Cloudear Manager, http://10.0.0.49:7180 - (IP 10.0.0.49 changes every time), from your web browser via SSH SOCKS Proxy (See https://www.cloudera.com/documentation/director/latest/topics/director_security_socks.html).

This ssh connection also creates a tunnel to Impala.
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
