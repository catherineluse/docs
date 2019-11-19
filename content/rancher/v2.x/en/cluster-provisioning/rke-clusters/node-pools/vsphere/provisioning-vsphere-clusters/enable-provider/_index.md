---
title: Enabling the vSphere Provider
weight: 2
---
Before provisioning vSphere clusters in Rancher, you need to modify the cluster YAML file to enable the vSphere provider.

This requirement applies to both pre-created [custom nodes]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/custom-nodes/) and for nodes created in Rancher using the vSphere node driver.

To enable the vSphere provider for cluster, follow these steps:

1. Log in to the Rancher UI as admin user.
2. Navigate to **Clusters** in the **Global** view.
3. Click **Add Cluster** and select the **vSphere** infrastructure provider.
4. Assign a **Cluster Name**.
5. Assign **Member Roles** as required.
6. Expand **Cluster Options** and configure as required.
7. Set **Cloud Provider** option to `Custom`.

    {{< img "/img/rancher/vsphere-node-driver-cloudprovider.png" "vsphere-node-driver-cloudprovider">}}

8. Click on **Edit as YAML**
9. Insert the following structure to the pre-populated cluster YAML. As of Rancher v2.3+, this structure must be placed under `rancher_kubernetes_engine_config`. In versions prior to v2.3, it has to be defined as a top level field. Note that the `name` *must* be set to `vsphere`. 

    ```yaml
    rancher_kubernetes_engine_config: # Required as of Rancher v2.3+
      cloud_provider:
          name: vsphere
          vsphereCloudProvider:
              [Insert provider configuration]
    ```

    Refer to the [configuration reference](#configuration-reference) or [example configuration]([link]({{<baseurl>}}/rke/latest/en/config-options/cloud-providers/vsphere/config-example) to learn about the properties of the `vsphereCloudProvider` directive.

10. Configure the **Node Pools** per your requirements while ensuring to use a node template that enables disk UUIDs for the VMs (See the section on [enabling disk UUIDs for vSphere VMs.](#enabling-disk-uuids-with-a-node-template)
11. Click on **Create** to start provisioning the VMs and Kubernetes services.