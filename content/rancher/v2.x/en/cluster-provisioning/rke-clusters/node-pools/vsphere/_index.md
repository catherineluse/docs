---
title: Creating a vSphere Cluster
shortTitle: vSphere
weight: 2225
aliases:
  - /rancher/v2.x/en/tasks/clusters/creating-a-cluster/create-cluster-vsphere/
---

By using Rancher with vSphere, you can bring cloud operations on-premises.

Rancher can provision nodes in vSphere and install Kubernetes on them. When creating a Kubernetes cluster in vSphere, Rancher first provisions the specified number of virtual machines by communicating with the vCenter API. Then it installs Kubernetes on top of them.

A vSphere cluster may consist of multiple groups of VMs with distinct properties, such as the amount of memory or the number of vCPUs. This grouping allows for fine-grained control over the sizing of nodes for the data, control, and worker plane respectively.

# New Features in Rancher v2.3.x

By provisioning nodes with node templates in Rancher, you can take advantage of the following new features:

### Self-healing Node Pools

_Available as of v2.3.0_

One of the biggest advantages of provisioning vSphere nodes with Rancher is that it allows you to take advantage of Rancher's self-healing node pools, also called the [node auto-replace feature,]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/#node-auto-replace) in your on-premises clusters. When Rancher provisions nodes from a node template, Rancher can automatically replace unreachable nodes.

### Dynamically Populated Options for Instances and Scheduling

_Available as of v2.3.3_

Node templates for vSphere have been updated so that when you create a node template with your vSphere credentials, the template is automatically populated with the same options for provisioning VMs that you have access to in the vSphere console.

For the fields to be populated, your setup needs to fulfill the [prerequisites.]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/provisioning-vsphere-clusters/#prerequisites)

### More Supported Operating Systems

In Rancher v2.3.3+, you can provision VMs with any operating system that supports cloud init.

In Rancher prior to v2.3.3, the vSphere node driver included in Rancher only supported the provisioning of VMs with [RancherOS]({{<baseurl>}}/os/v1.x/en/) as the guest operating system.