---
title: Provisioning Kubernetes Clusters in vSphere
weight: 1
---

This section explains how to configure Rancher with vSphere credentials, provision nodes in vSphere, and set up Kubernetes clusters on those nodes.

# Prerequisites

This section describes how to set up vSphere so that Rancher can provision vSphere VMs and clusters.

### vSphere API Permissions

Before proceeding to create a cluster, you must ensure that you have a vSphere user with sufficient permissions. When you set up a node template, the template will need to use these vSphere credentials.

The following table lists the permissions required for the vSphere user account. 

| Privilege Group       | Operations  |
|:----------------------|:-----------------------------------------------------------------------|
| Datastore             | AllocateSpace </br> Browse </br> FileManagement (Low level file operations) </br> UpdateVirtualMachineFiles </br> UpdateVirtualMachineMetadata |
| Network               | Assign |
| Resource              | AssignVMToPool |
| Virtual Machine       | Config (All) </br> GuestOperations (All) </br> Interact (All) </br> Inventory (All) </br> Provisioning (All) |

### Network Permissions

You must ensure that the hosts running Rancher servers are able to establish network connections to the following network endpoints:

- vCenter server (usually port 443/TCP)
- Every ESXi host that is part of the datacenter to be used to provision virtual machines for your clusters (port 443/TCP).

# Creating Clusters in vSphere with Rancher

This section describes how to set up vSphere credentials, node templates, and vSphere clusters using the Rancher UI.

You will need to do the following:

1. [Configure vSphere credentials](#1-configure-vsphere-credentials)
2. [Enable the vSphere provider](#2-enable-the-vsphere-provider)
3. [If not already enabled, enable disk UUIDs](#3-if-not-already-enabled-enable-disk-uuids)
4. [Create a node template using vSphere credentials](#4-create-a-node-template-using-vsphere-credentials)
  [Node template configuration reference](#node-template-configuration-reference)
5. [Create a Kubernetes cluster using the node template](#5-create-a-kubernetes-cluster-using-the-node-template)
6. [Optional: Provision storage](#6-optional-provision-storage)

# 1. Configure vSphere Credentials

Refer to this [how-to guide]({{<baseurl>}}/rancher/v2.x/en/cluster/rke-clusters/node-pools/vsphere/creating-credentials) for instructions on how to create a user in vSphere with the required permissions. These steps result in a username and password that you will need to provide to Rancher as a cloud credential, which allows Rancher to provision resources in vSphere.

# 2. Enable the vSphere Provider

Before provisioning vSphere clusters in Rancher, you need to modify the cluster YAML file to enable the vSphere provider.

This requirement applies to both pre-created [custom nodes]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/custom-nodes/) and for nodes created in Rancher using the vSphere node driver.

Refer to [these steps]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/enable-provider) for details.

# 3. If Not Already Enabled, Enable Disk UUIDs

In order to provision nodes with RKE, all nodes must be configured with disk UUIDs.

As of Rancher v2.0.4, disk UUIDs are enabled in vSphere node templates by default.

If you are using Rancher prior to v2.0.4, refer to these [instructions]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/provisioning-vsphere-clusters/#enabling-disk-uuids-with-a-node-template) for details on how to enable a UUID with a Rancher node template.

# 4. Create a Node Template Using vSphere Credentials

To create a cluster, you need to create at least one vSphere [node template]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/#node-templates) that specifies how VMs are created in vSphere.

After you create a node template, it is saved, and you can re-use it whenever you create additional vSphere clusters.

To create a node template,

1. Log in with an admin account to the Rancher UI.

1. From the user settings menu, select **Node Templates**.

1. Click **Add Template** and then click on the **vSphere** icon.

1. Under [Account Access](#account-access) enter the vCenter FQDN or IP address and the credentials for the vSphere user account.

	{{< step_create-cloud-credential >}}

### Configuring Node Scheduling
%%%% TABS FOR SCHEDULING

{{% tabs %}}
{{% tab "Rancher v2.3.3+" %}}
{{% /tab %}}
{{% tab "Rancher prior to v2.3.3" %}}

Under **Scheduling**, enter the name/path of the **Data Center** to create the VMs in, the name of the **VM Network** to attach to, and the name/path of the **Datastore** to store the disks in.

    {{< img "/img/rancher/vsphere-node-template-2.png" "image">}}

{{% /tab %}}
{{% /tabs %}}



### Configuring Instances and Operating Systems
{{% tabs %}}
{{% tab "Rancher v2.3.3+" %}}

Use the **Creation method** field to configure the method for setting up an operating system on the node.

You can configure the node template to use any operating system that supports cloud init.

Ensure that the [OS ISO URL](#instance-options) contains the URL of a VMware ISO release. 

{{% /tab %}}
{{% tab "Rancher prior to v2.3.3" %}}

Under [Instance Options](#instance-options), configure the number of vCPUs, memory, and disk size for the VMs created by this template.

Only RancherOS VMs are supported.

Ensure that the [OS ISO URL](#instance-options) contains the URL of a VMware ISO release. The URL for RancherOS is (`rancheros-vmware.iso`).

    {{< img "/img/rancher/vsphere-node-template-1.png" "image">}}

{{% /tab %}}
{{% /tabs %}}

### Configuring Tags and Custom Attributes
{{% tabs %}}
{{% tab "Rancher v2.3.3+" %}}

 > **Note:** Custom attributes are a legacy feature that will eventually be removed from vSphere. These attributes allow you to attach metadata to objects in the vSphere inventory to make it easier to sort and search for these objects.

{{% /tab %}}
{{% tab "Rancher prior to v2.3.3" %}}

**Optional:**

  - Provide a set of [Configuration Parameters](#instance-options) for the VMs.
  - Assign labels to the VMs that can be used as a base for scheduling rules in the cluster.
  - Customize the configuration of the Docker daemon on the VMs that will be created.
  - As of v2.3.3., you can optionally add vSphere tags and custom attributes. Tags allow you to attach metadata to objects in the vSphere inventory to make it easier to sort and search for these objects.

    > **Note:** Custom attributes are a legacy feature that will eventually be removed from vSphere. These attributes allow you to attach metadata to objects in the vSphere inventory to make it easier to sort and search for these objects.

{{% /tab %}}
{{% /tabs %}}

### Configuring Cloud Init

**Optional:** Enter the URL pointing to a cloud-config file in the [Cloud Init](#instance-options) field.

### Saving the Node Template

1. Assign a descriptive **Name** for this template and click **Create**.

# 5. Create a Kubernetes Cluster Using the Node Template

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

### Node Template Configuration Reference

Refer to [this section]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/node-template-reference) for a reference on the configuration options available for vSphere node templates.

# 6. Optional: Provision Storage

For an example of how to provision storage in vSphere using Rancher, refer to the 
 [cluster administration section.]({{<baseurl>}}/rancher/v2.x/en/cluster-admin/volumes-and-storage/examples/vsphere)

 In order to provision storage in vSphere, the vSphere provider must be enabled.