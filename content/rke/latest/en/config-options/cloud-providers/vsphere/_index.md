---
title: vSphere Cloud Provider
weight: 254
---

The [vSphere Cloud Provider](https://vmware.github.io/vsphere-storage-for-kubernetes/documentation/) interacts with VMware infrastructure (vCenter or standalone ESXi server) to provision and manage storage for persistent volumes in a Kubernetes cluster.

When provisioning Kubernetes using RKE CLI or using [RKE clusters]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/) in Rancher, the vSphere Cloud Provider can be enabled by configuring the `cloud_provider` directive in the cluster YAML file.

This section covers the following topics:

- [Prerequisites](#prerequisites)
- [Enabling the vSphere provider](#enabling-the-vsphere-provider)
- [vSphere configuration reference](#vsphere-configuration-reference)
- [Example vSphere configuration](#example-vsphere-configuration)
- [Enabling disk UUIDs for vSphere VMs](#enabling-disk-uuids-for-vsphere-vms)
- [Troubleshooting](#troubleshooting)
- [Related links](#related-links)

# Prerequisites

- You'll need to have credentials of a vCenter/ESXi user account with privileges allowing the cloud provider to interact with the vSphere infrastructure to provision storage. Refer to [this document](https://vmware.github.io/vsphere-storage-for-kubernetes/documentation/vcp-roles.html) to create and assign a role with the required permissions in vCenter.
- VMware Tools must be running in the Guest OS for all nodes in the cluster.
- All nodes must be configured with disk UUIDs. This is required so that attached VMDKs present a consistent UUID to the VM, allowing the disk to be mounted properly. See the section on [enabling disk UUIDs.]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/enabling-uuid)

# Enabling the vSphere Provider

The vSphere provider can be enabled in two ways: with the RKE CLI, and with Rancher.

### Enabling the vSphere Provider with the RKE CLI

To enable the vSphere Cloud Provider in the cluster, you must add the top-level `cloud_provider` directive to the cluster configuration file, set the `name` property to `vsphere` and add the `vsphereCloudProvider` directive containing the configuration matching your infrastructure. See the [configuration reference]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/config-reference) for the gory details.

### Enabling the vSphere Provider with Rancher

If you are using Rancher, refer to the [Rancher documentation]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/#enabling-the-vsphere-provider-in-rancher) for instructions on enabling the vSphere provider in Rancher.

# vSphere Configuration Reference

For details on vSphere configuration in RKE, refer to the [configuration reference.]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/config-reference)

# Example vSphere Configuration

Refer to [this section]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/config-example) for an example of how to configure vSphere with RKE.

# Enabling Disk UUIDs for vSphere VMs

Disk UUIDs can be enabled for vSphere VMs using the vSphere console, the GOVC CLI tool, or a Rancher node template. For details, refer to the section on [enabling disk UUIDs.]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/enabling-uuid)

# Troubleshooting

For guidance on troubleshooting a cluster with the vSphere cloud provider enabled, refer to the [troubleshooting section.]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/troubleshooting)

# Related Links

- [vSphere Storage for Kubernetes](https://vmware.github.io/vsphere-storage-for-kubernetes/documentation/)
- [Kubernetes Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [Creating vSphere clusters in Rancher]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere)
- [Provisioning storage in vSphere using Rancher]({{<baseurl>}}/rancher/v2.x/en/cluster-admin/volumes-and-storage/examples/vsphere)
