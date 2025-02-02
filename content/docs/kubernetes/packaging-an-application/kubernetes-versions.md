---
date: "2018-08-14T04:02:20Z"
title: "Auto-Upgrading Replicated"
description: "Configured Replicated to automatically upgrade"
weight: "2617"
categories: [ "Packaging a Kubernetes Application" ]
index: ["docs/kubernetes", "docs"]
gradient: "kubernetes"
icon: "replicatedKubernetes"
nextPage: "kubernetes/customer-installations/overview.md"
---

{{<kotsdocs>}}
For upgrading KOTS, see [kubectl kots admin-console upgrade](https://kots.io/kots-cli/admin-console/).

For upgrading kURL, see [kurl.sh/docs/install-with-kurl/upgrading](https://kurl.sh/docs/install-with-kurl/upgrading)
{{</kotsdocs>}}

Replicated is capable of auto-upgrading itself to any version that is [pinned to the same version of Kubernetes](/docs/kubernetes/customer-installations/installing/#compatible-kubernetes-versions).
Set the `replicated_version` in the `host_requirements` section of your Replicated yaml to a range to enable this feature.

```yaml
host_requirements:
  replicated_version: "<=2.26.0"
```

If configured, Replicated will attempt to auto-upgrade itself every time your app is updated to a new version.
This feature is not enabled on airgap installs.

Auto-upgrades can be disabled by setting the `DisableReplicatedAutoUpdates` parameter from the [cli](https://help.replicated.com/api/replicatedctl/replicatedctl_params_set/).
