---
date: "2018-08-14T04:02:20Z"
title: "Managing Storage"
description: "Instructions for managing the storage needs of Kubernetes in customer environments"
keywords: "storage, kubernetes"
weight: "2717"
categories: [ "Manage Customer Installations" ]
index: ["docs/kubernetes", "docs"]
gradient: "kubernetes"
icon: "replicatedKubernetes"
---

{{<kotsdocs>}}
For details on storage for KOTS + kURL embedded clusters, consult the appropriate [kURL add-on docs](https://kurl.sh/docs/introduction/what-kurl-does#after-kubeadm-adding-add-ons).
{{</kotsdocs>}}

Customers will generally need at least 40GB of free disk space to install and run Replicated.
Online installs and applications with small airgap bundles may succeed with less than this amount, while airgap installs with a large application package may require more.
Other factors that affect disk usage are container logging, the size of your application's local volumes, and customer's creation of local snapshots.

{{< linked_headline "Rook Persistent Volumes (PVs)" >}}

Replicated will create four Persistent Volume Claims in addition to any defined by your application.
Storage for all PVCs will be provisioned from `/opt/replicated/rook` across all nodes in the cluster.

### Opt-In Storage Nodes

Optionally, Replicated can be configured to provision storage on only specific nodes which include the label `node-role.kubernetes.io/rook=true`.
The flag `use-rook-nodes-label` (or the query parameter `?use_rook_nodes_label=1`) can be passed to the [install scripts](/docs/kubernetes/customer-installations/installing/#quick-install) to enable this feature.
Once enabled, passing the flag `rook-storage-node` to the [node join](/docs/kubernetes/customer-installations/add-nodes/) script will automatically add the `node-role.kubernetes.io/rook=true` label to the additional node and provision that node as a storage node.

{{< linked_headline "Docker Images" >}}

For online installs, images required for running Replicated and your application will only be pulled to nodes where they are scheduled to run.
Airgap installs require that a copy of every image be imported to Docker under `/var/lib/docker` on the primary node and a separate copy to Replicated's local registry, also on the primary node.
Unpacking your application airgap bundle will require five times as much storage space as the size of the bundle download.

{{< linked_headline "Preventing Disk Shortages" >}}

Customers that provide insufficient disk space will see `DiskPressure` warnings on the Clusters page of the Replicated Admin UI and in the output of `kubectl get nodes`.
If Kubernetes is not able to reclaim more storage space by deleting pods and deleting unused images, it will begin to evict running pods.
This can begin when a customer's disk is only 85% full.
It is critical to benchmark your app's storage requirements and enforce them with a [preflight check](/docs/kubernetes/packaging-an-application/programmable-preflight-checks/).


{{< linked_headline "Storage Requirements Example" >}}

A small app with a 1GB airgap bundle and a single 10GB Persistent Volume Claim would require 15GB to install in addition to the 16GB required for a base Replicated airgap install.
Adding ~25% capacity to allow for accumulating container logs without triggering Kubernetes evicition levels, a preflight check of 40GB would be the minimum required for the app.

{{< linked_headline "HostPath Storage Provisioner" >}}

As an alternative to the Rook storage provisioner, Replicated provides a host path provisioner. This provisioner is ideal in single node installations to avoid the overhead of Rook. This provisioner can be enabled by passing in additional flags to the Replicated Kubernetes init scripts. See the example below:

```yaml
curl -sSL https://get.replicated.com/kubernetes-init | sudo bash -s storage-provisioner=hostpath
```

{{< linked_headline "Alternate Storage Provisioners" >}}

It is possible to bring your own storage provisioner when necessary. See the example below to omit Replicated's provided storage provisioner:

```yaml
curl -sSL https://get.replicated.com/kubernetes-init | sudo bash -s storage-provisioner=0 storage-class=standard
```

{{< linked_headline "Troubleshooting" >}}

The [Ceph Dashboard](https://github.com/rook/rook/blob/master/Documentation/ceph-dashboard.md) is included in the `rook-ceph-mgr` pod of the `rook-ceph` namespace and proxied through the `/ceph` path of the Replicated console.

The [Rook toolbox](https://rook.io/docs/rook/master/ceph-toolbox.html) provides a convenient image for monitoring and debugging your Rook cluster. You can run it as a pod in the `rook-ceph` namespace and have access to commands such as `ceph status` to check the health of your storage cluster.
