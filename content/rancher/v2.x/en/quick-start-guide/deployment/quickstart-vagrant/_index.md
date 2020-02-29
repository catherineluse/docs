---
title: Vagrant Quick Start
weight: 200
---
In this guide, you will learn how to quickly deploy the Rancher management server in a single Docker container. You will also deploy one downstream single-node Kubernetes cluster.

> **Note:** In a production environment, the Rancher server would run on a separate, designated Kubernetes cluster. For more information, refer to our [Best Practices Guide.]({{<baseurl>}}/rancher/v2.x/en/best-practices/)

## Prerequisites

- [Vagrant](https://www.vagrantup.com): Vagrant is required as this is used to provision the machine based on the Vagrantfile.
- [Virtualbox](https://www.virtualbox.org): The virtual machines that Vagrant provisions need to be provisioned to VirtualBox.
- At least 4GB of free RAM.

## Getting Started

1. Clone [Rancher Quickstart](https://github.com/rancher/quickstart) to a folder using `git clone https://github.com/rancher/quickstart`.

2. Go into the folder containing the Vagrantfile by executing `cd quickstart/vagrant`.

3. **Optional:** Edit `config.yaml` to:

    - Change the number of nodes and the memory allocations, if required. (`node.count`, `node.cpus`, `node.memory`)
    - Change the password of the `admin` user for logging into Rancher. (`default_password`)

4. To initiate the creation of the environment run, `vagrant up`.

5. Once provisioning finishes, go to `https://172.22.101.101` in the browser. This IP has been configured as the Rancher management server IP, a permanent configuration. Downstream Kubernetes clusters will communicate with the Rancher management server by reaching this IP. The default user/password is `admin/admin`.

**Result:** Rancher Server and your Kubernetes cluster is installed on VirtualBox.

### What's Next?

Use Rancher to create a deployment. For more information, see [Creating Deployments]({{<baseurl>}}/rancher/v2.x/en/quick-start-guide/workload).

## Destroying the Environment

1. From the `quickstart/vagrant` folder execute `vagrant destroy -f`.

2. Wait for the confirmation that all resources have been destroyed.
