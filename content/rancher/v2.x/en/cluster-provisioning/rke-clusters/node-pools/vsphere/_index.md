---
title: Creating a vSphere Cluster
shortTitle: vSphere
weight: 2225
aliases:
  - /rancher/v2.x/en/tasks/clusters/creating-a-cluster/create-cluster-vsphere/
---
Use {{<product>}} to create a Kubernetes cluster in vSphere.

vSphere is an infrastructure provider that allows you to manage on-premesis nodes.

When creating a vSphere cluster, Rancher first provisions the specified amount of virtual machines by communicating with the vCenter API. Then it installs Kubernetes on top of them.

A vSphere cluster may consist of multiple groups of VMs with distinct properties, such as the amount of memory or the number of vCPUs. This grouping allows for fine-grained control over the sizing of nodes for the data, control, and worker plane respectively.

>**Note:**
>The vSphere node driver included in Rancher currently only supports the provisioning of VMs with [RancherOS]({{< baseurl >}}/os/v1.x/en/) as the guest operating system.

This section covers the following topics:

- [Prerequisites](#prerequisites)
  - [vSphere API permissions](#vsphere-api-permissions)
  - [Network permissions](#network-permissions)
  - [Creating vSphere credentials to create clusters](#creating-vsphere-credentials-to-create-clusters)
- [Enabling the vSphere provider in Rancher](#enabling-the-vsphere-provider-in-rancher)
- [Enabling Disk UUIDs with a node template](#enabling-disk-uuids-with-a-node-template)
- [Creating vSphere Clusters in Rancher](#creating-vsphere-clusters-in-rancher)
  - [1. Create a Node Template Using vSphere Credentials](#1-create-a-node-template-using-vsphere-credentials)
  - [2. Create a vSphere Kubernetes cluster with the node template](#2-create-a-vsphere-kubernetes-cluster-with-the-node-template)
- [Provisioning Storage](#provisioning-storage)
- [Node template configuration reference](#node-template-configuration-reference)

# Prerequisites

The following table lists the permissions required for the vSphere user account configured in the node templates:

| Privilege Group       | Operations  |
|:----------------------|:-----------------------------------------------------------------------|
| Datastore             | AllocateSpace </br> Browse </br> FileManagement (Low level file operations) </br> UpdateVirtualMachineFiles </br> UpdateVirtualMachineMetadata |
| Network               | Assign |
| Resource              | AssignVMToPool |
| Virtual Machine       | Config (All) </br> GuestOperations (All) </br> Interact (All) </br> Inventory (All) </br> Provisioning (All) |

### vSphere API Permissions

Before proceeding to create a cluster, you must ensure that you have a vSphere user with sufficient permissions. If you are planning to make use of vSphere volumes for persistent storage in the cluster, there are [additional requirements]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/) that must be met.

### Network Permissions

You must ensure that the hosts running Rancher servers are able to establish network connections to the following network endpoints:

- vCenter server (usually port 443/TCP)
- Every ESXi host that is part of the datacenter to be used to provision virtual machines for your clusters (port 443/TCP).

### Creating vSphere Credentials to Create Clusters

Refer to this [how-to guide]({{<baseurl>}}/rancher/v2.x/en/cluster/rke-clusters/node-pools/vsphere/creating-credentials) for instructions on how to create a user in vSphere with the required permissions. These steps result in a username and password that you will need to provide to Rancher as a cloud credential, which allows Rancher to provision resources in vSphere.

# Enabling the vSphere Provider in Rancher

When provisioning clusters in Rancher using the [vSphere node driver]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/) or on pre-created [custom nodes]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/custom-nodes/) the cluster YAML file must be modified in order to enable the cloud provider.

1. Log in to the Rancher UI as admin user.
2. Navigate to **Clusters** in the **Global** view.
3. Click **Add Cluster** and select the **vSphere** infrastructure provider.
4. Assign a **Cluster Name**.
5. Assign **Member Roles** as required.
6. Expand **Cluster Options** and configure as required.
7. Set **Cloud Provider** option to `Custom`.

    {{< img "/img/rancher/vsphere-node-driver-cloudprovider.png" "vsphere-node-driver-cloudprovider">}}

8. Click on **Edit as YAML**
9. Insert the following structure to the pre-populated cluster YAML. As of Rancher v2.3+, this structure must be placed under `rancher_kubernetes_engine_config`. In versions prior to v2.3, it has to be defined as a top level field. Note that the `name` *must* be set to `vsphere`. Refer to the [configuration reference](#configuration-reference) to learn about the properties of the `vsphereCloudProvider` directive.

    ```yaml
    rancher_kubernetes_engine_config: # Required as of Rancher v2.3+
      cloud_provider:
          name: vsphere
          vsphereCloudProvider:
              [Insert provider configuration]
    ```

10. Configure the **Node Pools** per your requirements while ensuring to use a node template that enables disk UUIDs for the VMs (See [Annex - Enable disk UUIDs for vSphere VMs]).
11. Click on **Create** to start provisioning the VMs and Kubernetes services.


# Enabling Disk UUIDs with a Node Template

When creating new clusters in Rancher using vSphere node templates, you can configure the template to automatically enable disk UUIDs for all VMs created for a cluster:

1. Navigate to the **Node Templates** in the Rancher UI while logged in as admin user.

2. Add or edit an existing vSphere node template.

3. Under **Instance Options** click on **Add Parameter**.

4. Enter `disk.enableUUID` as key with a value of **TRUE**.

    {{< img "/img/rke/vsphere-nodedriver-enable-uuid.png" "vsphere-nodedriver-enable-uuid">}}

5. Click **Create** or **Save**.

# Creating vSphere Clusters in Rancher

Create vSphere clusters in Rancher by following these steps:

1. [Create a vSphere node template using vSphere credentials](#1-create-a-vsphere-node-template-using-vsphere-credentials)
2. [Create a vSphere Kubernetes Cluster with the node template](#2-create-a-vsphere-kubernetes-cluster-with-the-node-template)

### 1. Create a Node Template Using vSphere Credentials

To create a cluster, you need to create at least one vSphere [node template]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/#node-templates)  that specifies how VMs are created in vSphere.

>**Note:**
>Once you create a node template, it is saved, and you can re-use it whenever you create additional vSphere clusters.

1. Log in with an admin account to the Rancher UI.

2. From the user settings menu, select **Node Templates**.

3. Click **Add Template** and then click on the **vSphere** icon.

4. Under [Account Access](#account-access) enter the vCenter FQDN or IP address and the credentials for the vSphere user account (see [Prerequisites](#prerequisites)).

	{{< step_create-cloud-credential >}}

5. Under [Instance Options](#instance-options), configure the number of vCPUs, memory, and disk size for the VMs created by this template.

6. **Optional:** Enter the URL pointing to a [RancherOS]({{< baseurl >}}/os/v1.x/en/) cloud-config file in the [Cloud Init](#instance-options) field.

7. Ensure that the [OS ISO URL](#instance-options) contains the URL of a VMware ISO release for RancherOS (`rancheros-vmware.iso`).

    {{< img "/img/rancher/vsphere-node-template-1.png" "image">}}

8. **Optional:** Provide a set of [Configuration Parameters](#instance-options) for the VMs.

9. Under **Scheduling**, enter the name/path of the **Data Center** to create the VMs in, the name of the **VM Network** to attach to, and the name/path of the **Datastore** to store the disks in.

    {{< img "/img/rancher/vsphere-node-template-2.png" "image">}}

10. **Optional:** Assign labels to the VMs that can be used as a base for scheduling rules in the cluster.

11. **Optional:** Customize the configuration of the Docker daemon on the VMs that will be created.

10. Assign a descriptive **Name** for this template and click **Create**.

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

# Provisioning Storage

For an example of how to provision storage in vSphere using Rancher, refer to the 
 [cluster administration section.]({{<baseurl>}}/rancher/v2.x/en/cluster-admin/volumes-and-storage/examples/vsphere)

# Node Template Configuration Reference

Refer to [this section]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/node-template-reference) for a reference on the configuration options available for vSphere node templates.
