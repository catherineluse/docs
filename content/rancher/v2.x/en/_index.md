---
title: "Rancher 2.x"
shortTitle: "Rancher 2.x"
description: "Rancher adds significant value on top of Kubernetes: managing hundreds of clusters from one interface, centralizing RBAC, enabling monitoring and alerting. Read more."
metaTitle: "Rancher 2.x Docs: What is New?"
metaDescription: "Rancher 2 adds significant value on top of Kubernetes: managing hundreds of clusters from one interface, centralizing RBAC, enabling monitoring and alerting. Read more."
insertOneSix: true
weight: 1
ctaBanner: intro-k8s-rancher-online-training
---

### About Rancher

Rancher is a container management platform built for organizations that deploy containers in production. Rancher makes it easy to run Kubernetes everywhere, meet IT requirements, and empower DevOps teams.

One Rancher management server, running on a dedicated Kubernetes cluster, can manage hundreds of downstream Kubernetes clusters from the same user interface.

Rancher can also be installed in a single Docker container for development and testing purposes.

### Kubernetes Cluster Management

Rancher was originally built to work with multiple orchestrators, and it included its own orchestrator called Cattle.

With the rise of Kubernetes in the marketplace, Rancher now exclusively deploys and manages multiple Kubernetes clusters running anywhere, on any provider.

It can provision Kubernetes from a hosted provider, provision compute nodes and then install Kubernetes onto them, or inherit existing Kubernetes clusters running anywhere.

Rancher adds significant value on top of Kubernetes. Rancher's features include the following:

- Centralizes role-based access control (RBAC) for all of the clusters
- Gives global admins the ability to control cluster access from one location
- Enables detailed monitoring and alerting for clusters and their resources and ships logs to external providers
- Integrates directly with Helm via the Application Catalog
- Automates and deploys workloads. If you have an external CI/CD system, you can plug it into Rancher, but if you don't, Rancher even includes a pipeline engine for automation and deployment. 

Rancher is a _complete_ container management platform for Kubernetes, giving you the tools to successfully run Kubernetes anywhere.

### Further Reading

- [Overview]({{<baseurl>}}/rancher/v2.x/en/overview/)
- [Architecture]({{<baseurl>}}/rancher/v2.x/en/overview/architecture/)
- [Installation Options]({{<baseurl>}}/rancher/v2.x/en/installation/)
- [Best Practices Guide]({{<baseurl>}}/rancher/v2.x/en/best-practices/)
- [Setting up Kubernetes Clusters in Rancher]({{<baseurl>}}/rancher/v2.x/en/cluster-provisioning/)