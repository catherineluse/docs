---
title: Enabling Disk UUIDs in Node Templates
weight: 3
---

When creating new clusters in Rancher using vSphere node templates, you can configure the template to automatically enable disk UUIDs for all VMs created for a cluster:

1. Navigate to the **Node Templates** in the Rancher UI while logged in as admin user.

2. Add or edit an existing vSphere node template.

3. Under **Instance Options** click on **Add Parameter**.

4. Enter `disk.enableUUID` as key with a value of **TRUE**.

    {{< img "/img/rke/vsphere-nodedriver-enable-uuid.png" "vsphere-nodedriver-enable-uuid">}}

5. Click **Create** or **Save**.

**Result:** The disk UUID is enabled in the vSphere node template.