---
date: "2016-07-03T04:02:20Z"
title: "Adding Nodes"
description: "Instructions for adding a node to an existing cluster"
keywords: "installing, cluster, kubernetes"
weight: "2708"
categories: [ "Manage Customer Installations" ]
index: ["docs/kubernetes", "docs"]
gradient: "kubernetes"
icon: "replicatedKubernetes"
---
{{<kotsdocs>}}
For information about adding nodes to a kURL cluster, check out the [kurl.sh docs](https://kurl.sh/docs/install-with-kurl/adding-nodes).
{{</kotsdocs>}}

When an application is configured by the vendor with a clustering strategy, Replicated makes it possible for the end customer to install additional nodes on remote instances to run a distributed application.

On the Cluster page on the On-Prem Console an "Add Node" button will be visible. This will prompt the end customer with an option for adding a node.

Additionally, the node join script can be generated using the CLI command `replicatedctl cluster node-join-script`. For more information please refer to the documentation [here](https://help.replicated.com/api/replicatedctl/replicatedctl_cluster_node-join-script/).

The join script will add Docker and all dependencies for a secondary node in a Kubernetes cluster, then join the cluster through the primary node.

![Add Node Script](/images/post-screens/add-node-k8s.png)
