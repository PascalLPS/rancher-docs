---
title: Rancher Logging 集成
description: Rancher 集成了主流的日志服务。了解集成日志服务的要求和优势，并在你的集群上启用 Logging。
---

现在，Rancher 的日志管理由 [Banzai Cloud Logging operator](https://banzaicloud.com/docs/one-eye/logging-operator/) 提供支持，它取代了以前的内部解决方案。

有关 Rancher 2.5 更改的概述，请参阅[本节](../explanations/integrations-in-rancher/logging/logging-architecture.md#rancher-2-5-变更)。有关迁移 Logging V1 的更多信息，请参阅[本页](../explanations/integrations-in-rancher/logging/migrate-to-rancher-v2.5+-logging.md)。


## 启用 Logging

你可以转到**应用**页面并安装 Logging 应用程序，从而为 Rancher 管理的集群启用 Logging：

1. 转到要安装 Logging 的集群，然后单击**应用 & 应用市场**。
1. 点击 **Logging** 应用。
1. 滚动到 Helm Chart README 的底部，然后单击**安装**。

**结果**：Logging 应用已部署到 `cattle-logging-system` 命名空间中。

## 卸载 Logging

1. 转到要安装 Logging 的集群，然后单击**应用 & 应用市场**。
1. 点击**已安装的应用**。
1. 转到 `cattle-logging-system` 命名空间并选中 `rancher-logging` 和 `rancher-logging-crd` 框。
1. 单击**删除**。
1. 确认**删除**。

**结果**：已卸载 `rancher-logging`。

## 架构

有关 Logging 应用程序工作原理的更多信息，请参阅[本节](../explanations/integrations-in-rancher/logging/logging-architecture.md)。



## RBAC

Rancher Logging 有两个角色，分别是 `logging-admin` 和 `logging-view`。有关如何以及何时使用这些角色的更多信息，请参阅[此页面](../explanations/integrations-in-rancher/logging/rbac-for-logging.md)。

## 配置 Logging 自定义资源

要管理 `Flows`、`ClusterFlows`、`Outputs` 和 `ClusterOutputs`：

1. 在左上角，单击 **☰ > 集群管理**。
1. 在**集群**页面上，转到要配置 Logging 自定义资源的集群，然后单击 **Explore**。
1. 在左侧导航栏中，单击 **Logging**。

### Flows 和 ClusterFlows

有关配置 `Flows` 和 `ClusterFlows` 的帮助，请参阅[此页面](../explanations/integrations-in-rancher/logging/custom-resource-configuration/flows-and-clusterflows.md)。

### Outputs 和 ClusterOutputs

有关配置 `Outputs` 和 `ClusterOutputs` 的帮助，请参阅[此页面](../explanations/integrations-in-rancher/logging/custom-resource-configuration/outputs-and-clusteroutputs.md)。

## 配置 Logging Helm Chart

有关在安装或升级 Logging 应用程序时可配置的选项，请参阅[此页面](../explanations/integrations-in-rancher/logging/logging-helm-chart-options.md)。

### Windows 支持

Windows 集群支持 Logging，你可以收集 Windows 节点的日志。

有关如何在 Windows 节点上启用或禁用 Logging 的详细信息，请参阅[本节](../explanations/integrations-in-rancher/logging/logging-helm-chart-options.md#启用/禁用-windows-节点-logging)。

### 使用自定义 Docker 根目录

有关使用自定义 Docker 根目录的详细信息，请参阅[本节](../explanations/integrations-in-rancher/logging/logging-helm-chart-options.md#使用自定义-docker-根目录)。


### 处理污点和容忍度

有关如何在 Logging 应用程序中使用污点和容忍度的信息，请参阅[此页面](../explanations/integrations-in-rancher/logging/taints-and-tolerations.md)。


### 在 SELinux 上使用 Logging V2

有关在启用了 SELinux 的节点上使用 Logging 应用程序的信息，请参阅[本节](../explanations/integrations-in-rancher/logging/logging-helm-chart-options.md#启用-logging-应用程序以使用-selinux)。

### 其他日志来源

默认情况下，Rancher 会收集所有类型集群的 controlplane 组件和节点组件的日志。在某些情况下，也会收集其他日志。有关详细信息，请参阅[本节](../explanations/integrations-in-rancher/logging/logging-helm-chart-options.md#其他日志来源)。


## 故障排除

### `cattle-logging` 命名空间正在重新创建

如果你的集群之前在旧版 Rancher UI 的全局视图中部署了 Logging，`cattle-logging` 命名空间可能会不断被重新创建。

要解决这个问题，你可以将所有 `clusterloggings.management.cattle.io` 和 `projectloggings.management.cattle.io` 自定义资源从管理集群中针对该集群的命名空间中删除。
这些自定义资源会导致 Rancher 在下游集群中创建 `cattle-logging` 命名空间（如果不存在）。

集群命名空间与集群 ID 匹配，因此我们需要找到每个集群的集群 ID。

1. 在左上角，单击 **☰ > 集群管理**。
1. 在**集群**页面上，转到要获取 ID 的集群，然后单击 **Explore**。
2. 从以下其中一个 URL 中复制 `<cluster-id>` 的内容。`<cluster-id>` 是集群命名空间名称。

```bash
# Cluster Management UI
https://<your-url>/c/<cluster-id>/

# Cluster Dashboard
https://<your-url>/dashboard/c/<cluster-id>/
```

现在我们有了 `<cluster-id>` 命名空间，我们可以删除导致 `cattle-logging` 不断重新创建的自定义资源。
*警告*：请当前未使用确保 Logging（从旧版 Rancher UI 全局视图中安装的版本）。

```bash
kubectl delete clusterloggings.management.cattle.io -n <cluster-id>
kubectl delete projectloggings.management.cattle.io -n <cluster-id>
```
