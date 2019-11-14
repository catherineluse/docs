---
title: Creating vSphere Credentials to Create Clusters
weight: 1
---

This section describes how to create a vSphere username and password that you can provide to Rancher as a cloud credential, which will allow Rancher to provision resources in vSphere.

The following steps create a role with the required privileges and then assign it to a new user in the vSphere console:

1. From the **vSphere** console, go to the **Administration** page.

2. Go to the **Roles** tab.

3. Create a new role.  Give it a name and select the privileges listed in the [permissions table](#annex-vsphere-permissions).

    {{< img "/img/rancher/rancherroles1.png" "image">}}

4. Go to the **Users and Groups** tab.

5. Create a new user. Fill out the form and then click **OK**. Make sure to note the username and password, as you will need it when configuring node templates in Rancher.

    {{< img "/img/rancher/rancheruser.png" "image">}}

6. Go to the **Global Permissions** tab.

7. Create a new Global Permission.  Add the user you created earlier and assign it the role you created earlier. Click **OK**.

    {{< img "/img/rancher/globalpermissionuser.png" "image">}}

    {{< img "/img/rancher/globalpermissionrole.png" "image">}}