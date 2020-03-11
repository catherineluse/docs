---
title: K3s Kubernetes Cluster Restoration
shortTitle: K3s Restore
weight: 1
---

When Rancher is installed on a high-availability Kubernetes cluster, we recommend using an external database to store the cluster data.

The database administrator will need to back up the external database, or restore it from a snapshot or dump.

We recommend configuring the database to take recurring snapshots.

### Creating Snapshots and Restoring Databases from Snapshots

If you used Amazon RDS (Relational Database Service) to store your K3s cluster data, refer to the official Amazon documentation about [Backing Up and Restoring Amazon RDS DB Instances.](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html)

Otherwise, refer to the [official upstream MySQL documentation](https://dev.mysql.com/doc/refman/8.0/en/replication-snapshot-method.html) or other upstream documentation for the setup that you used for the cluster datastore.