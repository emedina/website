---
reviewers:
- mikedanese
- luxas
- jbeda
title: kubeadm upgrade
content_template: templates/concept
weight: 40
---
{{% capture overview %}}
`kubeadm upgrade` is a user-friendly command that wraps complex upgrading logic behind one command, with support
for both planning an upgrade and actually performing it. `kubeadm upgrade` can also be used for downgrading
cluster if necessary.
{{% /capture %}}

{{% capture body %}}

## kubeadm upgrade guidance

Every upgrade process might be a bit different, so we've documented each minor upgrade process individually.
For more version-specific upgrade guidance, see the following resources:

 * [1.12 to 1.13 upgrades](/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade-1-13/)
 * [1.13 to 1.14 upgrades](/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade-1-14/)
 * [1.14 to 1.15 upgrades](/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade-1-15/)

_For older versions, please refer to older documentation sets on the Kubernetes website._

In Kubernetes v1.11.0 and later, you can use `kubeadm upgrade diff` to see the changes that would be
applied to static pod manifests.

To use kube-dns with upgrades in Kubernetes v1.13.0 and later please follow [this guide](/docs/reference/setup-tools/kubeadm/kubeadm-init-phase/#cmd-phase-addon).

In Kubernetes v1.15.0 and later, `kubeadm upgrade apply` and `kubeadm upgrade node` will also
automatically renew the kubeadm managed certificates on this node, including those stored in kubeconfig files.
To opt-out, it is possible to pass the flag `--certificate-renewal=false`. For more details about certificate
renewal see the [certificate management documentation](docs/tasks/administer-cluster/kubeadm/kubeadm-certs).

## kubeadm upgrade plan {#cmd-upgrade-plan}
{{< include "generated/kubeadm_upgrade_plan.md" >}}

## kubeadm upgrade apply  {#cmd-upgrade-apply}
{{< include "generated/kubeadm_upgrade_apply.md" >}}

## kubeadm upgrade diff {#cmd-upgrade-diff}
{{< include "generated/kubeadm_upgrade_diff.md" >}}

## kubeadm upgrade node {#cmd-upgrade-node}
{{< include "generated/kubeadm_upgrade_node.md" >}}

{{% /capture %}}

{{% capture whatsnext %}}
* [kubeadm config](/docs/reference/setup-tools/kubeadm/kubeadm-config/) if you initialized your cluster using kubeadm v1.7.x or lower, to configure your cluster for `kubeadm upgrade`
{{% /capture %}}
