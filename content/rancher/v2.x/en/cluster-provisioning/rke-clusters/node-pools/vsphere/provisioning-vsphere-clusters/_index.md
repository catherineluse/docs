---
title: Provisioning Kubernetes Clusters in vSphere
weight: 1
---

This section covers the following topics:

# Prerequisites

-[Preparation in vSphere](#preparation-in-vsphere)
 - [vSphere API permissions](#vsphere-api-permissions)
 - [Network permissions](#network-permissions)
 - [Creating vSphere credentials to create clusters](#creating-vsphere-credentials-to-create-clusters)

# Creating Clusters in vSphere with Rancher
- [Enabling the vSphere provider](#enabling-the-vsphere-provider)
- [Enabling Disk UUIDs](#enabling-disk-uuids)
- [Creating a Node Template Using vSphere Credentials](#creating-a-node-template-using-vsphere-credentials)
  - [Configuring an Operating System for Nodes](#configuring-an-operating-system-for-nodes)
- [Creating a vSphere Kubernetes cluster with a node template](#creating-a-vsphere-kubernetes-cluster-with-the-node-template)
- [Provisioning Storage](#provisioning-storage)
- [Node template configuration reference](#node-template-configuration-reference)

# Preparation in vSphere

The following table lists the permissions required for the vSphere user account configured in the node templates:

| Privilege Group       | Operations  |
|:----------------------|:-----------------------------------------------------------------------|
| Datastore             | AllocateSpace </br> Browse </br> FileManagement (Low level file operations) </br> UpdateVirtualMachineFiles </br> UpdateVirtualMachineMetadata |
| Network               | Assign |
| Resource              | AssignVMToPool |
| Virtual Machine       | Config (All) </br> GuestOperations (All) </br> Interact (All) </br> Inventory (All) </br> Provisioning (All) |

### vSphere API Permissions

Before proceeding to create a cluster, you must ensure that you have a vSphere user with sufficient permissions.

If you are planning to make use of vSphere volumes for persistent storage in the cluster, there are additional requirements. Rancher uses RKE (Rancher Kubernetes Engine) to provision Kubernetes clusters. Refer to the [RKE documentation]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/) for details.

### Network Permissions

You must ensure that the hosts running Rancher servers are able to establish network connections to the following network endpoints:

- vCenter server (usually port 443/TCP)
- Every ESXi host that is part of the datacenter to be used to provision virtual machines for your clusters (port 443/TCP).

### Creating vSphere Credentials to Create Clusters

Refer to this [how-to guide]({{<baseurl>}}/rancher/v2.x/en/cluster/rke-clusters/node-pools/vsphere/creating-credentials) for instructions on how to create a user in vSphere with the required permissions. These steps result in a username and password that you will need to provide to Rancher as a cloud credential, which allows Rancher to provision resources in vSphere.

# Enabling the vSphere Provider

Before provisioning vSphere clusters in Rancher, you need to modify the cluster YAML file to enable the vSphere provider.

This requirement applies to both pre-created [custom nodes]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/custom-nodes/) and for nodes created in Rancher using the vSphere node driver.

Refer to [these steps]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/enable-provider) for details.

> **Note:** In order to provision nodes with RKE, all nodes must be configured with disk UUIDs.
>
> - As of Rancher v2.0.4, disk UUIDs are enabled in vSphere node templates by default.
> - If you are using Rancher prior to v2.0.4, refer to these [instructions]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/#enabling-disk-uuids-with-a-node-template) for details on how to enable a UUID with a Rancher node template.

# Creating vSphere Clusters in Rancher

Create vSphere clusters in Rancher by following these steps:

1. [Create a vSphere node template using vSphere credentials](#1-create-a-vsphere-node-template-using-vsphere-credentials)
2. [Create a vSphere Kubernetes Cluster with the node template](#2-create-a-vsphere-kubernetes-cluster-with-the-node-template)

### 1. Create a Node Template Using vSphere Credentials

To create a cluster, you need to create at least one vSphere [node template]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/#node-templates)  that specifies how VMs are created in vSphere.

After you create a node template, it is saved, and you can re-use it whenever you create additional vSphere clusters.

To create a node template,

1. Log in with an admin account to the Rancher UI.

1. From the user settings menu, select **Node Templates**.

1. Click **Add Template** and then click on the **vSphere** icon.

1. Under [Account Access](#account-access) enter the vCenter FQDN or IP address and the credentials for the vSphere user account (see [Prerequisites](#prerequisites)).

	{{< step_create-cloud-credential >}}

1. Under **Scheduling**, enter the name/path of the **Data Center** to create the VMs in, the name of the **VM Network** to attach to, and the name/path of the **Datastore** to store the disks in.

    {{< img "/img/rancher/vsphere-node-template-2.png" "image">}}

1. Under [Instance Options](#instance-options), configure the number of vCPUs, memory, and disk size for the VMs created by this template.

Prior to v2.3.3, only RancherOS VMs are supported.

In Rancher v2.3.3+, you can use the **Creation method** field to configure the method for setting up an operating system on the node. You can use any operating system that supports cloud init. For details, refer to the section on [configuring an operating system for nodes.](#configurin-an-operating-system-for-nodes)

1. **Optional:** Enter the URL pointing to a cloud-config file in the [Cloud Init](#instance-options) field.

1. Ensure that the [OS ISO URL](#instance-options) contains the URL of a VMware ISO release. For example, the URL for RancherOS is (`rancheros-vmware.iso`).

    {{< img "/img/rancher/vsphere-node-template-1.png" "image">}}

1. **Optional:**

  - Provide a set of [Configuration Parameters](#instance-options) for the VMs.
  - Assign labels to the VMs that can be used as a base for scheduling rules in the cluster.
  - Customize the configuration of the Docker daemon on the VMs that will be created.
  - As of v2.3.3., you can optionally add vSphere tags and custom attributes. Tags allow you to attach metadata to objects in the vSphere inventory to make it easier to sort and search for these objects.
  
  > **Note:** Custom attributes are a legacy feature that will eventually be removed from vSphere. These attributes allow you to attach metadata to objects in the vSphere inventory to make it easier to sort and search for these objects.

1. Assign a descriptive **Name** for this template and click **Create**.

### 2. Create a vSphere Kubernetes Cluster with the Node Template

After you've created a template, you can use it stand up the vSphere cluster itself.

1. From the **Global** view, click **Add Cluster**.

2. Choose **vSphere**.

3. Enter a **Cluster Name**.

4. {{< step_create-cluster_member-roles >}}

5. {{< step_create-cluster_cluster-options >}}

6. {{< step_create-cluster_node-pools >}}

    {{< img "/img/rancher/vsphere-cluster-create-1.png" "Image">}}

7. Review your configuration, then click **Create**.

> **Note:**
> 
> If you have a cluster with DRS enabled, setting up [VM-VM Affinity Rules](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.resmgmt.doc/GUID-7297C302-378F-4AF2-9BD6-6EDB1E0A850A.html) is recommended. These rules allow VMs  assigned the etcd and control-plane roles to operate on separate ESXi hosts when they are assigned to different node pools. This practice ensures that the failure of a single physical machine does not affect the availability of those planes.

{{< result_create-cluster >}}

# Configuring an Operating System for Nodes

_Available as of v2.3.3_

Prior to v2.3.3, only RancherOS was supported, but in this version, you can configure a vSphere node template to use any operating system that supports cloud init.

# Node Template Configuration Reference

Refer to [this section]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/node-template-reference) for a reference on the configuration options available for vSphere node templates.

# Provisioning Storage

> Before Rancher can provision storage in vSphere, you need to [enable the vSphere provider.](#enabling-the-vsphere-provider-in-rancher)

For an example of how to provision storage in vSphere using Rancher, refer to the 
 [cluster administration section.]({{<baseurl>}}/rancher/v2.x/en/cluster-admin/volumes-and-storage/examples/vsphere)
