---
title: CLI with Rancher
weight: 100
---

Interact with Rancher using command line interface (CLI) tools from your workstation.

### Rancher CLI

Follow the steps in [Rancher CLI]({{<baseurl>}}/rancher/v2.x/en/cli/).

Ensure you can run `rancher kubectl get pods` successfully.

### kubectl
Install the `kubectl` utility. See [install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

Configure kubectl by visiting your cluster in the Rancher Web UI, then clicking on `Kubeconfig`, copying the contents, and putting it into your `~/.kube/config` file.

For more information on setting up `kubectl` to access downstream Kubernetes clusters from your workstation, refer to [this page.]({{<baseurl>}}/rancher/v2.x/en/cluster-admin/cluster-access/kubectl/)

Run `kubectl cluster-info` or `kubectl get pods` successfully.