---
title: Restoring Rancher Installed on a K3s Kubernetes Cluster
weight: 1
---

When Rancher is installed on a high-availability Kubernetes cluster, we recommend using an external database to store the cluster data.

The database administrator will need to back up the external database, or restore it from a snapshot or dump.

We recommend configuring the database to take recurring snapshots.

### Creating Snapshots and Restoring Databases from Snapshots

For details on taking database snapshots and restoring your database from them, refer to the [official MySQL documentation.](https://dev.mysql.com/doc/refman/8.0/en/replication-snapshot-method.html)