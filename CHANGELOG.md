# Azure Kubernetes Service Changelog

## Release 2025-07-20

Monitor the release status by region at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250720`.

### Announcements
* Kubernetes 1.27 LTS version and 1.30 community version are going out of support by July 30th. Please upgrade to a supported version , refer to [AKS release calendar](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) for more information.
* AKS Kubernetes version 1.33 is now compatible with Long-Term Support (LTS), aligning with [all supported Kubernetes versions are eligible for Long-Term Support (LTS)](https://learn.microsoft.com/azure/aks/long-term-support#will-every-aks-version-support-long-term-support-lts) on AKS.
* The asm-1-23 revision for the Istio add-on has been deprecated. Kindly upgrade your service mesh to a supported version following the [AKS Istio upgrade guide](https://learn.microsoft.com/azure/aks/istio-upgrade).
* [Virtual Machines (VMs) node pools](https://learn.microsoft.com/azure/aks/virtual-machines-node-pools) are now enabled by default when creating a new node pool. Previously [Virtual Machine Scale Sets (VMSS)](https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview) were the default node pool type when creating a node pool in AKS.  To learn more about VMs, an AKS-optimized node pool type, visit our [documentation](https://learn.microsoft.com/azure/aks/virtual-machines-node-pools).
* WASI Node Pool has been [retired](https://azure.microsoft.com/updates?id=484102). If you'd like to run WebAssembly (WASM) workloads, you can [deploy SpinKube to Azure Kubernetes Service (AKS)](https://learn.microsoft.com/azure/aks/deploy-spinkube) from Azure Marketplace.

### Release notes
* Features
  * Application routing add-on now supports configuration of SSL passthrough, custom logging format, and load balancer IP ranges. Review the [configuration of NGINX ingress controller documentation](https://learn.microsoft.com/azure/aks/app-routing-nginx-configuration?tabs=azurecli#configuration-of-the-nginx-ingress-controller) for more information.
  * [SecurityPatch Node OS upgrade](https://learn.microsoft.com/azure/aks/auto-upgrade-node-os-image?tabs=azure-cli) channel is now supported for all network isolated clusters.
  * API server VNet integration is now Generally Available (GA) in additional regions: East Asia, Southeast Asia, Switzerland North, Brazil South, Central India, Germany West Central, and more GA regions. For the complete list of supported regions and any capacity limitations, see the [API Server VNet Integration documentation](https://learn.microsoft.com/azure/aks/api-server-vnet-integration).
  * Kubelet Service Certificate Rotation will begin rollout to all remaining public regions, starting on 23 July 2025. Rollout is expected to be completed in 10 days. Note: This is an estimate and is subject to change. See [GitHub issue](https://github.com/Azure/AKS/issues/4986) for regional updates. Existing node pools will have kubelet serving certificate rotation enabled by default when they perform their first upgrade to any kubernetes version 1.27 or greater. New node pools on kubernetes version 1.27 or greater will have kubelet serving certificate rotation enabled by default. For more information on kubelet serving certificate rotation and disablement, see https://aka.ms/aks/kubelet-serving-certificate-rotation.
  * [Kubernetes Event-Driven Autoscaling (KEDA)](https://learn.microsoft.com/azure/aks/keda-about) is now supported in LTS.
  * [Static Block allocation mode](https://learn.microsoft.com/azure/aks/configure-azure-cni-static-block-allocation) for Azure CNI Networking is now Generally Available.

* Preview Features
  * [Azure Virtual Network Verifier](https://learn.microsoft.com/azure/virtual-network-manager/concept-virtual-network-verifier) is now available in Azure Portal (Node pools blade) for troubleshooting outbound connectivity issues in your AKS cluster. 
  * [Encryption in transit](https://learn.microsoft.com/azure/aks/azure-files-csi#encryption-in-transit-for-nfs-file-shares-preview) is now available for the Azure File CSI driver, starting from AKS version 1.33.
  * [Node auto provisioning metrics](https://learn.microsoft.com/azure/aks/monitor-aks-reference#minimal-ingestion-profile-for-control-plane-metrics-in-managed-prometheus) are now available through [Azure Monitor managed service for Prometheus](https://learn.microsoft.com/azure/azure-monitor/metrics/prometheus-metrics-overview#azure-monitor-managed-service-for-prometheus). To learn more, visit our [node auto provisioning documentation](https://learn.microsoft.com/azure/aks/node-autoprovision#node-auto-provisioning-metrics).
  * [Disable HTTP Proxy](https://aka.ms/aks/http-proxy) is now available in Preview.
 
* Bug Fixes
  * Fixed [issue](https://github.com/Azure/AKS/issues/4720) where AKS evicted pods that had already been manually relocated, causing upgrade failures. This fix adds a node consistency check to ensure the pod is still on the original node before retrying eviction.

* Behavior Changes
  * The [delete-machines API](https://learn.microsoft.com/azure/aks/delete-node-pool?tabs=azure-cli) will only delete machines from the system nodepool if the system addon PDBs are respected.
  * AKS will now reject invalid OsSku enums during cluster creation, node pool creation, and node pool update. Previously AKS would default to `Ubuntu`. Unspecified OsSku with OsType `Linux` will still default to `Ubuntu`. For more information on supported OsSku options, see documentation for [Azure CLI](https://learn.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create) and the [AKS API](https://learn.microsoft.com/rest/api/aks/agent-pools/create-or-update?view=rest-aks-2025-05-01&tabs=HTTP#ossku:~:text=and%20cluster%20upgrades.-,OSSKU,-Enumeration).
  * Application routing component Pods are now annotated with kubernetes.azure.com/set-kube-service-host-fqdn to automatically have the [API server's domain name injected into the pod](https://learn.microsoft.com/azure/aks/outbound-rules-control-egress#required-outbound-network-rules-and-fqdns-for-aks-clusters) instead of the cluster IP, to enable communication to the API server. This is useful in cases where the cluster egress is via a layer 7 firewall.
  * [Container Insights](https://learn.microsoft.com/azure/azure-monitor/containers/container-insights-overview) agents now have a memory limit of 750Mi (down from 4Gi).
  * [Advanced Container Networking Services (ACNS)](https://learn.microsoft.com/azure/aks/advanced-container-networking-services-overview?tabs=cilium) pods now run with priorityClassName: system-node-critical, preventing eviction under node resource pressure and improving cluster security posture.
  * Add node anti-affinity for [FIPS-enabled nodes](https://learn.microsoft.com/azure/aks/enable-fips-nodes) for retina-agent when pod-level metrics are enabled.

* Component Updates
  * Windows node images  
    * Server 2019 Gen1 – [`17763.7558.250714`](vhd-notes/AKSWindows/2019/17763.7558.250714.txt).
    * Server 2022 Gen1/Gen2 – [`20348.3932.250714`](vhd-notes/AKSWindows/2022/20348.3932.250714.txt).
    * Server 23H2 Gen1/Gen2 – [`25398.1732.250714`](vhd-notes/AKSWindows/23H2/25398.1732.250714.txt).
  * AKS Azure Linux v2 image has been updated to [202507.15.0](vhd-notes/AzureLinux/202507.15.0.txt).
  * AKS Azure Linux v3 image has been updated to [202507.15.0](vhd-notes/AzureLinuxv3/202507.15.0.txt).
  * AKS Ubuntu 22.04 node image has been updated to [202507.15.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202507.15.0.txt).
  * AKS Ubuntu 24.04 node image has been updated to [202507.15.0](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202507.15.0.txt).
  * Application Insights addon image is updated to [1.0.0-beta.7](https://github.com/microsoft/Docker-Provider/releases/tag/appmonitoring-1.0.0-beta.7) to expose container port 4000 for scraping Prometheus metrics.
  * Application routing operator is updated to [v0.2.7](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.6) for all supported Kubernetes versions.
  * Azure Network Policy Manager (NPM) image version is updated to [v1.6.29](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.29) to resolve iptables-legacy command issues and bump Ubuntu to 24.04 with CVE fixes.
  * Azure Disk CSI driver versions are upgraded to [v1.31.11](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.31.11), [v1.32.8](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.32.8), [v1.33.2](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.33.2) on AKS versions 1.31, 1.32, 1.33 respectively.
  * Cloud Controller Manager has been upgraded to [v1.33.1](https://github.com/kubernetes/cloud-provider-azure/releases/tag/v1.33.1), [v1.32.6](https://github.com/kubernetes/cloud-provider-azure/releases/tag/v1.32.6), [v1.31.7](https://github.com/kubernetes/cloud-provider-azure/releases/tag/v1.31.7) and [v1.30.13](https://github.com/kubernetes/cloud-provider-azure/releases/tag/v1.30.13).
  * Retina Basic image is updated to [v0.0.36](https://github.com/microsoft/retina/releases/tag/v0.0.36) on Linux and Windows.
  * Retina Enterprise has been updated to [v0.1.11](https://github.com/azure-networking/retina-enterprise/releases/tag/v0.1.11) to resolve several CVEs.
  * [Azure Monitor managed service for Prometheus](https://learn.microsoft.com/azure/azure-monitor/metrics/prometheus-metrics-overview#azure-monitor-managed-service-for-prometheus) addon is updated to the latest release [06-19-2025](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-06-19-2025).
  * Microsoft Defender for Cloud security-publisher image updated to 1.0.243 to address [CVE-2023-4039](https://nvd.nist.gov/vuln/detail/cve-2023-4039) and [CVE-2024-13176](https://nvd.nist.gov/vuln/detail/CVE-2024-13176).
  * Microsoft Defender for Cloud old-file-cleaner image updated to 1.0.243 to address [CVE-2025-0913](https://nvd.nist.gov/vuln/detail/CVE-2025-0913) and [CVE-2025-4673](https://nvd.nist.gov/vuln/detail/CVE-2025-4673).
  * [Image Cleaner](https://learn.microsoft.com/azure/aks/image-cleaner) eraser image is updated to [v1.4.0-4](https://github.com/eraser-dev/eraser/releases/tag/v1.4.0).
  * Bumped Azure Cloud Controller Manager to [v1.33.1](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.33.1), [v1.32.6](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.32.6), [v1.31.7](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.31.7), and [v1.30.13](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.30.13).
  * Tigera operator is updated from v.1.38.0 to [v1.38.2](https://github.com/tigera/operator/releases/tag/v1.38.2) to support Calico [v3.30.1](https://github.com/projectcalico/calico/blob/release-v3.30/release-notes/v3.30.1-release-notes.md).
  * Calico has been upgraded with the [v3.30.2](https://github.com/projectcalico/calico/blob/release-v3.30/release-notes/v3.30.2-release-notes.md).
  * Vertical Pod Autoscaler (VPA) addon images are now built with [Dalec](https://github.com/Azure/dalec) starting from AKS version 1.27.
  * Cluster Autoscaler is upgraded to [v1.33.0](https://github.com/kubernetes/autoscaler/releases/tag/cluster-autoscaler-1.33.0) with Dalec-built image.
  * Azure Policy Addon is upgraded to [v1.13.0](https://github.com/Azure/azure-policy/releases) with enhanced EUDB request routing.
  * `secrets-store-csi-driver` is upgraded to [v1.5.1](https://github.com/kubernetes-sigs/secrets-store-csi-driver/releases/tag/v1.5.1)
  * [Workload identity](https://learn.microsoft.com/azure/aks/workload-identity-overview?tabs=dotnet) image is updated to [v1.5.1](https://github.com/Azure/azure-workload-identity/releases/tag/v1.5.1) with CVE fixes.
  * Istio revision [asm-1-26](https://istio.io/latest/news/releases/1.26.x/announcing-1.26/) is now available for the Istio-based service mesh add-on. To adopt the new revision, follow the [canary upgrade guidance](https://learn.microsoft.com/azure/aks/istio-upgrade). Other updates:
  * [Istio-based service mesh add-on](https://learn.microsoft.com/azure/aks/istio-about) now supports the following [annotations](https://learn.microsoft.com/azure/aks/istio-deploy-ingress#ingress-gateway-service-customizations): `service.beta.kubernetes.io/azure-allowed-ip-ranges`, `service.beta.kubernetes.io/azure-load-balancer-disable-tcp-reset`, `service.beta.kubernetes.io/azure-pip-ip-tags`, `service.beta.kubernetes.io/azure-load-balancer-tcp-idle-timeout` for Istio ingress gateways.

## Release 2025-06-17

Monitor the release status by region at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250617`.

### Announcements

* Kubernetes 1.27 LTS version and 1.30 community version are going out of support by July 30th. Please upgrade to a supported version , refer to [AKS release calendar](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) for more information.
* Kubernetes 1.33 community version is going to be released before end of June 2025, please keep track of the rollout by regions via [AKS Release tracker](https://releases.aks.azure.com/).
* Customers using Azure Linux 2.0 should migrate to Azure Linux 3.0 before November 2025. For details on how to migrate from Azure Linux 2.0 to Azure Linux 3.0, see this [doc](https://learn.microsoft.com/azure/azure-linux/how-to-enable-azure-linux-3). AKS is currently working on a feature to allow for migrations between Azure Linux 2.0 and Azure Linux 3.0 through a node pool update command. For updates on feature progress and availability, see Github [issue](https://github.com/Azure/AKS/issues/4987).
* Starting in June 2025, AKS clusters with version >= 1.28 and using Azure Linux 2.0 can be opted into [Long Term Support](https://learn.microsoft.com/azure/aks/long-term-support). See [blog](https://techcommunity.microsoft.com/blog/linuxandopensourceblog/azure-linux-now-supports-aks-long-term-support-lts-starting-with-kubernetes-v1-2/4424826) for more information.
* Starting in June 2025, Azure Kubernetes Service will begin rolling out a change to enable quota for all current and new AKS customers. AKS quota will represent a limit of the maximum number of managed clusters that an Azure subscription can consume per region. Existing AKS customer subscriptions will be given a quota limit at or above their current usage, depending on region availability. Once quota is enabled, customers can view their available quota and request quota increases in the Quotas page in the Azure Portal or by using the Quotas REST API. For details on how to view and request quota increases via the Portal Quotas page, visit [Azure Quotas](https://learn.microsoft.com/azure/quotas/view-quotas). For details on how to view and request quota increases via the Quotas REST API, visit: [Azure Quota REST API Reference](https://learn.microsoft.com/rest/api/reserved-vm-instances/quotaapi). New AKS customer subscriptions will be given a default limit upon new subscription creation. More information on the default limits for new subscriptions is available in documentation [here](https://learn.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).
* [Ubuntu 18.04](https://github.com/Azure/AKS/issues/4873) is no longer supported on AKS. AKS will no longer create new node images or provide security updates for Ubuntu 18.04 nodes. Existing node images will be deleted by 17 July 2025. Scaling operations will fail. To avoid service disruptions, scaling restrictions, and remain supported, please follow our instructions to [upgrade](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) to a [supported Kubernetes version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions).
* [Teleport (preview)](https://github.com/Azure/acr/blob/main/docs/teleport/aks-getting-started.md) on AKS will be retired on 15 July 2025, please [migrate to Artifact Streaming (preview) on AKS](https://learn.microsoft.com/azure/aks/artifact-streaming) or update your node pools to set --aks-custom-headers EnableACRTeleport=false. Azure Container Registry has removed the Teleport API meaning that any nodes with Teleport enabled are pulling images from Azure Container Registry as any other AKS node. After 15 July 2025, any node pools with Teleport (preview) enabled may experience breakage and node provisioning failures. For more information, see [aka.ms/aks/teleport-retirement](https://aka.ms/aks/teleport-retirement).
Azure Kubernetes Service will no longer support the --skip-gpu-driver-install node pool tag to skip automatic driver installation. Starting on August 14 2025, you will no longer be able to use this node pool tag at AKS node pool creation time to install custom GPU drivers or use the GPU Operator. Alternatively, you should use the generally available gpu-driver API field to update your existing node pools or create new GPU-enabled node pools to [skip automatic GPU driver installation](https://learn.microsoft.com/azure/aks/gpu-cluster?tabs=add-ubuntu-gpu-node-pool#skip-gpu-driver-installation).

### Release Notes

* Preview Features
  * [Azure Monitor Application Insights for Azure Kubernetes Service (AKS) workloads](https://learn.microsoft.com/azure/azure-monitor/app/kubernetes-codeless) is now available in preview.
  * Ubuntu 24.04 is now available in public preview in k8s 1.32+. ContainerD 2.0 is enabled by default. You can create new Ubuntu 24.04 node pools or update existing Linux node pools to Ubuntu 24.04. Use the "Ubuntu2404" os sku enum after registering the preview flag "Ubuntu2404Preview". You can also use the new "Ubuntu2204" os sku enum to rollback to Ubuntu 22.04 if you encounter any issues! You may also rollback using "Ubuntu" os sku enum. For more information, see [upgrading your OS version](https://learn.microsoft.com/azure/aks/upgrade-os-version?tabs=testing-a-new-os-version%2Croll-back-your-os-version).
  * [Cost optimized add-on scaling](https://learn.microsoft.com/azure/aks/optimized-addon-scaling) is now available in preview. This feature allows you to autoscale supported addons or customize the resource's default CPU/ memory requests and limits to improve cost savings. 
  
* Features  
  * AKS patch versions 1.32.5, 1.31.9 are now available. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.
  * API Server VNet Integration is available now, please find the [most up to date regions](https://learn.microsoft.com/azure/aks/api-server-vnet-integration#limited-availability) where this feature has been rolled out.
  * Kubelet Service Certificate Rotation has now been rolled out to East US and UK South. Existing node pools will have kubelet serving certificate rotation enabled by default when they perform their first upgrade to any kubernetes version 1.27 or greater. New node pools on kubernetes version 1.27 or greater will have kubelet serving certificate rotation enabled by default. For more information on kubelet serving certificate rotation and disablement, see [certificate rotation in Azure Kubernetes Service](https://learn.microsoft.com/azure/aks/certificate-rotation).  
  * MaxblockedNodes property is getting rolled to all regions. This helps cluster operators to put a limit on number of nodes that can be blocked on pdb blocked eviction failures and continuing upgrade forward. [Read more](https://learn.microsoft.com/azure/aks/upgrade-cluster#scenario-2-node-drain-failures-and-pdbs).
  
* Bug Fixes
  * Fixed a race condition with streams sharing data between Cilium agent and ACNS security agent.
  * Fixed Azure Policy addon Gatekeeper regression causing crash loop on clusters with Kubernetes versions < 1.27.
  * Resolved an issue where node pool scaling failed with customized kubelet configuration. Without this fix, node pools using CustomKubeletConfigs could not be scaled, and encountered an error message stating that the CustomKubeletConfig or CustomLinuxOSConfig cannot be changed for the scaling operation.
  * Fixed an [issue](https://github.com/kubernetes-sigs/cloud-provider-azure/issues/8649) where updating node pools with the exclude label, it doesn't update the Load Balancer Backend Pool properly.
  * Resolved a problem when upgrading Kubenet or Nodesubnet cluster with AGIC enabled to Azure CNI Overlay there might be some connectivity issues to services exposed via Ingress App Gateway public IP.
  * Fixed a bug where clusters with Node Auto Provisioning enabled could intermittently get an error about "multiple identities configured" and be unable to authenticate with Azure.
  * Fixed an issure to ensure the vms in a specific cloud are compatible with the latest Windows 550 grid driver.
    

* Behavior Changes  
  * AKS now allows [daily schedules](https://learn.microsoft.com/azure/aks/planned-maintenance?tabs=azure-cli#schedule-types) for the auto upgrade configuration.
  * [Static Egress Gateway](https://learn.microsoft.com/azure/aks/configure-static-egress-gateway) memory limits increased from 500Mi to 3000Mi reducing the risk of memory-related restarts under load.
  * The GPU provisioner component of [KAITO](https://learn.microsoft.com/azure/aks/ai-toolchain-operator) has now been moved to the AKS control plane when the KAITO add-on is used. The OSS installation will still require this component to run on the kubernetes nodes.
  * [Azure Monitor managed service for Prometheus](https://learn.microsoft.com/azure/azure-monitor/essentials/prometheus-metrics-overview) updates the max shards from 12 to 24, ensuring enhanced scaling capabilities.
  * `linuxutil plugin` is enabled again for [Retina Basic and ACNS](https://retina.sh/docs/Metrics/plugins/Linux/linuxutil).
  * Node Auto-Provisioning (NAP) now requires Kubernetes RBAC to be enabled, because NAP relies on secure and scoped access to Kubernetes resources to provision nodes based on pending pod resource requests. Kubernetes RBAC is enabled by default. For more information, see [RBAC for Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).
  * [Deployment Safeguards](https://learn.microsoft.com/azure/aks/deployment-safeguards) no longer requires Azure Policy permissions. Cluster admins will have the ability to turn on and disable Deployment Safeguards.

* Component Updates  
  * Windows node images  
    * Server 2019 Gen1 – [`17763.7314.250518`](vhd-notes/AKSWindows/2019/17763.7314.250518.txt).
    * Server 2022 Gen1/Gen2 – [`20348.3692.250518`](vhd-notes/AKSWindows/2022/20348.3692.250518.txt)
    * Server 23H2 Gen1/Gen2 – [`25398.1611.250518`](vhd-notes/AKSWindows/23H2/25398.1611.250518.txt)
  * AKS Azure Linux v2 image has been updated to [202506.12.0](vhd-notes/AzureLinux/202506.12.0.txt).
  * AKS Azure Linux v3 image has been updated to [202506.12.0](vhd-notes/AzureLinuxv3/202506.12.0.txt).
  * AKS Ubuntu 22.04 node image has been updated to [202506.12.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202506.12.0.txt).
  * AKS Ubuntu 24.04 node image has been updated to [202506.12.0](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202506.12.0.txt).
  * Updated Istio-based service mesh add-on to versions [v1.23.6](https://istio.io/latest/news/releases/1.23.x/announcing-1.23.6/), [v1.24.6](https://istio.io/latest/news/releases/1.24.x/announcing-1.24.6/), [v1.25.3](https://istio.io/latest/news/releases/1.25.x/announcing-1.25.3/).
  * Updated Secret Store CSI driver to [v1.5.0](https://github.com/kubernetes-sigs/secrets-store-csi-driver/releases/tag/v1.5.0) and the Azure provider to [v1.7.0](https://github.com/Azure/secrets-store-csi-driver-provider-azure/releases/tag/v1.7.0).
  * Updated tigera operator to [v1.36.10](https://github.com/tigera/operator/releases/tag/v1.36.10) and calico to [v3.29.4](https://github.com/projectcalico/calico/releases/tag/v3.29.4) for versions running on K8S 1.32. Updated tigera operator to [v1.38.0](https://github.com/tigera/operator/releases/tag/v1.38.0) and calico to [v3.30.0](https://github.com/projectcalico/calico/releases/tag/v3.30.0) for versions running on K8S 1.33.
  * Updated KEDA to [v2.17](https://github.com/kedacore/keda/releases/tag/v2.17.0) for AKS clusters running on Kubernetes 1.33.
  * Updated Azure Disk CSI driver to [v1.32.7](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.32.7) for AKS 1.32, [v1.33.1](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.33.1) for AKS 1.33.
  * Updated Azure File CSI driver to [v1.30.10](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.10) for AKS 1.30, [v1.31.6](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.31.6) for AKS 1.31, [v1.32.4](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.32.4) for AKS 1.32, [v1.33.1](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.33.1) for AKS 1.33.
  * Updated Azure Blob CSI driver [v1.24.10](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.24.10) for AKS 1.30, [v1.25.8](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.25.8) for AKS 1.31, [v1.26.5](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.26.5) for AKS 1.32, AKS 1.33.
  * Updated Azure Network Policy Manager (NPM) to [v1.5.48](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.48).
  * Updated Azure Monitor managed service for Prometheus images to [05-29-2025 release](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-05-29-2025).
  * Updated Azure Policy add-on image to [v1.12.2](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#1122).
  * Updated Retina basic image to [v0.0.34](https://github.com/microsoft/retina/releases/tag/v0.0.34).
  * Updated Retina Enteprise image to [v0.1.10](https://github.com/azure-networking/retina-enterprise/releases/tag/v0.1.10).
  * Updated Metrics-server to [v0.7.2-7](https://github.com/aks-lts/metrics-server/releases/tag/v0.7.2-7) for AKS clusters on 1.32+ version, [v0.6.3-6](https://github.com/aks-lts/metrics-server/releases/tag/v0.6.3-6) for AKS clusters on 1.24+ version.
  * Updated coredns version to [v1.9.4-6](https://github.com/aks-lts/coredns/releases/tag/v1.9.4-6) for AKS clusters on 1.24+ version, [v1.11.3-8](https://github.com/aks-lts/coredns/releases/tag/v1.11.3-8) for AKS clusters on 1.32+ version, [v1.12.1-2](https://github.com/aks-lts/coredns/releases/tag/v1.12.1-2) for AKS clusters on 1.33+ version.
  * Released an update in GRID driver version on the guest VM for [NVs_v3](https://learn.microsoft.com/azure/virtual-machines/windows/n-series-driver-setup#nvidia-gridvgpu-drivers) VM family to ensure compatibility with host VM GPU driver version for Windows GPU node pools.
  * Updated cilium/fqdn images to v1.14.20-1 for Kubernetes 1.29, [v1.16.10](https://github.com/cilium/cilium/releases/tag/v1.16.10) for Kubernetes 1.31, [v1.17.4](https://github.com/cilium/cilium/releases/tag/v1.17.4) for Kubernetes 1.32 to fix [CVE‑2025‑22871](https://nvd.nist.gov/vuln/detail/CVE-2025-22871) , [CVE‑2025‑22872](https://nvd.nist.gov/vuln/detail/CVE-2025-22872) , [CVE‑2024‑45336](https://nvd.nist.gov/vuln/detail/CVE-2024-45336) , [CVE‑2024‑45341](https://nvd.nist.gov/vuln/detail/CVE-2024-45341) , [CVE‑2025‑22866](https://nvd.nist.gov/vuln/detail/CVE-2025-22866) , [CVE‑2025‑43973](https://nvd.nist.gov/vuln/detail/CVE-2025-43973) , [CVE‑2025‑43970](https://nvd.nist.gov/vuln/detail/CVE-2025-43970) , [CVE‑2025‑43972](https://nvd.nist.gov/vuln/detail/CVE-2025-43972) , [CVE‑2025‑43971](https://nvd.nist.gov/vuln/detail/CVE-2025-43971) , [CVE‑2025‑32793](https://nvd.nist.gov/vuln/detail/CVE-2025-32793).


## Release 2025-05-19

Monitor the release status by region at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250519`.

### Announcements

* Customers using Azure Linux 2.0 should migrate to Azure Linux 3.0 before November 2025. For details on how to migrate from Azure Linux 2.0 to Azure Linux 3.0, see this [doc](https://learn.microsoft.com/azure/azure-linux/how-to-enable-azure-linux-3). AKS is currently working on a feature to allow for migrations between Azure Linux 2.0 and Azure Linux 3.0 through a node pool update command. For updates on feature progress and availability, see Github [issue](https://github.com/Azure/AKS/issues/4987).
* Starting in June 2025, AKS clusters with version >= 1.28 and using Azure Linux 2.0 can be opted into [Long Term Support](https://learn.microsoft.com/azure/aks/long-term-support).
* Starting in June 2025, Azure Kubernetes Service will begin rolling out a change to enable quota for all current and new AKS customers. AKS quota will represent a limit of the maximum number of managed clusters that an Azure subscription can consume per region. Existing AKS customer subscriptions will be given a quota limit at or above their current usage, depending on region availability. Once quota is enabled, customers can view their available quota and request quota increases in the Quotas page in the Azure Portal or by using the Quotas REST API. For details on how to view and request quota increases via the Portal Quotas page, visit [Azure Quotas](https://learn.microsoft.com/azure/quotas/view-quotas). For details on how to view and request quota increases via the Quotas REST API, visit: [Azure Quota REST API Reference](https://learn.microsoft.com/rest/api/reserved-vm-instances/quotaapi). New AKS customer subscriptions will be given a default limit upon new subscription creation. More information on the default limits for new subscriptions is available in documentation [here](https://learn.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).
* Starting on 17 June 2025, AKS will no longer create new node images for [Ubuntu 18.04](https://github.com/Azure/AKS/issues/4873) or provide security updates. Existing node images will be deleted. Your node pools will be unsupported and you will no longer be able to scale. To avoid service disruptions, scaling restrictions, and remain supported, please follow our instructions to [upgrade](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) to a [supported Kubernetes version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions).
* [Teleport (preview)](https://github.com/Azure/acr/blob/main/docs/teleport/aks-getting-started.md) on AKS will be retired on 15 July 2025, please [migrate to Artifact Streaming (preview) on AKS](https://learn.microsoft.com/azure/aks/artifact-streaming) or update your node pools to set --aks-custom-headers EnableACRTeleport=false. Azure Container Registry has removed the Teleport API meaning that any nodes with Teleport enabled are pulling images from Azure Container Registry as any other AKS node. After 15 July 2025, any node pools with Teleport (preview) enabled may experience breakage and node provisioning failures. For more information, see [aka.ms/aks/teleport-retirement](https://aka.ms/aks/teleport-retirement).
Azure Kubernetes Service will no longer support the --skip-gpu-driver-install node pool tag to skip automatic driver installation. Starting on August 14 2025, you will no longer be able to use this node pool tag at AKS node pool creation time to install custom GPU drivers or use the GPU Operator. Alternatively, you should use the generally available gpu-driver API field to update your existing node pools or create new GPU-enabled node pools to [skip automatic GPU driver installation](https://learn.microsoft.com/azure/aks/gpu-cluster?tabs=add-ubuntu-gpu-node-pool#skip-gpu-driver-installation-preview).

### Release Notes

* Preview Features
  * For clusters with [Layer 7 policies with Advanced Container Networking Services](https://learn.microsoft.com/azure/aks/how-to-apply-l7-policies?tabs=cilium) enabled, a new metric [proxy_datapath_update_timeout_total](https://docs.cilium.io/en/stable/observability/metrics/#policy-l7-http-kafka-fqdn) has been introduced in disabled by default mode.

* Features  
  * Kubernetes 1.31 and 1.32 are now designated as [Long-Term Support (LTS)](https://learn.microsoft.com/azure/aks/long-term-support) versions.  
  * [Kubernetes 1.33](https://kubernetes.io/blog/2025/04/23/kubernetes-v1-33-release/) is available in Preview. A full matrix of supported add-ons and components is published at the [AKS versions page](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-1330).
  * AKS now allows [upgrading](https://learn.microsoft.com/azure/aks/upgrade-azure-cni#node-subnet-cluster-upgrade) from Azure CNI NodeSubnet to Azure CNI NodeSubnet with Cilium dataplane, and from Azure CNI NodeSubnet with Cilium dataplane to Azure CNI Overlay with Cilium dataplane.
  
* Bug Fixes  
  * Fixed failures triggered by duplicate [tag keys](https://learn.microsoft.com/azure/aks/use-tags) that differed only by character case.

* Behavior Changes  
  * Static egress gateway memory limits increased from 128Mi to 500Mi for greater stability.
  * Memory for Azure Monitor Container Insights container `ama-logs` increased from `750Mi` to `1Gi`.
  * Timeouts due to FQDN IP updates are exported by Cilium Agent as [cilium_proxy_datapath_update_timeout_total](https://docs.cilium.io/en/stable/observability/metrics/#policy-l7-http-kafka-fqdn) on Azure CNI Powered by Cilium.
* Component Updates  
  * Updated [Node Auto Provisioning](https://learn.microsoft.com/azure/aks/node-autoprovision?tabs=azure-cli) to use karpenter-provider-azure release [v1.4.0](https://github.com/Azure/karpenter-provider-azure/releases/tag/v1.4.0).
  * Updated [Azure Monitor Container Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-overview) images to [3.1.27](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#05082025--) for both Linux and Windows.
  * Updated the Windows GPU device-plugin to `v0.0.19`, mitigating [CVE-2025-22871](https://nvd.nist.gov/vuln/detail/CVE-2025-22871).  
  * Windows node images  
    * Server 2019 Gen1 – [`17763.7240.250416`](vhd-notes/AKSWindows/2019/17763.7240.250416.txt)
    * Server 2022 Gen1/Gen2 – [`20348.3561.250416`](vhd-notes/AKSWindows/2022/20348.3561.250416.txt)
    * Server 23H2 Gen1/Gen2 – [`25398.1551.250416`](vhd-notes/AKSWindows/23H2/25398.1551.250416.txt)
  * AKS Azure Linux v2 image has been updated to [202505.14.0](vhd-notes/AzureLinux/202505.14.0.txt).
  * AKS Azure Linux v3 image has been updated to [202505.14.0](vhd-notes/AzureLinuxv3/202505.14.0.txt).
  * AKS Ubuntu 22.04 node image has been updated to [202505.14.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202505.14.0.txt).
  * AKS Ubuntu 24.04 node image has been updated to [202505.14.0](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202505.14.0.txt).
  * Azure Disk CSI driver updated to versions [v1.30.12](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.30.12), [v1.31.9](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.31.9), [v1.32.5](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.32.5) on AKS versions 1.30, 1.31, and 1.32 respectively.
  * Azure Blob CSI driver updated to versions [v1.25.6](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.25.6) and [v1.26.3](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.26.3) on AKS versions 1.31 and 1.32 respectively
  * Azure File CSI driver [v1.32.2](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.32.2) for AKS 1.32  
  * Updated cloud-node-manager to [v1.32.5](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.32.5)
  * Updated cloud-controller-manager to [v1.31.6](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.31.6)
  * Updated acr-credential-provider to [v1.29.15](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.29.15)
  * Static egress gateway images updated to [v0.0.21](https://github.com/Azure/kube-egress-gateway/releases/tag/v0.0.21).  
  * Updated Azure Policy add-on image to [v1.11](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#1111). Gatekeeper updated to [v3.19.1](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.19.1).  
  
## Release 2025-04-27

Monitor the release status by region at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250427`.

### Announcements
* AKS supported Kubernetes version release updates are now available in [AKS Release Tracker](https://releases.aks.azure.com/). You can check current in-support Kubernetes versions and LTS versions for specific region and track new patches version release progress with Release Tracker.
* Customers using AzureLinux 2.0 should migrate to Azure Linux 3.0 before November 2025. For details on how to migrate from Azure Linux 2.0 to Azure Linux 3.0, see this [doc](https://learn.microsoft.com/azure/azure-linux/how-to-enable-azure-linux-3). AKS is currently working on a feature to allow for migrations between Azure Linux 2.0 and Azure Linux 3.0 through a node pool update command. For updates on feature progress and availability, see Github [issue](https://github.com/Azure/AKS/issues/4987).
* AKS now requires a minimum of 2GBs of memory for the SKU for all user nodepools. To learn more, see [aka.ms/aks/restrictedSKUs](https://aka.ms/aks/restrictedSKUs).
* Starting on 5 May, 2025, [WebAssembly System Interface (WASI) node pools](https://github.com/Azure/AKS/issues/4770) will no longer be supported. You can no longer create WASI (preview) node pools, and existing WASI node pools will be unsupported. 
* Starting in June 2025, Azure Kubernetes Service will begin rolling out a change to enable quota for all current and new AKS customers. AKS quota will represent a limit of the maximum number of managed clusters that an Azure subscription can consume per region. Existing AKS customer subscriptions will be given a quota limit at or above their current usage, depending on region availability. Once quota is enabled, customers can view their available quota and request quota increases in the Quotas page in the Azure Portal or by using the Quotas REST API. For details on how to view and request quota increases via the Portal Quotas page, visit [Azure Quotas](https://learn.microsoft.com/azure/quotas/view-quotas). For details on how to view and request quota increases via the Quotas REST API, visit: [Azure Quota REST API Reference](https://learn.microsoft.com/rest/api/reserved-vm-instances/quotaapi). New AKS customer subscriptions will be given a default limit upon new subscription creation. More information on the default limits for new subscriptions is available in documentation [here](https://learn.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).
* As of 31 March 2025, AKS no longer allows new cluster creation with the Basic Load Balancer. On 30 September 2025, the Basic Load Balancer will be retired. We will be posting updates on migration paths to the Standard Load Balancer. See [AKS Basic LB Migration Issue](https://github.com/Azure/AKS/issues/1687) for updates on when a simplified upgrade path is available. Refer to [Basic Load Balancer Deprecation Update](https://azure.microsoft.com/updates?id=azure-basic-load-balancer-will-be-retired-on-30-september-2025-upgrade-to-standard-load-balancer) for more information.
* The asm-1-22 revision for the Istio-based service mesh add-on has been deprecated. Migrate to a supported revision following the [AKS Istio upgrade guide](https://learn.microsoft.com/azure/aks/istio-upgrade).
* Starting on 17 June 2025, AKS will no longer create new node images for [Ubuntu 18.04](https://github.com/Azure/AKS/issues/4873) or provide security updates. Existing node images will be deleted. Your node pools will be unsupported and you will no longer be able to scale. To avoid service disruptions, scaling restrictions, and remain supported, please follow our instructions to [upgrade](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) to a [supported Kubernetes version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions).
* [Teleport (preview)](https://github.com/Azure/acr/blob/main/docs/teleport/aks-getting-started.md) on AKS will be retired on 15 July 2025, please [migrate to Artifact Streaming (preview) on AKS](https://learn.microsoft.com/azure/aks/artifact-streaming) or update your node pools to set --aks-custom-headers EnableACRTeleport=false. Azure Container Registry has removed the Teleport API meaning that any nodes with Teleport enabled are pulling images from Azure Container Registry as any other AKS node. After 15 July 2025, any node pools with Teleport (preview) enabled may experience breakage and node provisioning failures. For more information, see [aka.ms/aks/teleport-retirement](https://aka.ms/aks/teleport-retirement).

### Release Notes
* Features:
  * [Network isolated cluster](https://learn.microsoft.com/azure/aks/concepts-network-isolated) with outbound type `none` is now Generally Available.
  * [AKS Security Bulletin](https://learn.microsoft.com/azure/aks/security-bulletins/overview?tabs=aks-addons%2Caks-cluster%2Caks-node-image) and [AKS CVE Mitigation Status](https://releases.aks.azure.com/webpage/index.html) are now available to track Security and CVE mitigations.
* Preview Features:
  * [Kubernetes 1.33](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.33.md) version is now available for Preview, see [Release tracker](https://releases.aks.azure.com/) for when it hits your region.
  * Kubernetes 1.31 and 1.32 are now recognized as Long-Term Support (LTS) releases in AKS, joining existing LTS versions 1.28 and 1.29. You can view when these LTS releases hit your region in real time via the [Release tracker](https://releases.aks.azure.com/). For more information, see [Long Term Support (LTS)](https://learn.microsoft.com/azure/aks/long-term-support).

* Bug Fixes:
  * Fix an [issue](https://github.com/azure-networking/cilium-private/pull/465) in [Azure CNI Powered by Cilium](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium) to improves DNS request/response performance, especially in large scale clusters using FQDN based policies. Without this fix, if the user sets a DNS request timeout below 2 seconds, in high-scale scenarios they may experience request drops due to duplicate request IDs.
  * Fix an issue where [load balancer](https://learn.microsoft.com/azure/aks/load-balancer-standard) tags were not updated after accluster tag update. Load balancer tags now correctly reflect the latest state.
  * Fix an [issue](https://github.com/cilium/cilium/issues/38210) in Cilium v1.17 where a deadlock was causing server pods to be unable to start. 

* Behavior Changes:
  * `aksmanagedap` is blocked as a reserved name for AKS system component, you can no longer use it for creating agent pool. See [naming convention](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/create-upgrade-delete/aks-common-issues-faq#what-naming-restrictions-are-enforced-for-aks-resources-and-parameters-) for more information.
  * `linuxutil plugin` is temporarily disabled for [Retina Basic and ACNS](https://retina.sh/docs/Metrics/plugins/Linux/linuxutil) as it was causing memory leaks that leads to Retina pods OOMKill.
  * [Advanced Container Networking Services](https://learn.microsoft.com/azure/aks/advanced-container-networking-services-overview) (ACNS) configmaps (`cilium`, `retina`, `hubble`) now auto‑format cluster names to satisfy Cilium 1.17 rules:≤ 32 chars, lowercase alphanumeric characters and dashes, no leading/trailing dashes, functionality is unaffected. This change is due to the strict enforcement of Cilium 1.17. See this [link](https://github.com/cilium/cilium/commit/b1f10bcb26d71f046d8ad108d5f4b9e42aa5d486) for details.
  * The `defaultConfig.gatewayTopology` field is now included in the Istio add-on `MeshConfig` AllowList as an unsupported field. For more details, see the [Istio MeshConfig documentation](https://learn.microsoft.com/azure/aks/istio-meshconfig#allowed-supported-and-blocked-meshconfig-values).
  * Previously, you can't disable Node AutoProvisioning once enabled, now you can if meet certain criteria. See this [document](https://learn.microsoft.com/azure/aks/node-autoprovision) for more details.
  * [Disabling kube-proxy](https://learn.microsoft.com/azure/aks/configure-kube-proxy) no longer requires the `KubeProxyConfigurationPreview` feature flag in bring-your-own (BYO) CNI scenarios.
  * [Kubelet Service Certificate Rotation](https://learn.microsoft.com/azure/aks/certificate-rotation) will begin regional rollout, starting with westcentralus and eastasia by 16 May 2025. Existing node pools in these regions will have kubelet serving certificate rotation enabled by default when they perform their first upgrade to any kubernetes version 1.27 or greater. New node pools in these regions on kubernetes version 1.27 or greater will have kubelet serving certificate rotation enabled by default. For more information on kubelet serving certificate rotation, see [aka.ms/aks/kubelet-serving-certificate-rotation](https://aka.ms/aks/kubelet-serving-certificate-rotation).
  * Starting with Kubernetes 1.33, the default Kubernetes Scheduler is configured to use a `MaxSkew` value of 1 for `topology.kubernetes.io/zone`. For more details see [Ensure pods are spread across AZs](https://learn.microsoft.com/en-us/azure/aks/aks-zone-resiliency#ensure-pods-are-spread-across-azs)

* Component Updates:
  * Fleet networking components updated to [v0.39](https://github.com/Azure/fleet-networking/releases/tag/v0.3.9) from v0.38 to fix CVE.
  * [Workload Identity](https://learn.microsoft.com/azure/aks/workload-identity-deploy-cluster) updated from v1.4.0 to [v1.5.0](https://github.com/Azure/azure-workload-identity/releases/tag/v1.5.0)
  * App-routing-operator has been upgraded to [v0.2.5](https://github.com/Azure/aks-app-routing-operator/blob/main/CHANGELOG.md#025---2025-04-22) on all supported AKS versions.
  * Cost-analysis-agent and cost-analysis-scraper image updated from v0.0.22 to 0.0.23 to fix [CVE-2025-22871](https://pkg.go.dev/vuln/GO-2025-3563).
  * Cloud-controller-manager updated to [v1.32.4](https://cloud-provider-azure.sigs.k8s.io/blog/2025/04/21/v1.32.4/), [v1.31.5](https://cloud-provider-azure.sigs.k8s.io/blog/2025/04/21/v1.31.5/), [v1.30.11](https://cloud-provider-azure.sigs.k8s.io/blog/2025/04/21/v1.30.11/) and [v1.29.14](https://cloud-provider-azure.sigs.k8s.io/blog/2025/04/21/v1.29.14/).
  * [Promtheus collector](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-04-15-2025) for Azure Monitor managed service for Prometheus addon updated from 6.15.0-main-02-21-2025-4acb2b4c to 6.16.0-main-04-15-2025-d78050c6.
  * Retina Enterprise image updated to [v0.1.9](https://github.com/azure-networking/retina-enterprise/releases/tag/v0.1.9) to resolve issues with retina-operator container image build on ARM64 nodes.
  * Retina base image to [v0.0.30](https://github.com/microsoft/retina/releases/tag/v0.0.30) for both Linux and Windows to resolve security vulnerabilities [CVE-2024-40635](https://nvd.nist.gov/vuln/detail/CVE-2024-40635) , [CVE-2025-30162](https://nvd.nist.gov/vuln/detail/CVE-2025-30162) and [CVE-2025-22870](https://nvd.nist.gov/vuln/detail/CVE-2025-22870).
  * Calico CNI/Tigera Operator updated to [v3.28.4](https://github.com/projectcalico/calico/releases/tag/v3.28.4) (operator v1.34.10) and [v3.29.3](https://github.com/projectcalico/calico/releases/tag/v3.29.3) (operator v1.36.7), fixing [CVE-2025-0395](https://nvd.nist.gov/vuln/detail/CVE-2025-0395), [CVE-2025-22869](https://nvd.nist.gov/vuln/detail/CVE-2025-22869), [CVE-2025-22872](https://nvd.nist.gov/vuln/detail/CVE-2025-22872), [CVE-2024-9042](https://nvd.nist.gov/vuln/detail/CVE-2024-9042), [CVE-2025-0426](https://nvd.nist.gov/vuln/detail/CVE-2025-0426) and [CVE-2025-22871](https://nvd.nist.gov/vuln/detail/CVE-2025-22871).
  * Cilium image updated to patch version v1.13.18-250409 on AKS 1.27 and 1.28 to fix [CVE-2024-45336](https://avd.aquasec.com/nvd/cve-2024-45336), [CVE-2024-45341](https://avd.aquasec.com/nvd/cve-2024-45341), [CVE-2025-22866](https://avd.aquasec.com/nvd/cve-2025-22866), [CVE-2025-22870](https://avd.aquasec.com/nvd/cve-2025-22870), [CVE-2024-45337]( https://avd.aquasec.com/nvd/cve-2024-45337).
  * Updated [Istio-based service mesh add-on](https://learn.microsoft.com/azure/aks/istio-upgrade) revisions asm-1-23, asm-1-24, and asm-1-25 to patch [v1.23.6](https://istio.io/latest/news/releases/1.23.x/announcing-1.23.6), [v1.24.5](https://istio.io/latest/news/releases/1.24.x/announcing-1.24.5) and [1.25.2](https://istio.io/latest/news/releases/1.25.x/announcing-1.25.2). asm-1-24 support has now been extended to include AKS 1.33.
  * Azure Disk CSI driver version updated to [v1.32.4](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.32.4) on AKS 1.32, [v1.31.8](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.31.8) on AKS 1.31.
  * Coredns updated to v1.12.1-1 on AKS 1.33.
  * AKS Azure Linux v2 image has been updated to [202504.27.0](vhd-notes/AzureLinux/202504.27.0.txt).
  * AKS Azure Linux v3 image has been updated to [202504.27.0](vhd-notes/AzureLinuxv3/202504.27.0.txt).
  * AKS Ubuntu 22.04 node image has been updated to [202504.27.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202504.27.0.txt).
  * AKS Ubuntu 24.04 node image has been updated to [202504.27.00](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202504.27.0.txt).

## Release 2025-04-06

Monitor the release status by region at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250406`.

### Announcements
* Starting in May 2025, Azure Kubernetes Service will begin rolling out a change to enable quota for all current and new AKS customers. AKS quota will represent a limit of the maximum number of managed clusters that an Azure subscription can consume per region. Existing AKS customer subscriptions will be given a quota limit at or above their current usage, depending on region availability. Once quota is enabled, customers can view their available quota and request quota increases in the Quotas page in the Azure Portal or by using the Quotas REST API. For details on how to view and request quota increases via the Portal Quotas page, visit [Azure Quotas](https://learn.microsoft.com/azure/quotas/view-quotas). For details on how to view and request quota increases via the Quotas REST API, visit: [Azure Quota REST API Reference](https://learn.microsoft.com/rest/api/reserved-vm-instances/quotaapi). New AKS customer subscriptions will be given a default limit upon new subscription creation. More information on the default limits for new subscriptions is available in documentation [here](https://learn.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).
* AKS Kubernetes version 1.32 roll out has been delayed and is now expected to reach all regions on or before the end of April. Please use the [az-aks-get-versions](https://learn.microsoft.com/azure/aks?view=azure-cli-latest#az-aks-get-versions) command to accurately capture if Kubernetes version 1.32 is available in your region.
* Kubernetes 1.28 and 1.29 are now recognized as Long-Term Support (LTS) releases in AKS, joining existing LTS versions 1.27 and 1.30. For more information, see [Long Term Support (LTS)](https://learn.microsoft.com/azure/aks/long-term-support).
* AKS Kubernetes version 1.29 is going out of standard support in all regions on or before the end of April, 2025.
* You can now [switch non-LTS clusters](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#faq) on Kubernetes versions 1.25 onwards and within 3 versions of the current LTS versions to LTS by switching their tier to Premium.
* As of 31 March 2025, AKS no longer allows new cluster creation with the Basic Load Balancer. On 30 September 2025, the Basic Load Balancer will be retired. We will be posting updates on migration paths to the Standard Load Balancer. See [AKS Basic LB Migration Issue](https://github.com/Azure/AKS/issues/1687) for updates on when a simplified upgrade path is available. Refer to [Basic Load Balancer Deprecation Update](https://azure.microsoft.com/updates?id=azure-basic-load-balancer-will-be-retired-on-30-september-2025-upgrade-to-standard-load-balancer) for more information.
* The asm-1-22 revision for the Istio-based service mesh add-on has been deprecated. Migrate to a supported revision following the [AKS Istio upgrade guide](https://learn.microsoft.com/azure/aks/istio-upgrade).
* The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) feature was retired on 1st August 2023 and removed from AKS versions 1.25 and higher. PodSecurityPolicy property will be officially removed from AKS API starting from 2025-03-01.
* Starting on 17 June 2025, AKS will no longer create new node images for [Ubuntu 18.04](https://github.com/Azure/AKS/issues/4873) or provide security updates. Existing node images will be deleted. Your node pools will be unsupported and you will no longer be able to scale. To avoid service disruptions, scaling restrictions, and remain supported, please follow our instructions to [upgrade](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) to a [supported Kubernetes version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions).
* Starting on 17 March 2027, AKS will no longer create new node images for [Ubuntu 20.04](https://github.com/Azure/AKS/issues/4874) or provide security updates. Existing node images will be deleted. Your node pools will be unsupported and you will no longer be able to scale. To avoid service disruptions, scaling restrictions, and remain supported, please follow our instructions to [upgrade](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) to Kubernetes version 1.34+ by the retirement date.  
* HTTP Application Routing (preview) has been retired as of March 3, 2025 and AKS will start to block new cluster creation with HTTP App routing enabled. Affected clusters must [migrate](https://learn.microsoft.com/azure/aks/app-routing-migration) to the generally available Application Routing add-on prior to that date. 
* Customers with nodepools using [Standard_NC24rsv3](https://learn.microsoft.com/en-us/azure/virtual-machines/ncv3-nc24rs-retirement) VM sizes should resize or deallocate those VMs. Microsoft will deallocate remaining Standard_NC24rsv3 VMs in the coming weeks.
* [Teleport (preview)](https://github.com/Azure/acr/blob/main/docs/teleport/aks-getting-started.md) on AKS will be retired on 15 July 2025, please [migrate to Artifact Streaming (preview) on AKS](https://learn.microsoft.com/azure/aks/artifact-streaming) or update your node pools to set --aks-custom-headers EnableACRTeleport=false. Azure Container Registry has removed the Teleport API meaning that any nodes with Teleport enabled are pulling images from Azure Container Registry as any other AKS node. After 15 July 2025, any node pools with Teleport (preview) enabled may experience breakage and node provisioning failures. For more information, see [aka.ms/aks/teleport-retirement](https://aka.ms/aks/teleport-retirement).

### Release Notes
* Features:
  * [AKS Security Bulletin](https://learn.microsoft.com/en-us/azure/aks/security-bulletins/overview?tabs=aks-addons%2Caks-cluster%2Caks-node-image) and [AKS CVE Mitigation Status](https://releases.aks.azure.com/webpage/index.html) are now available to track Security and CVE mitigations
  * Azure Portal will now show you Deployment Recommendations based on available capacity of virtual machines
  * [Microsoft Copilot in Azure](https://techcommunity.microsoft.com/blog/azureinfrastructureblog/microsoft-copilot-in-azure-is-now-generally-available/4402033), including AKS is now generally available
  * [AKS cost recommendations in Azure Advisor](https://learn.microsoft.com/azure/aks/cost-advisors) is Generally Available
  * Kubernetes [1.32](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.32.md) is now Generally Available
  * AKS [Kubernetes patch versions](https://kubernetes.io/releases/patch-releases/) 1.31.7, 1.30.11, 1.29.15 to resolve [CVE-2025-0426](https://nvd.nist.gov/vuln/detail/CVE-2025-0426)
  * You can now enable [Federal Information Process Standard (FIPS)](https://aka.ms/aks/enable-fips) when using [Arm64 VM SKUs](https://aka.ms/aks/arm64) in Azure Linux 3.0 node pools in Kubernetes version 1.31+.
  * Enable Pod Sandboxing Confidential mounts for [Azure File CSI](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.32.1) driver on AKS 1.32
  * The Azure Portal now offers Deployment Recommendations proactively if there are capacity constraints on the selected node pool sku, zone, and region when creating a new AKS cluster. 
  * Custom Certificate Authority is available as GA in the [2025-01-01 GA API](https://learn.microsoft.com/rest/api/aks/managed-clusters/create-or-update?view=rest-aks-2025-01-01&tabs=HTTP#create-managed-cluster-with-custom-ca-trust-certificates). It isn't yet available in the CLI until May 2025. To use the GA feature in CLI before release, you can use the [`az rest`](https://learn.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-rest) command to add custom certificates during cluster creation. For more information, see [aka.ms/aks/custom-certificate-authority](https://aka.ms/aks/custom-certificate-authority).


* Behavior Changes:
  * Add node anti-affinity for FIPS-compliant nodes to prevent scheduling of retina-agent pods to stop CrashLoopBackOff on FIPS-enabled nodes whilst fix for Retina + FIPS is being rolled out.
  * Increased tofqdns-endpoint-max-ip-per-hostname from 50 to 1000 and tofqdns-min-ttl from 0 to 3600 in Azure Cilium for better handling of large DNS responses and reduce DNS query load.
  * Konnectivity agent will now scale based on cluster node count.
  * Starting on 15 April 2025, you will now be able to update your clusters to add an HTTP Proxy Configuration. Any update command that adds/changes an HTTP Proxy Configuration will now trigger an automatic reimage that will ensure all node pools in the cluster will have the same configuration. For more information, see [aka.ms/aks/http-proxy](https://aka.ms/aks/http-proxy).

* Component Updates:
  * Cost Analysis add-on updated to v0.0.22 to fix [CVE-2025-22866](https://pkg.go.dev/vuln/GO-2025-3447)
  * Updated ip-masq-agent updated to 0.1.15-2 to address [CVE-2024-45338](https://nvd.nist.gov/vuln/detail/CVE-2024-45338)
  * Application routing add-on updated to [v0.2.1-patch-8](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.1-patch-8) for Kubernetes below 1.30 and to [v0.2.3-patch-6](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.3-patch-6) for Kubernetes 1.30+. This updates ingress-nginx to v1.11.5 to fix [CVE-2025-1097](https://nvd.nist.gov/vuln/detail/CVE-2025-1097), [CVE-2025-1098](https://nvd.nist.gov/vuln/detail/CVE-2025-1098), [CVE-2025-1974](https://nvd.nist.gov/vuln/detail/CVE-2025-1974), [CVE-2025-24513](https://nvd.nist.gov/vuln/detail/CVE-2025-24513), and [CVE-2025-24514](https://nvd.nist.gov/vuln/detail/CVE-2025-24514).
  * Coredns 1.12.0 introduced a breaking change which was used in 1.32 AKS clusters. After the issue was discovered, Coredns was updated to [v1.11.3-6](https://github.com/coredns/coredns/releases/tag/v1.12.1) for 1.32 AKS clusters which does not contain the breaking change. Coredns upstream reverted the breaking change in v1.12.1. and AKS clusters on 1.33+ version will use coredns v1.12.1-1 (which does not contain the breaking change).
  * KEDA 2.16 is now supported on AKS 1.32. KEDA 2.15 and KEDA 2.14 introduced multiple breaking changes. View the [troubleshooting guide](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/extensions/changes-in-kubernetes-event-driven-autoscaling-add-on-214-215) to learn how to mitigate these breaking changes.
  * Updated NPM to [v1.5.45](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.45) to resolve [CVE-2025-22870](https://nvd.nist.gov/vuln/detail/CVE-2025-22870)
  * Cilium updated to v1.17 so that L7 policy (http, kafka etc) can now be applied to a cluster when advancedNetworkPolicies is set.
  * Windows GPU Device plugin updated to 0.0.17 to resolve [CVE-2025-22870](https://nvd.nist.gov/vuln/detail/CVE-2025-22870).
  * Egress gateway updated to [0.019](https://github.com/Azure/kube-egress-gateway/releases/tag/v0.0.19)
  * Eraser updated to v1.4.0-2 for Image Cleaner
  * Retina updated to [v0.0.29](https://github.com/microsoft/retina/releases/tag/v0.0.29) on Linux and Windows.
  * Cluster Autoscaler [updated](https://github.com/kubernetes/autoscaler/releases) to [1.29.5](https://github.com/kubernetes/autoscaler/releases/tag/cluster-autoscaler-1.29.5), [1.30.3](https://github.com/kubernetes/autoscaler/releases/tag/cluster-autoscaler-1.30.3), [1.31.1](https://github.com/kubernetes/autoscaler/releases/tag/cluster-autoscaler-1.31.1).
  * Updated Istio-based service mesh add-on revision asm-1-23 to patch [v1.23.5](https://istio.io/latest/news/releases/1.23.x/announcing-1.23.5) and  [v1.24.3](https://istio.io/latest/news/releases/1.24.x/announcing-1.24.3).
  * Azure File & Disk CSI driver updated to v1.29.14, v1.30.10,  v1.31.6 & v1.32.1
  * Azure Blob CSI driver updated to v1.25.5 on AKS 1.31 & v1.26.2 on AKS 1.32
  * AKS Azure Linux v2 image has been updated to [202504.06.0](vhd-notes/AzureLinux/202504.06.0.txt).
  * AKS Azure Linux v3 image has been updated to [202504.06.0](vhd-notes/AzureLinuxv3/202504.06.0.txt).
  * AKS Ubuntu 22.04 node image has been updated to [202504.06.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202504.06.0.txt).
  * AKS Ubuntu 24.04 node image has been updated to [202504.06.00](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202504.06.0.txt).

## Release 2025-03-16

Monitor the release status by region at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250316`.

### Announcements
* Starting in April 2025, Azure Kubernetes Service will begin rolling out a change to enable quota for all current and new AKS customers. AKS quota will represent a limit of the maximum number of managed clusters that an Azure subscription can consume per region. Existing AKS customer subscriptions will be given a quota limit at or above their current usage, depending on region availability. Once quota is enabled, customers can view their available quota and request quota increases in the Quotas page in the Azure Portal or by using the Quotas REST API. For details on how to view and request quota increases via the Portal Quotas page, visit [Azure Quotas](https://learn.microsoft.com/azure/quotas/view-quotas). For details on how to view and request quota increases via the Quotas REST API, visit: [Azure Quota REST API Reference](https://learn.microsoft.com/rest/api/reserved-vm-instances/quotaapi). New AKS customer subscriptions will be given a default limit upon new subscription creation. More information on the default limits for new subscriptions is available in documentation [here](https://learn.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).
* AKS Kubernetes version 1.28 will be the next [Long Term Support](https://learn.microsoft.com/azure/aks/long-term-support) version.
* You can now [switch non-LTS clusters](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#faq) on Kubernetes versions 1.25 onwards and within 3 versions of the current LTS versions to LTS by switching their tier to Premium.
* On 31 March 2025, AKS will no longer allow new cluster creation with the Basic Load Balancer. On 30 September 2025, the Basic Load Balancer will be retired. We will be posting updates on migration paths to the Standard Load Balancer. See [AKS Basic LB Migration Issue](https://github.com/Azure/AKS/issues/1687) for updates on when a simplified upgrade path is available. Refer to [Basic Load Balancer Deprecation Update](https://azure.microsoft.com/updates?id=azure-basic-load-balancer-will-be-retired-on-30-september-2025-upgrade-to-standard-load-balancer) for more information.
* The asm-1-22 revision for the Istio-based service mesh add-on has been deprecated. Migrate to a supported revision following the [AKS Istio upgrade guide](https://learn.microsoft.com/azure/aks/istio-upgrade).
* The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) feature was retired on 1st August 2023 and removed from AKS versions 1.25 and higher. PodSecurityPolicy property will be officially removed from AKS API starting from 2025-03-01.
* Starting on 17 June 2025, AKS will no longer create new node images for [Ubuntu 18.04](https://github.com/Azure/AKS/issues/4873) or provide security updates. Existing node images will be deleted. Your node pools will be unsupported and you will no longer be able to scale. To avoid service disruptions, scaling restrictions, and remain supported, please follow our instructions to [upgrade](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) to a [supported Kubernetes version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions).
* Starting on 17 March 2027, AKS will no longer create new node images for [Ubuntu 20.04](https://github.com/Azure/AKS/issues/4874) or provide security updates. Existing node images will be deleted. Your node pools will be unsupported and you will no longer be able to scale. To avoid service disruptions, scaling restrictions, and remain supported, please follow our instructions to [upgrade](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) to Kubernetes version 1.34+ by the retirement date.  

### Release Notes
* Features:
   * Application routing add-on support for [configuring the default NGINX ingress controller visibility](https://learn.microsoft.com/azure/aks/app-routing-nginx-configuration?tabs=azurecli#control-the-default-ingress-controller-configuration-when-creating-the-cluster) is now generally available in API 2025-02-01.
   * Kubernetes events for monitoring [node auto-repair](https://learn.microsoft.com/azure/aks/node-auto-repair) actions are now available for your AKS cluster. You can ingest these events and create alerts following the same [process as other Kubernetes events](https://learn.microsoft.com/azure/aks/events?tabs=azure-cli).
   * AKS [Kubernetes patch versions](https://kubernetes.io/releases/patch-releases/) 1.29.12, 1.29.13, 1.30.8, 1.30.9, 1.31.4, and 1.31.5 are now available.
   * [Application Gateway Ingress Controller](https://learn.microsoft.com/azure/application-gateway/tutorial-ingress-controller-add-on-new) now supports Azure CNI overlay clusters.
   * You can now upgrade AKS clusters with the Istio-based service mesh add-on enabled regardless of the compatibility with the current mesh revision, allowing to recover to a compatible and supported state. For more information, visit [istio upgrade documentation](https://learn.microsoft.com/azure/aks/istio-upgrade).
   * Istio-based service mesh add-on users can now customize the `externalTrafficPolicy` field in the Istio ingress gateway `Service` spec. AKS will no longer reconcile this field, preserving user-defined values.
   * [Message of the day](https://aka.ms/aks/message-of-the-day) is now generally available.
   * You can now enable [Federal Information Process Standard (FIPS)](https://aka.ms/aks/enable-fips) when using [Arm64 VM SKUs](https://aka.ms/aks/arm64). This is only supported for Azure Linux 3.0 node pools on Kubernetes version 1.32+.
   * You can now create Windows type Virtual Machine Node Pools. Note that existing Linux type VM node pools cannot be converted to Windows VM node pools. For more information, see [Create a Virtual Machine node pool](https://learn.microsoft.com/azure/aks/virtual-machines-node-pools).
   * Private clusters are now supported in [Automated Deployments](https://learn.microsoft.com/azure/aks/automated-deployments).
* Preview Features:
   * You can use the `EnableCiliumNodeSubnet` feature in preview to [create Cilium node subnet clusters](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium#option-3-assign-ip-addresses-from-the-node-subnet-preview) using Azure CNI Powered by Cilium.
   * [Control plane metrics](https://learn.microsoft.com/azure/aks/control-plane-metrics-monitor) are now available through Azure Monitor platform metrics in preview to monitor critical control plane components such as API server and etcd.

* Bug Fixes:
   * Fixed an [issue](https://github.com/microsoft/retina/issues/1386) with the retina-agent volume to restrict access to only `/var/run/cilium` directory. Currently retina-agent mounts `/var/run` from host directory. This can have potential issue as it can overwrite data in the directory.
   * Fixed an issue where SSHAccess was being reset to the default value `enabled` on partial PUT requests for `managedCluster.AgentPoolProfile.SecurityProfile` without specifying SSHAccess.
   * Fixed an issue where Node Auto Provisioning (Karpenter) failed to properly apply the `kubernetes.azure.com/azure-cni-overlay=true` label to nodes which resulted in failure to assign pod IPs in some cases.
   * Fixed an issue where `calico-typha` could be scheduled on virtual-kubelet due to overly permissive tolerations. Tolerations are now properly restricted to prevent incorrect scheduling. Check this [GitHub Issue](https://github.com/Azure/AKS/issues/4667) for more details.
   * Fixed an issue in Hubble-Relay scheduling behavior to prevent deployment on cordoned nodes, allowing the cluster autoscaler to properly scale down nodes.
   * Fixed an issue where pods could get stuck in `ContainerCreating` during Cilium+NodeSubnet to Cilium+Overlay upgrades by ensuring the original network configuration is retained on existing nodes.
   * Fixed an issue where priority class isn't set on the [Custom CA Trust DaemonSet](https://aka.ms/aks/custom-certificate-authority). This change ensures that the DaemonSet will not be evicted first in case of node pressure.
   * Fixed an issue where policy enforcements through Azure Policy addon were interrupted during cluster scaling or upgrade operations due to a [missing Pod Disruption Budget (PDB)](https://github.com/Azure/AKS/issues/4675) for the Gatekeeper webhook pods.

* Behavior Changes:
   * AI toolchain operator add-on has switched from using the `Machine` CRD to `NodeClaim` CRD, introducing a change in the experience to clean up resources after a KAITO workspace is deleted. Note that existing workspaces and GPU nodes provisioned by the add-on remain unaffected. Please refer to the [updated AKS guidance](https://aka.ms/aks/ai-toolchain-operator) for deletion of resources provisioned in old workspaces.
   * AKS will now enforce the limit of 10 unique CAs added to the node's trust store when using [Custom Certificate Authority](https://learn.microsoft.com/azure/aks/custom-certificate-authority).
   * Default maxSurge value to 10% for all new and existing clusters with Kubernetes versions >= 1.32.0.
   * Starting with Kubernetes 1.32, all Azure CNI NodeSubnet clusters will have the CNI installed via the Azure CNS DaemonSet instead of during node provisioning.
   * AKS now validates Istio custom resources that do not have the `istio.io/rev` label set.
   * When creating an [Azure Container Registry (ACR) cache rule](https://learn.microsoft.com/azure/aks/network-isolated?pivots=byo-acr#step-2-create-the-acr-and-enable-artifact-cache) to cache Microsoft Container Registry (MCR) container images in the private ACR, the required cache rule should be changed from `/*` to `aks-managed-repository/*`.
   * When creating a network isolated cluster with a managed [private Azure Container Registry (ACR)](https://learn.microsoft.com/azure/aks/network-isolated?pivots=byo-acr#step-2-create-the-acr-and-enable-artifact-cache), the registry will now have [anonymous pull access](https://learn.microsoft.com/azure/container-registry/anonymous-pull-access) set to false.

* Component Updates:
  * Istio revision `asm-1-24` is now available with Istio-based service mesh add-on. Please refer to [Istio's release announcement](https://istio.io/latest/news/releases/1.24.x/announcing-1.24/) for a full list of changes. Note that this release removes the deprecated `istio.io/gateway-name` label, please use `gateway.networking.k8s.io/gateway-name` label instead.
  * Update Azure Disk CSI driver to [v1.31.5](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.31.5) on AKS 1.31, [v1.30.9](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.30.9) on AKS 1.30.
   * Update Azure File CSI driver to [v1.31.4](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.31.4) on AKS 1.31, [v1.30.8](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.8) on AKS 1.30.
   * Update Azure Blob CSI driver to [v1.25.3](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.25.3)  on AKS 1.31, [v1.24.7](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.24.7) on AKS 1.30.
   * Update AI toolchain operator add-on (preview) to [KAITO v0.4.4](https://github.com/kaito-project/kaito/releases/tag/v0.4.4).
   * Update Cilium to [v1.17.0](https://github.com/cilium/cilium/releases/tag/v1.17.0) for AKS clusters >= 1.32.0.
   * Update Azure Monitor Container Insights image to [v3.1.26](https://github.com/microsoft/Docker-Provider/releases/tag/3.1.26).
   * Update Azure Monitor metrics image to [v6.15.0](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-02-21-2025) for all AKS cluster versions addressing CVE-2024-45338.
   * Update Credential Provider to [v1.32.3](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.32.3/), [v1.30.10](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.30.10/), [v1.31.4](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.31.4/), and [v1.32.3](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.32.3/).
   * Update Cloud Controller Manager [v1.32.3](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.32.3/), [v1.30.10](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.30.10/), [v1.31.4](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.31.4/), and [v1.32.3](https://cloud-provider-azure.sigs.k8s.io/blog/2025/03/07/v1.32.3/).
   * Update the Application Gateway Ingress Controller add-on to [v1.8.0](https://github.com/Azure/application-gateway-kubernetes-ingress/releases/tag/1.8.0), adding support for CNI Overlay.
   * Update windows-gmsa-webhook-image version has been bumped to [v0.12.1[(https://github.com/kubernetes-sigs/windows-gmsa/releases/tag/v0.12.1)] to address security vulnerabilities. There is no functionality change between v0.10.0 and v1.21.1.
   * Update Calico to [v3.28.3](https://github.com/projectcalico/calico/releases/tag/v3.28.3), TigeraOperator to [v1.34.8](https://github.com/tigera/operator/releases/tag/v1.34.8), Calico to [v3.29.2](https://github.com/projectcalico/calico/releases/tag/v3.29.2), and TigeraOperator to [v1.36.5](https://github.com/tigera/operator/releases/tag/v1.34.8), addressing security vulnerabilities including [CVE-2024-45337](https://nvd.nist.gov/vuln/detail/CVE-2024-45337) and [CVE-2024-45338](https://nvd.nist.gov/vuln/detail/CVE-2024-45338).
   * Update Node Auto Provisioning to use Karpenter [v0.7.3](https://github.com/Azure/karpenter-provider-azure/releases/tag/v0.7.3).
   * Update defender-admission-controller version from 20250212.3 to 20250304.1 to address [CVE-2024-56138](https://nvd.nist.gov/vuln/detail/CVE-2024-56138) and [CVE-2024-45339](https://nvd.nist.gov/vuln/detail/CVE-2024-45339).
   * Revert CoreDNS to v1.11.3-6 for AKS clusters on version 1.32+ due to an upstream regression in [v1.12.0](https://github.com/coredns/coredns/pull/6898). This version of CoreDNS is built using go version 1.23.3, OS family=azurelinux, version=3.0, and has no CVEs reported.
   * Update the `azure-ip-masq-agent` DaemonSet to use the `ip-masq-v2 image` addressing [CVE-2024-5535](https://nvd.nist.gov/vuln/detail/CVE-2024-5535) and [CVE-2024-9143](https://nvd.nist.gov/vuln/detail/CVE-2024-9143).
   * Update NPM image tag to [v1.5.44](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.44) to resolve CVEs in the Ubuntu base image, including [CVE-2025-0395](https://nvd.nist.gov/vuln/detail/CVE-2025-0395), [CVE-2024-12133](https://nvd.nist.gov/vuln/detail/CVE-2024-12133), [CVE-2024-12243](https://nvd.nist.gov/vuln/detail/CVE-2024-12243), [CVE-2024-13176](https://nvd.nist.gov/vuln/detail/CVE-2024-13176), [CVE-2024-9143](https://nvd.nist.gov/vuln/detail/CVE-2024-9143), [CVE-2024-45336](https://nvd.nist.gov/vuln/detail/CVE-2024-45336), [CVE-2024-45341](https://nvd.nist.gov/vuln/detail/CVE-2024-45341), and [CVE-2025-22866](https://nvd.nist.gov/vuln/detail/CVE-2025-22866). 
   * Update Azure Policy add-on to v1.10.0 which uses gatekeeper [v3.18.2](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.18.2).
   * AKS Azure Linux v2 image has been updated to [202503.13.0](vhd-notes/AzureLinux/202503.13.0.txt).
   * AKS Ubuntu 22.04 node image has been updated to [202503.13.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202503.13.0.txt).
   * AKS Ubuntu 24.04 node image has been updated to [202503.13.0](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202503.13.0.txt).
   * AKS Windows Server 2019 image has been updated to [17763.7009.250316](vhd-notes/AKSWindows/2019/17763.7009.250316.txt).
   * AKS Windows Server 2022 image has been updated to [20348.3328.250314](vhd-notes/AKSWindows/2022/20348.3328.250314.txt).
   * AKS Windows Server 23H2 image has been updated to [25398.1486.250314](vhd-notes/AKSWindows/23H2/25398.1486.250314.txt).

## Release 2025-02-20

Monitor the release status by region at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250220`.

### Announcements
* AKS Kubernetes version 1.32 is rolling out soon and is expected to reach all regions on or before the end of March. Please use the [az-aks-get-versions](https://learn.microsoft.com/azure/aks?view=azure-cli-latest#az-aks-get-versions) command to accurately capture if Kubernetes version 1.32 is available in your region.
* HTTP Application Routing (preview) is going to be [retired on March 3, 2025](https://azure.microsoft.com/updates?id=retirement-http-application-routing-addon-preview-for-aks-will-retire-03032025) and AKS will start to block new cluster creation with HTTP Application Routing (preview) enabled. Affected clusters must migrate to the generally available Application Routing add-on prior to that date. Refer to the [migration guide](https://learn.microsoft.com/azure/aks/app-routing-migration) for more information.
* Using the GPU VHD image (preview) to provision GPU-enabled AKS nodes was retired on January 10, 2025 and AKS will block creation of new node pools with the GPU VHD image (preview). Follow the detailed steps to [create GPU-enabled node pools](https://learn.microsoft.com/azure/aks/gpu-cluster?tabs=add-ubuntu-gpu-node-pool#use-the-aks-gpu-image-preview) using the alternative supported options.
* Extend the AKS [security patch release notes](https://releases.aks.azure.com/) in release tracker to include a package comparison with the current - 1 AKS Ubuntu base image.
* On 31 March 2025, AKS will no longer allow new cluster creation with the Basic Load Balancer. On 30 September 2025, the Basic Load Balancer will be retired. We will be posting updates on migration paths to the Standard Load Balancer. See [AKS Basic LB Migration Issue](https://github.com/Azure/AKS/issues/1687) for updates on when a simplified upgrade path is available. Refer to [Basic Load Balancer Deprecation Update](https://azure.microsoft.com/updates?id=azure-basic-load-balancer-will-be-retired-on-30-september-2025-upgrade-to-standard-load-balancer) for more information.

### Release Notes
* Features:
   * Application routing add-on support for [configuring the default NGINX ingress controller visibility](https://learn.microsoft.com/azure/aks/app-routing-nginx-configuration?tabs=azurecli#control-the-default-ingress-controller-configuration-when-creating-the-cluster) is now generally available in API 2025-02-01.
   * Kubernetes events for monitoring [node auto-repair](https://learn.microsoft.com/azure/aks/node-auto-repair) actions are now available for your AKS cluster. You can ingest these events and create alerts following the same [process as other Kubernetes events](https://learn.microsoft.com/azure/aks/events?tabs=azure-cli).
   * AKS [Kubernetes patch versions](https://kubernetes.io/releases/patch-releases/) 1.29.12, 1.29.13, 1.30.8, 1.30.9, 1.31.4, and 1.31.5 are now available.

* Preview Features:
   * You can use the `EnableCiliumNodeSubnet` feature in preview to [create Cilium node subnet clusters](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium#option-3-assign-ip-addresses-from-the-node-subnet-preview) using Azure CNI Powered by Cilium.
   * [Control plane metrics](https://learn.microsoft.com/azure/aks/control-plane-metrics-monitor) are now available through Azure Monitor platform metrics in preview to monitor critical control plane components such as API server, etcd, scheculer, autoscaler, and controller-manager.

* Bug Fixes:
   * Resolved an [issue](https://github.com/Azure/AKS/issues/4662) with Istio service mesh add-on where having multiple operations with the Lua EnvoyFilter (e.g. adding the Lua filter to call an external service and specifying the cluster referenced by Lua code) was not allowed.
   * Fixed a bug in Azure CNI Pod Subnet Static Block Allocation mode with Cilium which caused incorrect iptables rules, leading to pod connectivity failures to DNS and IMDS.
   * Resolved an [issue](https://github.com/Azure/azure-sdk-for-go/issues/24109) in Azure CNI static block IP allocation mode, where the updated Azure Table client mishandled untyped numbers, causing static block node pools to be misidentified as dynamic and leading to operation failures.
   * Fixed a bug in Azure Kubernetes Fleet Manager hub cluster resource groups (FL_ prefix resource groups) by truncating the name to avoid issues with long generated managed resource group names breaking the maximum length of resource groups.

* Behavior Changes:
   * [Horizontal Pod Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) introduced for `ama-metrics replicaset pod` in the [Azure Monitor managed service for Prometheus](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/prometheus-metrics-overview) add-on. More details about the configuration of the Horizontal Pod Autoscaler can be found [here](https://github.com/Azure/prometheus-collector/blob/main/internal/docs/HPA.md).
   * Starting with Kubernetes v1.32, node subnet mode will be installed via the `azure-cns` DaemonSet, allowing for faster security updates.

* Component Updates:
   * Updated Windows CNS from v1.6.13 to [v1.6.21](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.21) and Linux CNS from v1.6.18 to [v1.6.21](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.21).
   * Updated Windows CNI and Linux CNI from v1.6.18 to [v1.6.21](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.21).
   * Updated [tigera operator to v1.36.3](https://github.com/tigera/operator/releases/tag/v1.36.3) and [calico to v3.29.0](https://github.com/projectcalico/calico/releases/tag/v3.29.0).
   * Node Auto Provisioning has been upgraded to use Karpenter [v0.7.2](https://github.com/Azure/karpenter-provider-azure/releases/tag/v0.7.2).
   * Updated LTS patch version 1.27.102 for Command Injection affecting Windows nodes to address [CVE-2024-9042](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-9042).
   * Updated the Retina basic image to v0.0.25 for Linux and Windows to address [CVE-2025-23047](https://nvd.nist.gov/vuln/detail/CVE-2025-23047) and [CVE-2024-45338](https://nvd.nist.gov/vuln/detail/CVE-2024-45338).
   * Updated the cost-analysis-agent image from v0.0.20 to v0.0.21. Upgrades the following dependencies in cost-analysis-agent to fix [CVE-2024-45341](https://nvd.nist.gov/vuln/detail/CVE-2024-45341) and [CVE-2024-45336](https://nvd.nist.gov/vuln/detail/CVE-2024-45336):
      * github.com/Azure/azure-sdk-for-go/sdk/azcore v1.15.0 to [v1.17.0](https://github.com/Azure/azure-sdk-for-go/blob/main/sdk/azcore/CHANGELOG.md#1170-2025-01-07)
      * github.com/Azure/azure-sdk-for-go/sdk/azidentity v1.8.0 to [v1.8.1](https://github.com/Azure/azure-sdk-for-go/blob/main/sdk/azidentity/CHANGELOG.md#181-2025-01-15)
      * github.com/prometheus/common v0.60.0 to [v0.62.0](https://github.com/prometheus/common/releases/tag/v0.62.0)
      * github.com/samber/lo v1.47.0 to [v1.49.1](https://github.com/samber/lo/releases/tag/v1.49.1)
      * github.com/stretchr/testify v1.9.0 to [v1.10.0](https://github.com/stretchr/testify/releases/tag/v1.10.0)
   * AKS Azure Linux v2 image has been updated to [202502.09.0](vhd-notes/AzureLinux/202502.09.0.txt).
   * AKS Ubuntu 22.04 node image has been updated to [202502.09.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202502.09.0.txt).
   * AKS Ubuntu 24.04 node image has been updated to [202502.09.0](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202502.09.0.txt).
   * AKS Windows Server 2019 image has been updated to [17763.6775.250117](vhd-notes/AKSWindows/2019/17763.6775.250117.txt).
   * AKS Windows Server 2022 image has been updated to [20348.3091.250117](vhd-notes/AKSWindows/2022/20348.3091.250117.txt).
   * AKS Windows Server 23H2 image has been updated to [25398.1369.250117](vhd-notes/AKSWindows/23H2/25398.1369.250117.txt).

## Release 2025-01-30

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled `v20250130`.

### Announcements
* General support for AKS Kubernetes version 1.28 was deprecated on Jan 30, 2025. Upgrade your clusters to version 1.29 or later. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.
* Azure Kubernetes Service will no longer support the [WebAssembly System Interface (WASI) nodepools (preview).](https://learn.microsoft.com/azure/aks/use-wasi-node-pools) Starting on May 5, 2025 you will no longer be able to create new WASI nodepools. If you'd like to run WebAssembly (WASM) workloads, you can [deploy SpinKube to Azure Kubernetes Service (AKS)](https://learn.microsoft.com/azure/aks/deploy-spinkube) from Azure Marketplace. For more information on this retirement, see [AKS GitHub](https://github.com/Azure/AKS/issues/4770).
* The open-source project Bridge to Kubernetes will be retired on April 30, 2025. For more information, please see the [Bridge to Kubernetes](https://github.com/Azure/Bridge-To-Kubernetes) repository.
* [Azure Linux 3.0](https://learn.microsoft.com/azure/azure-linux/how-to-enable-azure-linux-3) is now the default Azure Linux version on AKS 1.32 Preview.
* The HTTP Application Routing add-on (preview) is going to be [retired](https://azure.microsoft.com/updates?id=retirement-http-application-routing-addon-preview-for-aks-will-retire-03032025) on March 3, 2025. You will no longer be able to create clusters that enable the add-on. [Migrate](https://learn.microsoft.com/Azure/AKS/app-routing-migration) to the generally available Application Routing add-on now.

### Release Notes
* Features:
  * AKS Kubernetes patch versions 1.29.11, 1.30.7 and 1.31.3 are now available.
  * Security patch releases in release tracker, starting with 20250115T000000Z will contain release notes for the release.

* Preview Features:
  * You can now monitor your stateful workloads running on AKS with Azure Container Storage using Azure Monitor managed service for Prometheus in Preview. You can use Azure Monitor managed service for Prometheus to collect Azure Container Storage metrics along with other Prometheus metrics from your AKS cluster. For more information please see [Enable monitoring for Azure Container Storage](https://learn.microsoft.com/azure/storage/container-storage/enable-monitoring?source=recommendations).
  * CNI validation for node autoprovisioner now allows all CNI configurations except for Calico and kubenet. See [AKS CNI Overview](https://learn.microsoft.com/azure/aks/concepts-network-cni-overview) for more information.
  * AKS Automatic SKU now supports using a [custom virtual network](https://learn.microsoft.com/azure/aks/automatic/quick-automatic-custom-network?pivots=azure-cli).
  * When using [NAP](https://learn.microsoft.com/azure/aks/node-autoprovision), custom subnets can be specified for node use via an update to the AKSNodeClass CRD which adds the vnetSubnetID property.

* Behavior change:
  * Proper casing will be enforced on PUT of `Microsoft.ContainerService/managedClusters/agentPools` for the `AgentPoolMode` property. See this [issue](https://github.com/Azure/AKS/issues/4468) for more detail.
  * Removed Prometheus port and scrape annotations from Retina Linux and Windows DaemonSets to avoid double scraping of metrics.
  * The standard load balancer can now be customized to include `port_*` annotations referenced in the [documentation](https://cloud-provider-azure.sigs.k8s.io/topics/loadbalancer/#loadbalancer-annotations).  An additional annotation has been added for: `external-dns.alpha.kubernetes.io/hostname`.  See this [document](https://learn.microsoft.com/en-us/azure/aks/load-balancer-standard) for more information.

* Bug Fix: 
  * Fixed a bug where some AgentPools with `"kubeletDiskType":"OS",` were not validated.
  * Fixed a bug when creating a cluster with a private DNS zone may result in an `InvalidTemplateDeployment` error.
  * Fixed a race and potential deadlock condition when a Non-Cilium cluster is updating to ACNS Cilium.
  * Added early validation on cluster creation when attempting to use 169.254.0.0/16 (link local) for pod or service CIDR blocks to prevent later run-time failures.
  * Fixed a breaking change between AppArmor and cilium. Starting on K8s 1.30 and Ubuntu 24.04, cilium containers can fail with error Init:CreateContainerError since AppArmor annotations are no longer supported. This change keeps apparmor annotations for k8s versions below 1.30, and adds the new security context field for k8s versions 1.30 and above. Related PR in upstream cilium charts: [https://github.com/cilium/cilium/pull/32199](https://github.com/cilium/cilium/pull/32199).
  * Fixed a bug that prevented upgrade from starting if the PDB `expectedPods` count is less than the `minAvailable` count.
  * Fixed an error condition when AKS attempts to remove the taint `disk.csi.azure.com/agent-not-ready=NoExecute` on node startup.  More details: [https://github.com/kubernetes-sigs/azuredisk-csi-driver/pull/2309](https://github.com/kubernetes-sigs/azuredisk-csi-driver/pull/2309)
  * Addressed an issue related to node subnet `IPAM Invoker Add failed with error: Failed to allocate pool` in the CNI logs and the associated [agentbaker release](https://github.com/Azure/AgentBaker/pull/5551).
  * Added validation when a cluster migrates to CNI Overlay to block migration when there is a custom ip-masq-agent config in the kube-system namespace.  This prevents loss of connectivity during migration.  See the [AKS documentation](https://learn.microsoft.com/azure/aks/upgrade-aks-ipam-and-dataplane) for more information.

* Component updates:
  * Cilium v1.14 version from v1.14.18-241220 to v1.14.18-250107 (v1.14.18-1) to include a fix for cilium dual stack upgrades. On upgrades, cilium config changes bpf-filter-priority from 1 to 2 but is not cleaning up the old filters at the old priority and as a result impacts connectivity. This patch will fix this bug, see GH issue in cilium repo for more details https://github.com/cilium/cilium/issues/36172
  * Update Azure File CSI driver version to [v1.29.10](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.10) on AKS 1.28
  * Update Azure File CSI driver version to [v1.30.7](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.7) on AKS 1.29 and 1.30
  * Update Azure File CSI driver version to [v1.31.3](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.31.3) on AKS 1.31
  * Update Azure Disk CSI driver to [v1.29.12](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.12) on AKS 1.28, 1.29
  * Update Azure Disk CSI driver to [v1.30.7](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.30.7) on AKS 1.30, 1.31
  * Update Azure Blob CSI driver to [v1.23.10](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.23.10) on AKS 1.28, 1.29
  * Update Azure Blob CSI driver to [v1.24.6](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.24.6) on AKS 1.30, 1.31
  * Update Workload Identity image version to [v1.4.0](https://github.com/Azure/azure-workload-identity/releases/tag/v1.4.0)
  * CNS/CNI updated to [v1.6.18](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.18) which includes Cilium nodesubnet support
  * Added Multi-Instance GPU support for standard_nc40ads_h100_v5
  * Update the OMS image to [v3.1.25-1](https://github.com/microsoft/Docker-Provider/releases/tag/3.1.25)
  * Update secret store driver to [v1.4.7](https://github.com/kubernetes-sigs/secrets-store-csi-driver/releases/tag/v1.4.7) and akv provider to [v1.6.2](https://github.com/Azure/secrets-store-csi-driver-provider-azure/releases/tag/v1.6.2).
  * Updates the Retina basic image to v0.0.23 on Linux and Windows: [release notes](https://github.com/microsoft/retina/releases/tag/v0.0.23) 
  * Update karpenter image version to [0.6.1-aks](https://github.com/Azure/karpenter-provider-azure/releases/tag/v0.6.1)
  * Update Cilium v1.16 from v1.16.5-250108 to v1.16.5-250110 (v1.16.5-1) to include a fix for Cilium dual stack upgrades. This will fix [https://github.com/cilium/cilium/issues/36172](https://github.com/cilium/cilium/issues/36172). Cilium [v1.16.5](https://github.com/cilium/cilium/releases/tag/v1.16.5) also contains fix for [CVE-2024-52529](https://nvd.nist.gov/vuln/detail/CVE-2024-52529).
  * The following CVEs were patched in Cilium v1.14.15
    * [CVE-2024-24789](https://nvd.nist.gov/vuln/detail/CVE-2024-24789)
    * [CVE-2024-24790](https://nvd.nist.gov/vuln/detail/CVE-2024-24790)
    * [CVE-2024-24791](https://nvd.nist.gov/vuln/detail/CVE-2024-24791)
    * [CVE-2024-34156](https://nvd.nist.gov/vuln/detail/CVE-2024-34156)
    * [CVE-2024-34155](https://nvd.nist.gov/vuln/detail/CVE-2024-34155)
    * [CVE-2024-34158](https://nvd.nist.gov/vuln/detail/CVE-2024-34158)
    * [CVE-2024-37307](https://nvd.nist.gov/vuln/detail/CVE-2024-37307)
    * [CVE-2024-42486](https://nvd.nist.gov/vuln/detail/CVE-2024-42486)
    * [CVE-2024-42487](https://nvd.nist.gov/vuln/detail/CVE-2024-42487)
    * [CVE-2024-42488](https://nvd.nist.gov/vuln/detail/CVE-2024-42488)
    * [CVE-2024-47825](https://nvd.nist.gov/vuln/detail/CVE-2024-47825)
  * Update the cost-analysis-agent image v0.0.19 to v0.0.20.  Upgrades the following dependencies in cost-analysis-agent to fix [CVE-2024-45337](https://nvd.nist.gov/vuln/detail/CVE-202445337) and [CVE-2024-45338](https://nvd.nist.gov/vuln/detail/CVE-2024-45338)
    * golang.org/x/crypto v0.27.0 to [v0.31.0](https://pkg.go.dev/golang.org/x/crypto@v0.31.0)
    * golang.org/x/net v0.29.0 to [v0.33.0](https://pkg.go.dev/golang.org/x/net@v0.33.0)
    * golang.org/x/sys v0.25.0 to [v0.28.0](https://pkg.go.dev/golang.org/x/sys@v0.28.0)
    * golang.org/x/text v0.18.0 to [v0.21.0](https://pkg.go.dev/golang.org/x/text@v0.21.0)
  * coredns image v1.12.0-1 and v1.9.4-5 versions have been built using Dalec framework, published to MCR under oss/v2 path. All AKS clusters starting with 1.32+ versions will use v1.12.0-1 coredns image version and existing AKS clusters on versions 1.24 to 1.32 will use v1.9.4-5 coredns image version.
  * Update the ip-masq-agent-v2 to [v0.1.15](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.15) to address [CVE-2024-45338](https://nvd.nist.gov/vuln/detail/CVE-2024-45338) and [CVE-2024-10220](https://nvd.nist.gov/vuln/detail/CVE-2024-10220)
  * Update NPM image to v1.5.41 to fix [CVE-2024-45338](https://nvd.nist.gov/vuln/detail/CVE-2024-45338) in usr/bin/azure-npm (gobinary) and GHSA-xr7q-jx4m-x55m in usr/bin/azure-npm (gobinary).  See the release notes for [v1.5.41](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.41) for more details.
  * Update the prometheus collector for azuremonitor to the [01-16-2025](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-01-16-2025) release
  * VHD Updates
    * AKS Windows Server 2019 image has been updated to [AKSWindows-2019-17763.6775.250117](vhd-notes/AKSWindows/2019/17763.6775.250117.txt).
    * AKS Windows Server 2022 image has been updated to [AKSWindows-2022-20348.3091.250117](vhd-notes/AKSWindows/2022/20348.3091.250117.txt).
    * AKS Windows Server Annual Channel 23H2 image has been updated to [AKSWindows-2022-23H2-25398.1369.250117](vhd-notes/AKSWindows/23H2/25398.1369.250117.txt).
    * AKS Azure Linux 2.0 image has been updated to [202501.28.0](vhd-notes/AzureLinux/202501.28.0.txt).
    * AKS Azure Linux 3.0 image has been updated to [202501.28.0](vhd-notes/Azurelinuxv3/202501.28.0.txt).
    * AKS Ubuntu 2204 image has been updated to [202501.28.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202501.28.0.txt).
    * AKS Ubuntu 2404 image has been updated to [202501.28.0](vhd-notes/aks-ubuntu/AKSUbuntu-2404/202501.28.0.txt).

## Release 2025-01-06

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as `v20250106`.

### Announcements
* AKS Kubernetes version 1.28 is deprecated by Jan 30, 2025. Kindly upgrade your clusters to 1.29 version or above. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.

### Release Notes
* Features:
  * AKS Kubernetes version 1.31 is now in GA.
  * AKS Kubernetes patch versions 1.29.11, 1.30.7, 1.31.2, and 1.31.3 are now available.
  * AKS LTS version 1.27.101 available in all regions since December 2024. This patches the kubelet [CVE-2024-10220](https://nvd.nist.gov/vuln/detail/CVE-2024-10220)
  * [Advanced Container Networking Service](https://learn.microsoft.com/azure/aks/advanced-container-networking-services-overview?tabs=cilium) (ACNS) is Generally Available.
   
* Preview features:
  * SeccompDefault is now an available parameter in custom node configuration. For more information on enabling seccomp profiles, see [Secure container access to resources](https://learn.microsoft.com/azure/aks/secure-container-access).

* Behavior change:
  * [Invalid values sent to the Azure AKS API for the properties.mode field of AKS AgentPools will now be rejected](https://github.com/Azure/AKS/issues/4468). Prior to this change, unknown modes were assumed to be User. The only valid values for this field are the (case-sensitive) strings: "User", "System", or "Gateway".
  * AKS no longer supports the [GPU image (preview)](https://github.com/Azure/AKS/issues/4472) to provision GPU-enabled AKS nodes. Alternative options that are supported today and recommended by AKS include the default experience with manual NVIDIA device plugin installation or the NVIDIA GPU Operator, detailed in [AKS GPU node pool documentation](https://learn.microsoft.com/azure/aks/gpu-cluster?tabs=add-ubuntu-gpu-node-pool#confirm-that-gpus-are-schedulable).
  * [Kubernetes version 1.32 is the last version that supports Windows Server 2019](https://github.com/Azure/AKS/issues/4268). You will not be able to create new or upgrade existing Windows Server 2019 node pools in AKS versions 1.33+. Follow the detailed steps [in AKS documentation](https://aka.ms/aks/ws2019-migration) to transition to Windows Server 2022 or any newly supported Windows Server version by that date. After 1 March 2026, Windows Server 2019 won't be supported.
  * New API throttling limit has been added to [PutManagedCluster](https://learn.microsoft.com/rest/api/aks/managed-clusters?view=rest-aks-2024-09-01) API for AKS. Please see [AKS resource provider throttling limits](aka.ms/aks/throttling) for more details.
 
* Bug Fix: 
  * GPU bootstrapping issue impacting GPU provisioning with Node Auto Provision has been fixed. Refer [Github issue](https://github.com/Azure/karpenter-provider-azure/pull/587) for more details.
  * Fixed an issue in v1.31 where Cluster Autoscaler did not respond to external changes in Spot VMSS based nodepool's node count (e.g., evictions), leading to scale-up failures. Refer [Github Issue 7373](https://github.com/kubernetes/autoscaler/issues/7373) for more details.
  * Resolved an issue [(NotFound error message)](https://learn.microsoft.com/en-us/azure/azure-resource-manager/troubleshooting/error-not-found?tabs=bicep#symptoms) when querying a VM which has been deleted, which results in the NodeClaim being stuck in notReady state resulting in the NodeClaim not being deleted.
  * Fixed the windows nodes CNS pods restarting [Github issue](https://github.com/Azure/AKS/issues/4679) observed in clusters running on AKS +v1.27 Kubernetes version.
    
* Component updates:
  * Tigera operator image version has been bumped to [v1.34.7](https://github.com/tigera/operator/releases/tag/v1.34.7) with this release, for clusters running Kubernetes version (and including) v1.30.0. This patches the following CVEs detected in the tigera operator - [CVE-2021-3999](https://nvd.nist.gov/vuln/detail/CVE-2021-3999), [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751), [CVE-2019-19126](https://nvd.nist.gov/vuln/detail/CVE-2019-19126), [CVE-2021-35942](https://nvd.nist.gov/vuln/detail/CVE-2021-35942), [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752), [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029), [CVE-2019-9169](https://nvd.nist.gov/vuln/detail/CVE-2019-9169), [CVE-2020-6096](https://nvd.nist.gov/vuln/detail/CVE-2020-6096), [CVE-2021-38604](https://nvd.nist.gov/vuln/detail/CVE-2021-38604), [CVE-2018-19591](https://nvd.nist.gov/vuln/detail/CVE-2018-19591), [CVE-2018-20796](https://nvd.nist.gov/vuln/detail/CVE-2018-20796), [CVE-2019-9192](https://nvd.nist.gov/vuln/detail/CVE-2019-9192), [CVE-2021-3326](https://nvd.nist.gov/vuln/detail/CVE-2021-3326), [CVE-2019-6488](https://nvd.nist.gov/vuln/detail/CVE-2019-6488), [CVE-2016-10739](https://nvd.nist.gov/vuln/detail/CVE-2016-10739), [CVE-2019-7309](https://nvd.nist.gov/vuln/detail/CVE-2019-7309), [CVE-2022-23219](https://nvd.nist.gov/vuln/detail/CVE-2022-23219), [CVE-2022-23218](https://nvd.nist.gov/vuln/detail/CVE-2022-23218), [CVE-2019-25013](https://nvd.nist.gov/vuln/detail/CVE-2019-25013), [CVE-2020-27618](https://nvd.nist.gov/vuln/detail/CVE-2020-27618).
  * Azure Disks CSI driver version has been bumped to [v1.30.6](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.30.6) for AKS clusters running AKS Kubernetes version +v1.30. This patches the following CVEs - [CVE-2024-51744](https://nvd.nist.gov/vuln/detail/CVE-2024-51744), [CVE-2024-50602](https://nvd.nist.gov/vuln/detail/CVE-2024-50602), [CVE-2024-9143](https://nvd.nist.gov/vuln/detail/CVE-2024-9143), [CVE-2019-11255](https://nvd.nist.gov/vuln/detail/CVE-2019-11255)
  * Bumping the Azure CNI version from v1.4.56 to v1.4.58. This patches the CVE regarding grpc 1.52.0 dependencies - [CVE-2023-2976](https://nvd.nist.gov/vuln/detail/CVE-2023-2976), [CVE-2020-8908](https://nvd.nist.gov/vuln/detail/CVE-2020-8908)
  * Cilium container image version bumped to [v1.14.15-241024](https://github.com/cilium/cilium/releases/tag/v1.14.15) for AKS clusters running k8s version greater than v1.29.
  * AKS Azure Linux v2 image has been updated to [202501.12.0](vhd-notes/AzureLinux/202501.12.0.txt)
  * AKS Azure Linux v3 image has been updated to [202501.05.0](vhd-notes/Azurelinuxv3/202501.05.0.txt)
  * AKS Ubuntu 22.04 node image has been updated to [202501.12.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202501.12.0.txt)
  * AKS Windows Server 2022 image has been updated to [v20348.2966.241218](vhd-notes/AKSWindows/2022/20348.2966.241218.txt)
  * AKS Windows Server 2019 image has been updated to [17763.6659.241226](vhd-notes/AKSWindows/2019/17763.6659.241226.txt)
  * AKS Windows Server 23H2 image has been updated to [25398.1308.241226](vhd-notes/AKSWindows/23H2/25398.1308.241226.txt)
  * App routing operator updated to [0.2.1-patch-6 ](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.1-patch-6)for K8s < 1.30 and which upgrades external-dns to version [0.15.0](https://github.com/kubernetes-sigs/external-dns/releases/tag/v0.15.0) fixing a number of CVEs  ([CVE-2023-39325](https://nvd.nist.gov/vuln/detail/CVE-2023-39325), [GHSA-m425-mq94-257g](https://github.com/advisories/GHSA-m425-mq94-257g), [CVE-2024-24790](https://nvd.nist.gov/vuln/detail/CVE-2024-24790), [CVE-2023-39325](https://nvd.nist.gov/vuln/detail/CVE-2023-39325), [CVE-2023-45283](https://nvd.nist.gov/vuln/detail/CVE-2023-45283), [CVE-2023-45288](https://nvd.nist.gov/vuln/detail/CVE-2023-45288), [CVE-2024-34156](https://nvd.nist.gov/vuln/detail/CVE-2024-34156))
  * App routing operator updated to [0.2.3-patch-3](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.3-patch-3) for K8s +1.30 which fixes an issue where Open Service Mesh would not reload correctly on Nginx deployment updates. The Prometheus metrics endpoint has now been moved to a separate Service called nginx-metrics behind a ClusterIP. Prometheus scraping will continue to work as expected.
  * Cost-analysis-agent image upgraded from v0.0.18 to v0.0.19. this upgrades the [golang-jwt](https://pkg.go.dev/github.com/golang-jwt/jwt/v4) dependency in cost-analysis-agent to patch [CVE-2024-51744](https://nvd.nist.gov/vuln/detail/CVE-2024-51744)
  * [Promtheus collector](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md) for Azure Monitor managed service for Prometheus addon version bumped from 6.10.1-main-10-04-2024-77dcfe3d to 6.11.0-main-10-21-2024-91ec49e3. This fixes a bug where the minimal ingestion profile keep list was not being honored.
  * Application Gateway ingress controller addon version bumped from 1.7.4 to 1.7.6 for clusters with AKS Kubernetes version greater than or equal to 1.27. please find more details [here](https://github.com/Azure/application-gateway-kubernetes-ingress/releases/tag/1.7.6)
  * Retina enterprise and operator image version bumped to [v0.1.3](https://github.com/azure-networking/retina-enterprise/releases/tag/v0.1.3). This resolves the following CVEs - [CVE-2024-37307](https://nvd.nist.gov/vuln/detail/CVE-2024-37307), [CVE-2024-42486](https://nvd.nist.gov/vuln/detail/CVE-2024-42486), [CVE-2024-42487](https://nvd.nist.gov/vuln/detail/CVE-2024-42487), [CVE-2024-42488](https://nvd.nist.gov/vuln/detail/CVE-2024-42488), [CVE-2024-47825](https://nvd.nist.gov/vuln/detail/CVE-2024-47825), and [CVE-2023-45288](https://nvd.nist.gov/vuln/detail/CVE-2023-45288) and changes for high-level filtering of some metric labels. This results in less irrelevant metric collection which can affect clusters at a large scale.
  * Retina basic image version bumped to [v0.0.17](https://github.com/microsoft/retina/releases/tag/v0.0.17) which patches the following CVEs: [CVE-2024-37307](https://nvd.nist.gov/vuln/detail/CVE-2024-37307), [CVE-2024-42486](https://nvd.nist.gov/vuln/detail/CVE-2024-42486), [CVE-2024-42487](https://nvd.nist.gov/vuln/detail/CVE-2024-42487), [CVE-2024-42488](https://nvd.nist.gov/vuln/detail/CVE-2024-42488), [CVE-2024-47825](https://nvd.nist.gov/vuln/detail/CVE-2024-47825), and [CVE-2023-45288](https://nvd.nist.gov/vuln/detail/CVE-2023-45288). This also changes for high-level filtering of some metric labels. This results in less irrelevant metric collection which can affect clusters at a large scale
  * NPM image version bumped to [v1.5.39](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.39) to fix potential connectivity issues for clusters with "azure" network policy manager on nodes with high scale of iptables rules and [CVE-2024-34155](https://nvd.nist.gov/vuln/detail/CVE-2024-34155), [CVE-2024-34156](https://nvd.nist.gov/vuln/detail/CVE-2024-34156), and [CVE-2024-34158](https://nvd.nist.gov/vuln/detail/CVE-2024-34158)
  * Istio-based service mesh add-on revision asm-1-23 has been upgraded to patch [v1.23.4](https://istio.io/latest/news/releases/1.23.x/announcing-1.23.4/), revision asm-1-22 has been upgraded to patch [v1.22.7](https://istio.io/latest/news/releases/1.22.x/announcing-1.22.7/), and revision asm-1-22 has been upgraded to patch [v1.22.3](https://istio.io/latest/news/releases/1.22.x/announcing-1.22.3). Users can restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade#patch-version-upgrade). Vulnerability [CVE-2024-41110](https://nvd.nist.gov/vuln/detail/CVE-2024-41110) and [CVE-2024-53271](https://nvd.nist.gov/vuln/detail/CVE-2024-53271) has been addressed in the patch version 1.23.4 and 1.22.7.

## Release 2024-10-25

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as `v20241025`.

### Announcements

* AKS version 1.28 End of Life is Jan, 15 2025.
* AKS will be upgrading the KEDA addon to more recent [KEDA versions](https://github.com/kedacore/keda/releases). The AKS team has added KEDA 2.15 on AKS clusters with K8s versions >=1.32, KEDA 2.14 for Kubernetes v1.30 and v1.31. KEDA 2.15 and KEDA 2.14 will introduce multiple breaking changes. View the [troubleshooting guide](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/extensions/changes-in-kubernetes-event-driven-autoscaling-add-on-214-215) to learn how to mitigate these breaking changes.
* AKS will no longer support the [GPU image (preview)](https://github.com/Azure/AKS/issues/4472) to provision GPU-enabled AKS nodes. Starting on Jan 10, 2025 you will no longer be able to create new GPU-enabled node pools with the GPU image. Alternative options that are supported today and recommended by AKS include the default experience with manual NVIDIA device plugin installation or the NVIDIA GPU Operator, detailed in [AKS GPU node pool documentation](https://learn.microsoft.com/azure/aks/gpu-cluster?tabs=add-ubuntu-gpu-node-pool#confirm-that-gpus-are-schedulable).
* Starting on January 1, 2025, [invalid values sent to the Azure AKS API for the properties.mode field of AKS AgentPools will be rejected](https://github.com/Azure/AKS/issues/4468). Prior to this change, unknown modes were assumed to be User. The only valid values for this field are the (case-sensitive) strings:["User", "System"](https://learn.microsoft.com/rest/api/aks/agent-pools/create-or-update?view=rest-aks-2024-02-01&tabs=HTTP#agentpoolmode), or ["Gateway"](https://learn.microsoft.com/rest/api/aks/agent-pools/create-or-update?view=rest-aks-2024-06-02-preview&tabs=HTTP#agentpoolmode).
* AKS will start to block new cluster creation with basic load balancer in January 2025. Basic Load Balancer will be deprecated September 31 2025 and affected clusters must be migrated to the Standard Load Balancer prior to that date. Refer to [BLB deprecation announcement](
https://azure.microsoft.com/updates/azure-basic-load-balancer-will-be-retired-on-30-september-2025-upgrade-to-standard-load-balancer/) for more information.
* As of November 30th, 2024, new AKS clusters created with Kubernetes versions 1.28 and 1.29 will no longer enable beta Kubernetes APIs. This matches the behavior of AKS 1.27 LTS and AKS 1.30+ clusters, which no longer enable beta APIs.

### Release Notes

* Features:
  * AKS patch versions 1.28.14, 1.29.9, 1.30.5 are now available. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.
  * AKS version [`1.31`](https://kubernetes.io/blog/2024/08/13/kubernetes-v1-31-release/) is now generally available. Please check the release tracker for when your region will receive the GA update. Some regions may not receive this update until later in November.
  * The first official patch version of AKS LTS 1.27, 1.27.100, is being released.
  * [GitHub Copilot](https://azure.microsoft.com/products/github/Copilot) for Azure now supports [AKS commands](https://learn.microsoft.com/azure/aks/aks-extension-ghcopilot-plugins).
  * You can now skip one release [while upgrading Azure Service Mesh](https://learn.microsoft.com/azure/aks/istio-upgrade) as long as the destination release is a supported revision - for example, asm-1-21 can upgrade directly to asm-1-23.
  * You can now fine-tune supported models on KAITO version [0.3.1](https://github.com/Azure/kaito/releases/tag/v0.3.1) with the AI toolchain operator add-on on your AKS cluster.

* Preview features:
  * We've added [a new way to optimize your upgrade process drain behavior](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli#optimize-for-undrainable-node-behavior-preview). By default, a node drain failure causes the upgrade operation to fail, leaving the undrained nodes in a schedulable state, this behavior is called `Schedule`. Alternatively, you can select the `Cordon` behavior, which skips nodes that fail to drain by placing them in a quarantined state, labeling them `kubernetes.azure.com/upgrade-status:Quarantined` and proceeds with upgrading the remaining nodes. This ensures that all nodes are either upgraded or quarantined. This approach allows you to troubleshoot drain failures and gracefully manage the quarantined nodes.
  * You can now [block pod access to the Azure Instance Metadata Service (IMDS) endpoint](https://learn.microsoft.com/azure/aks/imds-restriction) to enhance security.
  * [Azure Linux v3](https://learn.microsoft.com/azure/azure-linux/) is now in preview for AKS 1.31 clusters. After registering the preview flag `AzureLinuxV3Preview` newly created AzureLinux node pools will receive the v3 image. Existing Azure Linux v2 node pools will not upgrade to v3 and must be recreated to upgrade.
    * NOTE: Azure Linux v3 changes the cryptographic provider to OpenSSL + SymCrypt. The SymCrypt library will operate in FIPS mode but is still in the final stages of the validation process and thus **is not considered to be FIPS-validated at this time**. Do not use this preview with FIPS-enabled node pools if you must use a FIPS-validated cryptographic library.

* Behavior change:
  * Virtual Machine node pools creation will be blocked if the cluster is using system-assigned identity and bring-your-own virtual network, as this combination does not function properly. To utilize virtual machine node pools, [migrate the cluster to a user-assigned managed identity](https://learn.microsoft.com/azure/aks/use-managed-identity#update-an-existing-cluster-to-use-a-user-assigned-managed-identity) with the required permissions on the virtual network. Virtual Machine Scale Set pools are unaffected by this change.
  * Enabling [long term support](https://learn.microsoft.com/azure/aks/long-term-support) no longer changes the default [cluster upgrade channel](https://learn.microsoft.com/azure/aks/auto-upgrade-cluster) to `patch`.
  * AKS CoreDNS configuration will now block all queries ending in `reddog.microsoft.com` and some queries ending in `internal.cloudapp.net` from being forwarded to upstream DNS when they are the result of improper search domain completion. See [the documentation for more details.](https://learn.microsoft.com/azure/aks/coredns-custom#invalid-search-domain-completions-for-internalcloudappnet-and-reddogmicrosoftcom)
  * Azure NPM's CPU request has been lowered from 250m to 50m.
  * Azure CNI Overlay now checks that the pod CIDR does not conflict with any subnet in the virtual network, rather than checking if it conflicts with the virtual network address space as a whole.

* Component updates:
  * gMSA support is updated to version [v0.10.0](https://github.com/kubernetes-sigs/windows-gmsa/releases/tag/v0.10.0), adding support for random hostnames and fixing [an issue with multiple containers invalidating domain trusts.](https://github.com/microsoft/Windows-Containers/issues/405)
  * [Image Cleaner](https://learn.microsoft.com/azure/aks/image-cleaner) has been upgraded to [v1.4.0-1](https://github.com/eraser-dev/eraser/releases/tag/v1.4.0).
  * The following Azure CSI drivers have been updated:
    * [Azure Blob CSI Driver](https://github.com/kubernetes-sigs/blob-csi-driver/releases): v1.22.9 for AKS 1.27, v1.23.9 for AKS 1.28 and 1.29, and v1.24.5 for AKS 1.30+
    * [Azure Disk CSI Driver](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases): v1.28.11 for AKS 1.27, v1.19.10 for AKS 1.28 and 1.29, and v1.30.5 for AKS 1.30+
    * [Azure Files CSI Driver](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases): v1.28.13 for AKS 1.27, v1.29.9 for AKS 1.28, v1.30.6 for AKS 1.29+
  * Azure Monitor for Containers has been upgraded to [3.1.24](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#10072024--).
  * AKS Windows Server 2019 image has been updated to [AKSWindows-2019-17763.6414.241010](vhd-notes/AKSWindows/2019/17763.6414.241010.txt).
  * AKS Windows Server 2022 image has been updated to [AKSWindows-20348.2762.241009](vhd-notes/AKSWindows/2022/20348.2700.240911.txt).
  * AKS Azure Linux image has been updated to [202410.27.0](vhd-notes/AzureLinux/202410.27.0.txt).
  * AKS Ubuntu image has been updated to [202410.27.0](vhd-notes/aks-ubuntu/202410.27.0.txt).
  * cost-analysis-agent image has been updated to v0.0.18
  * ip-masq-agent image has been updated to [v0.1.14](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.14)
  * Components in the AKS run-command image have been added and upgraded
    * New components: jq, awk, grep, xargs
    * Upgraded: kubectl to v1.30.5, helm to 3.15.4

## Release 2024-10-06

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as `v20241006`.

### Announcements

* AKS version 1.30 is now available as a [Long term support version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) and AKS version 1.28 End of Life is Jan, 15 2025.
* Upgrade from LTS 1.27 to LTS 1.30 is now supported. 
* AKS will be upgrading the KEDA addon to more recent [KEDA versions](https://github.com/kedacore/keda/releases). The AKS team has added KEDA 2.15 on AKS clusters with K8s versions >=1.31, KEDA 2.14 for Kubernetes v1.30. KEDA 2.15 and KEDA 2.14 will introduce multiple breaking changes. View the [troubleshooting guide](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/extensions/changes-in-kubernetes-event-driven-autoscaling-add-on-214-215) to learn how to mitigate these breaking changes.
* AKS will no longer support the [GPU image (preview)](https://github.com/Azure/AKS/issues/4472) to provision GPU-enabled AKS nodes. Starting on Jan 10, 2025 you will no longer be able to create new GPU-enabled node pools with the GPU image. Alternative options that are supported today and recommended by AKS include the default experience with manual NVIDIA device plugin installation or the NVIDIA GPU Operator, detailed in [AKS GPU node pool documentation](https://learn.microsoft.com/azure/aks/gpu-cluster?tabs=add-ubuntu-gpu-node-pool#confirm-that-gpus-are-schedulable).
* Starting on January 1, 2025, [invalid values sent to the Azure AKS API for the properties.mode field of AKS AgentPools will be rejected](https://github.com/Azure/AKS/issues/4468). Prior to this change, unknown modes were assumed to be User. The only valid values for this field are the (case-sensitive) strings:["User", "System"](https://learn.microsoft.com/rest/api/aks/agent-pools/create-or-update?view=rest-aks-2024-02-01&tabs=HTTP#agentpoolmode), or ["Gateway"](https://learn.microsoft.com/rest/api/aks/agent-pools/create-or-update?view=rest-aks-2024-06-02-preview&tabs=HTTP#agentpoolmode).

### Release Notes

* Preview features:
  * AKS version [`1.31`](https://kubernetes.io/blog/2024/08/13/kubernetes-v1-31-release/) is now available in preview. 
  * You can now [specify the GPU driver type](https://learn.microsoft.com/azure/aks/use-windows-gpu) when creating a new AKS Windows GPU Nodepool using the `--driver-type` flag.
  * You can now assign a [static egress gateway](https://learn.microsoft.com/azure/aks/configure-static-egress-gateway) node pool to provide a stable egress IP for your pods.
    
* Bug fixes:
  * Bug fix to address an issue where Calico pods were stuck in Terminating state.
  * Fixed a race condition in Azure Network Policy when editing or deleting then re-adding a network policy without a CIDR handle.
  * Fixed a race condition between Cilium and Retina CRDs for Cilium (when Retina is updating to Cilium).
  * Bug fix for certificate rotation in the gMSA webhook.
  * Bug fix for Advanced Network Observability where the Retina operator didn't have proper permissions.
  * Bug fix to address an issue where the Retina operator was not reading the configuration from the ConfigMap.
 
* Behavior change:
  * Deprecated API detection will now [only show usage on non-readonly verbs (ie: not GET/LIST/WATCH)](https://learn.microsoft.com/azure/aks/stop-cluster-upgrade-api-breaking-changes).
  * Starting with AKS version 1.31, nodes will now [pull container images in a parallel](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/availability-performance/container-image-pull-performance) by default. In versions prior to 1.31, the pull type will remain serialized.
  * When cloud-node-manager-windows enables [Windows HostProcess containers](https://learn.microsoft.com/azure/aks/use-windows-hpc), a Windows DaemonSet will be deployed to initialize kube-proxy.

* Component updates:
  * Updated CNI and CNS versions to [`v1.6.7`](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.7).    
  * Updated Azure Network Policy Manager (NPM) to [`v1.5.37`](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.37).
  * Updated Azure Policy addon to [`v1.7.1`](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#171).
  * Updated konnectivity-agent image version to `v0.30.3-hotfix.20240819`.
  * Updated containerd-spin-shim to [`v0.15.1`](https://github.com/spinkube/containerd-shim-spin/releases/tag/v0.15.1).
  * Updated Istio-based service mesh add-on revision `asm-1-23` to patch [`v1.23.1`](https://istio.io/latest/news/releases/1.23.x/announcing-1.23.1). `asm-1-20` is now unsupported. Users can restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade#patch-version-upgrade).
  * Updated Cilium to [`v1.14.15-241002`](https://github.com/cilium/cilium/releases/tag/v1.14.15).
  * Updated Calico to [`v3.28.1`](https://github.com/projectcalico/calico/blob/v3.28.1/release-notes/v3.28.1-release-notes.md).
  * Updated ama-logs to [`v3.1.24`](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#10072024--).
  * Updated azure-cloud-controller-manager to versions [`v1.31.1`](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.31.1), [`v1.30.7`](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.30.7), [`v1.29.11`](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.29.11), [`v1.28.13`](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.28.13).
  * Updated overlay-vpa to `v1.2.1` for Kubernetes 1.31.0+ and `v1.0.0` for Kubernetes 1.27.0+.
  * Azure Linux image has been updated to [`Azure Linux-202409.30.0`](vhd-notes/AzureLinux/202409.30.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [`AKSUbuntu-202409.30.0`](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202409.30.0.txt).
    
## Release 2024-09-18

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as v20240918.

### Announcements

* AKS version 1.30 is now available as a [Long term support version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) and AKS version 1.28 End of Life is Jan, 15 2025.
* AKS will be upgrading the KEDA addon to more recent [KEDA versions](https://github.com/kedacore/keda/releases). The AKS team has added KEDA 2.15 on AKS clusters with K8s versions >=1.31, KEDA 2.14 for Kubernetes v1.30. KEDA 2.15 and KEDA 2.14 will introduce multiple breaking changes which are listed below:
  * **KEDA 2.15** for Kubernetes >=1.31: The removal of [Pod Identity support](https://github.com/kedacore/keda/issues/5035). If you use pod identity, we recommend you move over to [workload identity for your authentication](https://learn.microsoft.com/azure/aks/keda-workload-identity). 
  * **KEDA 2.14** for Kubernetes = 1.30: The removal of [Azure Data Explorer 'metadata.clientSecret' as it was not safe for managing secrets](https://github.com/kedacore/keda/issues/4514).
  * **KEDA 2.14** for Kubernetes = 1.30: Removal of the [deprecated metricName from trigger metadata section](https://github.com/kedacore/keda/issues/4240). The two impacted Azure Scalers are Azure Blob Scaler and Azure Log Analytics Scaler. If you are using `metricName` today, please move `metricName` outside of trigger metadata section  to`trigger.name` in the trigger section to optionally name your trigger. To view an example of what this would look like, please view the open [GitHub issue](https://github.com/Azure/AKS/issues/4471).
* AKS will no longer support the [GPU image (preview)](https://github.com/Azure/AKS/issues/4472) to provision GPU-enabled AKS nodes. Starting on Jan 10, 2025 you will no longer be able to create new GPU-enabled node pools with the GPU image. Alternative options that are supported today and recommended by AKS include the default experience with manual NVIDIA device plugin installation or the NVIDIA GPU Operator, detailed in [AKS GPU node pool documentation](https://learn.microsoft.com/azure/aks/gpu-cluster?tabs=add-ubuntu-gpu-node-pool#confirm-that-gpus-are-schedulable).
* Starting on January 1, 2025, [invalid values sent to the Azure AKS API for the properties.mode field of AKS AgentPools will be rejected](https://github.com/Azure/AKS/issues/4468). Prior to this change, unknown modes were assumed to be User. The only valid values for this field are the (case-sensitive) strings:["User", "System"](https://learn.microsoft.com/rest/api/aks/agent-pools/create-or-update?view=rest-aks-2024-02-01&tabs=HTTP#agentpoolmode), or ["Gateway"](https://learn.microsoft.com/rest/api/aks/agent-pools/create-or-update?view=rest-aks-2024-06-02-preview&tabs=HTTP#agentpoolmode).

### Release Notes

* Features:
  * AKS patch versions 1.28.13, 1.29.8, 1.30.4 are now available. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.

* Bug fixes:
  * Bug fix to address the issue where the OSDiskSize validator throws an error if the existing agent pool does not have a default value set
  * Bug fix causing cluster creation to fail when creating a new cluster with multiple agent pools using the [Dynamic Pod IP Allocation feature (podsubnet)](https://learn.microsoft.com/azure/aks/configure-azure-cni-dynamic-ip-allocation)
  * Resolved a race condition that could occur when deleting a [CNI Overlay](https://learn.microsoft.com/azure/aks/concepts-network-azure-cni-overlay) cluster with auto-scaler enabled, ensuring smoother cluster deletion.

* Behavior change:
  * Abandoned cluster will be [deallocated with status `Failed(Deallocated)` instead of `Succeeded (Stopped)`](https://learn.microsoft.com/azure/aks/support-policies#:~:text=Stopped%2C%20deallocated%2C%20and%20Not%20Ready%20nodes).
  * PDB drain errors will now include additional PDB debug message and appropriate original error instead of generic "API call to Kubernetes API Server failed" error message. Example - "PDB debug info: myNode/myPod1 blocked by pdb myPDB (MaxUnavailable: 1) with 1 unready pods: myNode/myPod2".
  * Updated [Azure NPM version to v1.5.36](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.36) to address race condition in Azure NPM Linux which can occur when editing/deleting a NetworkPolicy with "enough" rules. The race can result in unexpected connectivity for traffic to/from Pods on the impacted Node. NPM will now auto-restart to mitigate the issue ~15 seconds after if it enters a broken state caused by the race.
  * Lowering Linux Azure NPM's CPU request from 250m to 50m. This addresses [Github Issue 2792](https://github.com/Azure/AKS/issues/2792.
  *Clusters using the [Key Management Service (KMS)](https://learn.microsoft.com/azure/aks/use-kms-etcd-encryption) plugin based on Azure Key Vault with a private endpoint and konnectivity tunnel may run into a deadlock issue resulting in `apiserver` becoming unreachable. Clusters using this configuration will not be allowed starting Kubernetes version >= 1.31.
  * Allow Istio add-on users to add the [customizations to the Ingress gateway](https://learn.microsoft.com/azure/aks/istio-deploy-ingress#ingress-gateway-service-annotation-customization).
  * Busybox will be removed from [kube-proxy](https://learn.microsoft.com/azure/aks/configure-kube-proxy) init container. This will eliminate the need for security updates on busybox.

* Component updates:
  * All revisions of [Azure Service Mesh](https://learn.microsoft.com/azure/aks/servicemesh-about) use zipkin as the default tracer config.
  * [Cost-analysis-agent](https://learn.microsoft.com/azure/aks/cost-analysis) image upgraded from v0.0.16 to v0.0.17.
  * Updated [retina](https://github.com/microsoft/retina/releases/tag/v0.0.15) linux to v0.0.15.
  * Updated [ip-masq-agent](https://github.com/Azure/ip-masq-agent-v2/compare/v0.1.11...v0.1.13) to v0.1.13 to address [CVE-2024-24790](https://nvd.nist.gov/vuln/detail/CVE-2024-24790), [CVE-2023-45288](https://nvd.nist.gov/vuln/detail/CVE-2023-45288), [CVE-2023-45289](https://nvd.nist.gov/vuln/detail/CVE-2023-45289), [CVE-2023-45290](https://nvd.nist.gov/vuln/detail/CVE-2023-45290), [CVE-2024-24783](https://nvd.nist.gov/vuln/detail/CVE-2024-24783), [CVE-2024-24784](https://nvd.nist.gov/vuln/detail/CVE-2024-24784), [CVE-2024-24785](https://nvd.nist.gov/vuln/detail/CVE-2024-24785), [CVE-2024-24789](https://nvd.nist.gov/vuln/detail/CVE-2024-24789), [CVE-2024-24791](https://nvd.nist.gov/vuln/detail/CVE-2024-24791), [CVE-2024-5321](https://nvd.nist.gov/vuln/detail/CVE-2024-5321). 
  * Updated [CNI versions to v1.5.35 and v1.6.5. Updated CNS versions to v1.5.35 and v1.6.5](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.5).
  * Updated [Azure Container Instances (ACI) connector addon](https://learn.microsoft.com/azure/aks/virtual-nodes) to [v1.6.2](https://github.com/virtual-kubelet/azure-aci/releases/tag/v1.6.2) and init-validation to v0.3.0.
  * Azure Monitor managed service for Prometheus images updated to [09-16-2024 release](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-09-16-2024).
  * Updated Azure Disk CSI driver version to [v1.29.9](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.9)  on AKS 1.28, 1.29, and to [v1.30.4](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.30.4) on AKS 1.30.
  * Updated Azure File CSI driver to [v1.29.8](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases) on AKS 1.28.
  * Updated [tigera operator to v1.30.11](https://github.com/tigera/operator/releases/tag/v1.30.11) and [calico to v3.26.5](https://github.com/projectcalico/calico/releases/tag/v3.26.5) for versions running on k8s 1.29 and 1.30 to address CVE patches.
  * Updated the [Advanced Container Networking Services](https://learn.microsoft.com/azure/aks/advanced-container-networking-services-overview) Image tag for fixing the [bug](https://github.com/Azure/AKS/issues/4525) that causes cilium pods to crash in Advanced Container Networking Service enabled AKS clusters.
  * Retina Enterprise and Operator image update [v0.1.0].
  * Updated the Windows containerd version from v1.6.21 to [v1.6.35](https://github.com/containerd/containerd/releases/tag/v1.6.35) for Kubernetes version < 1.28.
  * AKS Windows Server 2022 image has been updated to [AKSWindows-2022-20348.2700.240911](vhd-notes/AKSWindows/2022/20348.2700.240911.txt).
  * AKS Windows Server 2019 image has been updated to [AKSWindows-2019-17763.6293.240911](vhd-notes/AKSWindows/2019/17763.6293.240911.txt).
  * Azure Linux image has been updated to [Azure Linux-202409.09.0](vhd-notes/AzureLinux/202409.09.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-202409.09.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202409.09.0.txt).

## Release 2024-08-27

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as v20240827.

### Announcements

* AKS version 1.27 is now deprecated. Enable [long-term support for AKS versions](https://learn.microsoft.com/azure/aks/long-term-support) if you still need to operate on 1.27.
* The attestation report for [CIS Kubernetes V1.9.0 Benchmark](https://learn.microsoft.com/azure/aks/cis-kubernetes) is published which covers AKS 1.27.x through AKS 1.29.x.
* AKS will be upgrading the KEDA addon to more recent KEDA versions. The AKS team has added KEDA 2.15 on AKS clusters with K8s versions >=1.31, KEDA 2.14 for Kubernetes v1.30. KEDA 2.15 and KEDA 2.14 will introduce multiple breaking changes which are listed below:
  * **KEDA 2.15** for Kubernetes >=1.31: The removal of [Pod Identity support](https://github.com/kedacore/keda/issues/5035). If you use pod identity, we recommend you move over to [workload identity for your authentication](https://learn.microsoft.com/azure/aks/keda-workload-identity). 
  * **KEDA 2.14** for Kubernetes = 1.30: The removal of [Azure Data Explorer 'metadata.clientSecret' as it was not safe for managing secrets](https://github.com/kedacore/keda/issues/4514).
  * **KEDA 2.14** for Kubernetes = 1.30: Removal of the [deprecated metricName from trigger metadata section](https://github.com/kedacore/keda/issues/4240). The two impacted Azure Scalers are Azure Blob Scaler and Azure Log Analytics Scaler. If you are using `metricName` today, please move `metricName` outside of trigger metadata section  to`trigger.name` in the trigger section to optionally name your trigger. To view an example of what this would look like, please view the open [GitHub issue](https://github.com/Azure/AKS/issues/4471).

### Release Notes

* Features:
  * Existing Linux node pools can now be updated to enable or disable Federal Information Process Standard (FIPS). See [documentation](https://learn.microsoft.com/azure/aks/enable-fips-nodes#update-an-existing-node-pool-to-enable-or-disable-fips) for more information.

* Bug fixes:
  * Fix an Azure NPM issue that user could meet unexpected connectivity for Pods on the Node when editing a NetworkPolicy with a CIDR "except" field.
  * Fix bug to block non-VMSS (VirtualMachineScaleSets) agent pools in the Automatic SKU validation process.
  * Fix bug to ensure correct default network plugin settings for Kubernetes clusters using VMAS.
  * Fix bug for intermittent precondition failures when applying an AKS Bicep deployment on the pod subnet delegation.
  * Fix bug of public IP on VMSS dropped after upgrade node image or reset service principal operation.
  * Fix bug https://github.com/Azure/AKS/issues/4282 to remove duplicated toleration from Calico components.
  * Fix bug to ensure `AnnotationControlled` is correctly populated by default when creating AKS clusters with app routing enabled, and to ensure `AnnotationControlled` is an accepted value for the [default nginx ingress controller config](https://learn.microsoft.com/azure/aks/app-routing-nginx-configuration?tabs=bicep#control-the-default-nginx-ingress-controller-configuration) for AKS clusters with K8s versions <1.30.
  * Fix bug for [Cluster Autoscaler](https://github.com/Azure/AKS/issues/4286) that requires an implementation of the `HasInstance` method on AKS. This implementation prevents the Cluster Autoscaler from stalling during scale-up due to node scale-down issues.
  * Fix bug https://github.com/Azure/azure-service-operator/issues/3220 to allow creation of AgentPools without `Count` field specified if autoscaler enabled.
  * Fix bug to accept user to set the `PowerState` field for API versions that do not support the filed. Impacted API versions are 2020-09-01, 2020-11-01, 2020-12-01, 2021-02-01 and 2021-03-01.

* Behavior change:
  * For non-host network pods running on AKS nodes, they cannot access wireserver(168.63.129.16) port 32526. Before this change user cannot access wireserver port 80, but port 32526 is accessible.
  * When deploying an [AKS Automatic (preview)](https://learn.microsoft.com/azure/aks/intro-aks-automatic) cluster, user do not need to register extra feature flags for related preview features, such as APIServerVnetIntegration, NRGLockdown, NodeAutoProvisioning, and Safeguards.
  * CBL-Mariner 1.0 is end of life, creation of new nodepools with OSSKU cblmariner is disabled.
  * [Application Gateway Ingress Controller addon](https://learn.microsoft.com/azure/application-gateway/ingress-controller-overview) has been assigned the network contributor role.

* Component updates:
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-202408.27.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202408.27.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202408.27.0](vhd-notes/AzureLinux/202408.27.0.txt). 
  * Azure Disk CSI driver has been upgraded to [v1.30.3](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.30.3) on AKS 1.30, [V1.29.8](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.8) on AKS 1.28, [1.28.1](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.28.10) on AKS 1.27.
  * Azure Blob Disk CSI driver has been upgraded to [v1.24.3](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.24.3) on AKS 1.30, [v1.23.7](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.23.7) on AKS 1.29 and 1.28.
  * Azure File CSI driver has been upgraded to [v1.30.5](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.5) on AKS 1.30 and 1.29, [v1.29.7](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.7) on AKS 1.28.
  * AKS Windows Server 2019 image has been updated to [AKSWindows-2019-17763.6189.240814](vhd-notes/AKSWindows/2019/17763.6189.240814.txt).
  * AKS Windows Server 2022 image has been updated to [AKSWindows-2022-20348.2655.240814](vhd-notes/AKSWindows/2022/20348.2655.240814.txt).
  * AKS App Routing operator image has been updated to [v0.2.3-patch-2](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.3-patch-2) for AKS cluster with K8s versions >=1.30, [v0.2.1-patch-4](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.1-patch-4) for AKS cluster with K8s versions <1.30 to address CVEs.
  * Windows containerd has been updated to [v1.7.20](https://github.com/containerd/containerd/releases/tag/v1.7.20) in AKS cluster with K8s versions >= v1.28.
  * Kubernetes Secrets Store CSI Driver has been updated to [v1.4.4](https://github.com/kubernetes-sigs/secrets-store-csi-driver/releases/tag/v1.4.4) and Azure Key Vault Provider for Secrets Store CSI Driver to [v1.5.3](https://github.com/Azure/secrets-store-csi-driver-provider-azure/releases/tag/v1.5.3)
  * [Application Gateway Ingress Controller add-on](https://learn.microsoft.com/azure/application-gateway/ingress-controller-overview) image has been updated to [v1.7.5](https://github.com/Azure/application-gateway-kubernetes-ingress/releases/tag/1.7.5).
  * Retina Enterprise and Operator image has been updated to [v0.0.9](https://github.com/azure-networking/retina-enterprise/releases/tag/v0.0.9).
  * azure-cloud-controller-manager has been updated to version [v1.30.5](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.30.5), [v1.29.9](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.29.9), [v1.28.11](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.28.11), [v1.27.19](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.27.19).
  * KEDA addon has been updated to [v2.14.1](https://github.com/kedacore/keda/releases/tag/v2.14.1) for Kubernetes = 1.30.
  * Azure Policy addon has been updated to [v1.7.0](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#170).
  * Istio-based service mesh add-on revision asm-1-20 has been upgraded to patch [v1.20.8](https://istio.io/latest/news/releases/1.20.x/announcing-1.20.8/), revision asm-1-21 has been upgraded to patch [v1.21.5](https://istio.io/latest/news/releases/1.21.x/announcing-1.21.5), and revision asm-1-22 has been upgraded to patch [v1.22.3](https://istio.io/latest/news/releases/1.22.x/announcing-1.22.3). Users can restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade#patch-version-upgrade).
  * Calico [v3.28.1](https://github.com/projectcalico/calico/releases/tag/v3.28.1) is supported for AKS cluster with K8s versions 1.30.

## Release 2024-08-05

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as v20240805.

### Announcements

* AKS will be upgrading the KEDA addon to more recent KEDA versions. The AKS team has added KEDA 2.15 on AKS clusters with K8s versions >=1.31, KEDA 2.14 for Kubernetes v1.30. KEDA 2.15 and KEDA 2.14 will introduce multiple breaking changes which are listed below:
  * **KEDA 2.15** for Kubernetes >=1.31: The removal of [Pod Identity support](https://github.com/kedacore/keda/issues/5035). If you use pod identity, we recommend you move over to [workload identity for your authentication](https://learn.microsoft.com/azure/aks/keda-workload-identity). 
  * **KEDA 2.14** for Kubernetes = 1.30: The removal of [Azure Data Explorer 'metadata.clientSecret' as it was not safe for managing secrets](https://github.com/kedacore/keda/issues/4514).
  * **KEDA 2.14** for Kubernetes = 1.30: Removal of the [deprecated metricName from trigger metadata section](https://github.com/kedacore/keda/issues/4240). The two impacted Azure Scalers are Azure Blob Scaler and Azure Log Analytics Scaler. If you are using `metricName` today, please move `metricName` outside of trigger metadata section  to`trigger.name` in the trigger section to optionally name your trigger. To view an example of what this would look like, please view the open [GitHub issue](https://github.com/Azure/AKS/issues/4471).

### Release Notes

* Features:
  * AKS version 1.30 is now available and will be the next LTS version of AKS. You can now [upgrade your 1.27 clusters](https://learn.microsoft.com/azure/aks/long-term-support#migrate-from-lts-to-the-next-lts-release) to 1.30 during the LTS period.
  * [Updating an existing node pool to enable or disable FIPS](https://learn.microsoft.com/azure/aks/enable-fips-nodes#update-an-existing-node-pool-to-enable-or-disable-fips) is now Generally Available.
  * AKS patch versions 1.30.3, 1.29.7, 1.28.12, 1.27.16 are now available. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.
  * Istio add-on now only allows `EnvoyFilter`s of the types Lua, local rate limiting, and gzip compression.
  * [Telemetry API v1](https://istio.io/latest/blog/2024/v1-apis/#telemetry-api) is now available for the Istio based service mesh add-on.
  * The [AKS extension for Visual Studio Code](https://learn.microsoft.com/azure/aks/aks-extension-vs-code) now supports the ability to attach an ACR to your cluster, generate Kubernetes deployment files, generate Dockerfiles, and generate GitHub Actions.
  * The [ignore-daemonsets-utilization, daemonset-eviction-for-empty-nodes, and daemonset-eviction-for-occupied-nodes parameters](https://learn.microsoft.com/azure/aks/cluster-autoscaler?tabs=azure-cli#cluster-autoscaler-profile-settings) on the cluster autoscaler profile are GA from API version 2024-05-01 onwards. If you are using the CLI to update these flags, please ensure you are using version 2.63 or later.
  
* Bug fixes:
  * Fixed a bug where sometimes `NodePublicIPPrefixID` could show unset on a cluster even though it was set.
  * Previously, as part of Istio addon canary upgrade, users had to manually copy their edits to HorizontalPodAutoscaler from old revision to new revision. This has been fixed so that changes done to Horizontal Pod Autoscaler will be automatically copied for the newer revision.
  * Added validation that if a LTS cluster has a node pool on non-LTS version, upgrade to the next LTS version is blocked.

* Behavior change:
  * When [Advanced Networking Observability](https://learn.microsoft.com/azure/aks/advanced-network-observability-concepts?tabs=non-cilium) is enabled, increased memory limit of 700Mi (from 400Mi) is used for retina-agent.
  * `GOMAXPROCS` for coredns has been set to equal CPU limit to avoid throttling.
  * In Azure CNI, `init-cni-dropgz` initContainer has been renamed to `cni-installer`.
  * Validation for minimum 5 minutes has been introduced for [drain timeout value](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#set-node-drain-timeout-value) to prevent drain issues during upgrade.
  * `query` label removed from `dns` metrics in [Advanced Network Observability](https://learn.microsoft.com/azure/aks/advanced-network-observability-concepts?tabs=non-cilium#features-of-advanced-network-observability).
  * [Control plane only AKS upgrades](https://learn.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-upgrade) will now reconcile node pools to desired state. For example, previously let's say a user did did a Kubernetes upgrade and network plugin mode transition to overlay where a reimaging of the nodes was required, but it wasn't done as nodes were skipped. Going ahead nodes will be reconciled in these circumstances.
  
* Component updates:
  * To address scheduler issues fixed in this upstream [change](https://github.com/kubernetes/kubernetes/pull/124933), 1.27.15, 1.28.11, 1.29.6 schedulers versions will be used for Kubernetes versions 1.27.14, 1.28.10, 1.29.5 respectively.
  * Updated Azure Blob CSI driver to [v1.22.7](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.22.7) on AKS version 1.27.
  * For Node Auto Provisioning, Azure provider of Karpenter is upgraded to [v0.5.1](https://github.com/Azure/karpenter-provider-azure/releases/tag/v0.5.1).
  * Updated Azure Monitor Container Insights image to [v3.1.23](https://github.com/microsoft/Docker-Provider/releases/tag/3.1.23).
  * Azure Monitor managed service for Prometheus images updated to [07-19-2024 release](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-07-19-2024).
  * Updated Eraser version to [v1.3.1](https://github.com/eraser-dev/eraser/releases/tag/v1.3.1) for [Image Cleaner](https://learn.microsoft.com/azure/aks/image-cleaner).
  * Updated Azure Disk CSI driver to [v1.28.9](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.28.9) on AKS 1.27 and to [v1.29.7](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.7) on AKS 1.28 and 1.29.
  * Updated Azure File CSI driver to [v1.28.11](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.11) on AKS 1.27, to [v1.29.6](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.6) on AKS 1.28, and to [v1.30.3](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.3) on AKS 1.29.
  * Updated Ratify image used in [Image Integrity](https://learn.microsoft.com/azure/aks/image-integrity) to [v1.2.0](https://github.com/ratify-project/ratify/releases/tag/v1.2.0).
  * Updated Cilium version has been updated to [1.14.12](https://github.com/cilium/cilium/releases/tag/v1.14.12) for AKS cluster with versions >= 1.29 and [Advanced Network Observability](https://learn.microsoft.com/azure/aks/advanced-network-observability-concepts?tabs=non-cilium#features-of-advanced-network-observability) enabled.
  * Istio-based service mesh add-on revision asm-1-21 has been upgraded to patch [v1.21.4](https://github.com/istio/istio/releases/tag/1.21.4) and revision asm-1-22 has been upgraded to patch [v1.22.2](https://github.com/istio/istio/releases/tag/1.22.2). Users can restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade).
  * Updated Windows Kubernetes packages in all AKS versions to address [CVE-2024-5321](https://nvd.nist.gov/vuln/detail/CVE-2024-5321).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-202407.29.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202407.29.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202407.29.0](vhd-notes/AzureLinux/202407.29.0.txt).
  * AKS Windows Server 2019 image has been updated to [AKSWindows-2019-17763.6054.240716](vhd-notes/AKSWindows/2019/17763.6054.240716.txt).
  * AKS Windows Server 2022 image has been updated to [AKSWindows-2022-20348.2582.240716](vhd-notes/AKSWindows/2022/20348.2582.240716.txt).

## Release 2024-07-16

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as v20240716.

### Announcements

* 1.30 is the next LTS version after 1.27. Upgrade from 1.27 LTS to 1.30 LTS will be possible starting August 2024. More information about AKS LTS is available [here](https://learn.microsoft.com/azure/aks/long-term-support).
* 1.27 community supported has ended. To exit [Long-Term Support (LTS)](https://learn.microsoft.com/azure/aks/long-term-support) upgrade to 1.28 and disable LTS with the `az aks upgrade` command.
* A new tutorial has been released on how to [securely scale your applications using the KEDA add-on and workload identity](https://learn.microsoft.com/azure/aks/keda-workload-identity).
* Running `az aks get-versions` now returns all patch versions for an AKS version. Each supported minor version can support any number of patches at a given time. AKS reserves the right to deprecate patches if a critical CVE or security vulnerability is detected. To learn more about AKS versioning, please read the [official documentation](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy).

### Release Notes

* Features:
  * AKS version 1.30 is now GA.
  * AKS patch versions 1.30.2, 1.30.1, 1.29.6, 1.28.11, 1.27.15, are now available. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.
  * [Istio add-on for AKS](https://learn.microsoft.com/azure/aks/istio-about) now supports EnvoyFilter of the type Lua `(type.googleapis.com/envoy.extensions.filters.http.lua.v3.Lua)`. While this `EnvoyFilter` is allowed, any issue arising from the Lua script itself is not supported. Other `EnvoyFilter` types currently remain blocked.
  * The ability to migrate your existing Ubuntu node pools to Azure Linux by changing the [OS SKU of the node pool](https://learn.microsoft.com/azure/azure-linux/tutorial-azure-linux-migration?tabs=azure-cli#in-place-os-sku-migration-preview) is now GA.

* Preview features:
  * CNI Overlay dual-stack (IPv4/IPv6) is now available on Windows Agent Pools.
  * Existing node pools can now be updated to enable or disable Federal Information Process Standard (FIPS). See [aka.ms/aks/updatefips](https://aka.ms/aks/updatefips) for more information.
  
* Bug Fixes:
  * Updated iptables rules in clusters with Azure Network Policy Manager to block pod access to wireserver.
  * A bug regarding App Routing's [placeholderPod](https://github.com/Azure/aks-app-routing-operator/blob/main/CHANGELOG.md#023-patch-1---2024-07-10) not properly cleaning up has been fixed.
  * A [regression in kube-scheduler impacting Kubernetes versions 1.27.14, 1.28.10, 1.29.5](https://github.com/Azure/AKS/issues/4434) has been fixed in new patch versions 1.30.2, 1.30.1, 1.29.6, 1.28.11, 1.27.15. If you are on the affected patch versions. Please follow release [v20240716](https://releases.aks.azure.com/) and upgrade your AKS version once the release is in your region.

* Behavior Change:
  * AKS Automatic clusters now use Azure Linux for Node Auto Provision dynamic nodes.
     
* Component Updates:
  * AKS has released new patches v1.29.5 and v1.30.4 for [Cluster Autoscaler](https://github.com/kubernetes/autoscaler) to fix a bug which prevents scaling from zero of selected SKUs.
  * coreDNS has been updated to use image v1.9.4-hotfix.20240704 to fix CVE vulnerabilities.
  * [Istio add-on](https://learn.microsoft.com/azure/aks/istio-about) has been bumped to include v1.21.3-hotfix.20240626 and v1.22.1-hotfix.20240626 to fix [datadog tracer zero-day crash](https://github.com/envoyproxy/envoy/security/advisories/GHSA-8mq4-c2v5-3h39).
  * [KEDA add-on](https://learn.microsoft.com/azure/aks/keda-about) has been updated to [v2.14.0](https://github.com/kedacore/keda/releases/tag/v2.14.0) on AKS v1.30.
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-202407.08.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202407.08.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202407.08.0](vhd-notes/AzureLinux/202407.08.0.txt).

## Release 2024-06-27

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as v20240627.

### Announcements

* Starting 1.30 Kubernetes version and 1.27 LTS versions, beta APIs will be disabled by default, when you upgrade to [them](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#before-you-begin). There will be an option provided to explicitly enable beta APIs closer to the 1.30 release.
* 1.30 is the next LTS version after 1.27. Upgrade from 1.27 LTS to 1.30 LTS will be possible starting August 2024. More information about AKS LTS is available [here](https://learn.microsoft.com/azure/aks/long-term-support).

### Release Notes

* Features:
  * AKS patch versions  1.27.14, 1.28.10, and 1.29.5, are now available. 1.27.9, 1.28.5, and 1.29.2 patch versions are deprecated. Refer to [version support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy) and [upgrading a cluster](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli) for more information.
  * Cost Analysis views for AKS are now available under AKS resource blade in Azure portal. More information can be found in this [document](https://learn.microsoft.com/azure/cost-management-billing/costs/view-kubernetes-costs).

* Preview feature:
  * [Windows Server Annual Channel for Containers](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/windows-server-annual-channel-for-containers/ba-p/3866248) is now in public preview on Azure Kubernetes Service (AKS). More information can be found [here](https://aka.ms/aks/windowsannualchannel).
  
* Bug Fixes:
  * Fixed a [bug](https://github.com/Azure/AKS/issues/4292) that previously didn't allow switching from non-LTS K8s version to [LTS](https://learn.microsoft.com/azure/aks/long-term-support) K8s version when upgrading the cluster. For example, you can now upgrade from 1.26 to 1.27 while switching to LTS.
  * Related to the above, also fixed a bug where previously it was not possible to upgrade from an LTS K8s version to non-LTS K8s version. For example, you can now upgrade from 1.27 LTS to 1.28.

* Behavior Change
  * The memory limit for [Azure Key Vault provider for Secrets Store CSI Driver](https://learn.microsoft.com/azure/aks/csi-secrets-store-driver) has been updated from 300Mi to 500Mi.
  * Base CPU and memory for metrics-server container are updated from 44M to 150M and 51Mi to 100Mi respectively on clusters with K8s version >= 1.30.0. More information on metrics server scaling can be found [here](https://learn.microsoft.com/azure/aks/use-metrics-server-vertical-pod-autoscaler).
     
* Component Updates:
  * Linux Network Policy Manager has been upgraded from v1.5.23 to [v.1.5.29](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.29) to address [CVE-2024-28085](https://nvd.nist.gov/vuln/detail/CVE-2024-28085).
  * Upgraded Azure workload identity to [v1.3.0](https://github.com/Azure/azure-workload-identity/releases/tag/v1.3.0).
  * Upgraded ip-masq-agent-v2 to [v0.1.11](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.11) having fixes for [CVE-2024-2961](https://nvd.nist.gov/vuln/detail/CVE-2024-2961) and [CVE-2024-33599](https://nvd.nist.gov/vuln/detail/CVE-2024-33599).
  * Upgraded Azure Monitor Container Insights image to [v3.1.22](https://github.com/microsoft/Docker-Provider/releases/tag/3.1.22)
  * Upgraded Azure CNS to [v1.6.0](https://github.com/Azure/azure-container-networking/releases/tag/v1.6.0) for 1.30 version clusters.
  * Istio-based service mesh add-on revision asm-1-19 has been upgraded to patch v1.19.10-hotfix.20240528, asm-1-20 has been upgraded to patch v1.20.7, and asm-1-21 has been upgraded to patch v1.21.3. These contain fixes for [CVE-2024-34362](https://nvd.nist.gov/vuln/detail/CVE-2024-34362), [CVE-2024-32974](https://nvd.nist.gov/vuln/detail/CVE-2024-32974), [CVE-2024-32975](https://nvd.nist.gov/vuln/detail/CVE-2024-32975), [CVE-2024-34363](https://nvd.nist.gov/vuln/detail/CVE-2024-34363), [CVE-2024-34364](https://nvd.nist.gov/vuln/detail/CVE-2024-34364), [CVE-2024-32976](https://nvd.nist.gov/vuln/detail/CVE-2024-32976), [CVE-2024-23326](https://nvd.nist.gov/vuln/detail/CVE-2024-23326).
  Users can restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-202406.19.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202406.19.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202406.19.0](vhd-notes/AzureLinux/202406.19.0.txt).
  * AKS Windows Server 2022 image has been updated to [AKSWindows-2022-20348.2529.240621](vhd-notes/AKSWindows/2022/20348.2529.240621.txt).

## Release 2024-06-09

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/). This release is titled as v20240609.

### Announcements

* Starting 1.30 Kubernetes version and 1.27 LTS versions, beta apis will be disabled by default, when you upgrade to [them](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#before-you-begin). There will be an option provided to explicitly enable beta apis closer to the 1.30 release.
* Istio service mesh addon revision asm-1-19 is no longer supported. If you are still using this revision on your cluster, please upgrade for continued support. More information about mesh upgrades and version support can be found [here](https://aka.ms/asm-aks-upgrade-docs).
* 1.30 is the next LTS version after 1.27. Upgrade from 1.27 LTS to 1.30 LTS will be possible starting January 2025 thus providing a  6 months upgrade window before 1.27 LTS goes End of Life in July 2025. More information about AKS LTS is available [here](https://learn.microsoft.com/azure/aks/long-term-support).
* Container Insights has automatically migrated from legacy authentication to managed authentication on AKS clusters where the Container Insights addon was enabled with legacy authentication. This migration occurs when any feature, such as the cost-analysis addon or authorized IP ranges, is enabled using the preview API version 2023-07-02-preview or later. This unintended migration has caused monitoring to break, this issue has been fixed for new clusters. To mitigate this issue on existing clusters, re-onboarding or re-configuring of Container Insights is required.
* Released improvements to [planned maintenance schedule](https://learn.microsoft.com/azure/aks/planned-maintenance), so that there are no missed or delayed windows. Also you no longer have to wait for 24 hours between the creation or update of a maintenance configuration and the scheduled start time, this will now only take few minutes going forward. 

### Release Notes

* Features:
  * Generally Available - [Security Patch channel - Live patching mechanism](https://learn.microsoft.com/azure/aks/auto-upgrade-node-os-image?tabs=azure-cli) for VHD updates.
  * AKS Patch version [1.27.13](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.27.md#v12713) is now available. 

* Preview Features:
  * AKS version [1.30](https://kubernetes.io/blog/2024/04/17/kubernetes-v1-30-release/) is available in preview. 
    
* Bug Fixes:
  * Updated cilium to version 1.14.10 for K8s version 1.29+, to fix the issue [where the host network is broken and remains broken even if the underlying interface goes up again](https://github.com/cilium/cilium/issues/18706).
  * Removes the post-upgrade annotation on [hubble-generate-cert](https://github.com/cilium/cilium/blob/aa10df3a4c6a9e7bd947a4a32613cedf22b3731d/Documentation/gettingstarted/hubble-configuration.rst#L81) Job. On each aks cluster reconcile, the helm chart revision is incremented which counts as an upgrade. Each time the helm chart is upgraded or installed this job will restart. This change fixes that to not restart on helm chart upgrades and successfully clean up.
   * Fixed two bugs resulting in a wrong [default path](https://github.com/Azure/AKS/issues/4196) and a [deadlock issue](https://github.com/containerd/ttrpc/issues/72#issuecomment-2105545516).
   * Fixed the following issues for [AKS Edge zone support](https://learn.microsoft.com/azure/aks/edge-zones?tabs=azure-resource-manager) -
      * Fixed bug where clusters with ExtendedLocation set would accept create node pool with availability zones even though availability zones aren't supported in ExtendedLocation mode. 
      * Fixed bug where `edgezone` was previously being wrongly accepted in small case. Only `EdgeZone` is accepted.
     
* Component Updates:
  * Windows containerd has been upgraded from v1.7.14 to v1.7.17 in K8s v1.28+.
  * Changing [cilium operator](https://docs.cilium.io/en/stable/internals/cilium_operator/) tolerations to match cilium-agent. Adding tolerations for NoExecute and NoSchedule. This should fix a race condition in upgrades, where cilium-operator cannot schedule due to node taint.
  * Retina Enterprise and Operator image update [v0.0.8](https://github.com/azure-networking/retina-enterprise/releases/tag/v0.0.8).
  * Updated linux cni versions to [v1.4.54](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.54) and [v1.5.28](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.28).
  * Gatekeeper is updated to [3.16](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.16.3) for kubernetes versions 1.27+.
  * Updated Cilium to [v1.13.13](https://github.com/cilium/cilium/releases/tag/v1.13.13) for Kubernetes v1.28.0+.
  * Upgrade azure disk csi-drivers to [1.29.6](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.6) on AKS 1.28 and 1.29. 
  * Updated the aks app routing operator nginx version from 1.9 to [1.10](https://github.com/Azure/aks-app-routing-operator/releases/tag/v0.2.1-patch-1).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-202406.07.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202406.07.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202406.07.0](vhd-notes/AzureLinux/202406.07.0.txt).
  * AKS Windows Server 2019 image has been updated to [AKSWindows-2019-17763.5936.240612](vhd-notes/AKSWindows/2019/17763.5936.240612.txt).
  * AKS Windows Server 2022 image has been updated to [AKSWindows-2022-20348.2527.240612](vhd-notes/AKSWindows/2022/20348.2527.240612.txt).
 
## Release 2024-05-13

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Starting 1.30 Kubernetes version and 1.27 LTS versions, beta apis will be disabled by default, when you upgrade to [them](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#before-you-begin). There will be an option provided to explicitly enable beta apis closer to the 1.30 release.
* Introducing the [AKS blog](https://azure.github.io/AKS/) and the [AKS Youtube community](https://www.youtube.com/@theakscommunity)
* In 2020 Docker enacted a Rate Limiting policy for all users. In-order to assist customers with the change, Microsoft worked directly with Docker to prevent users of Microsoft Azure from being impacted. However, beginning on June 30th, 2024, Azure customers will begin to be impacted by this limit. In-order for customers to mitigate the potential effects of this limit. We recommend customers begin to use the Artifact Cache feature within Azure Container Registry or sign up for a Docker Subscription.  More information is available [here](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/best-practices-for-using-azure-container-registry-and-docker-hub/ba-p/4068979).
* [GetOSOptions](https://github.com/Azure/AKS/issues/4261) will no longer be included in new AKS API versions starting with 2024-05-02. This API was used to get OS options that support Federal Information Process Standard (FIPS) in the specified subscription. If you're calling this API via the CLI, it will no longer be available in newer az aks extension versions. You can use an older version of the az aks extension, however this is not recommended. The CLI preview version supporting the 2024-05-02 preview API can be found [here](https://github.com/Azure/azure-cli-extensions/tree/main/src/aks-preview#released-version-and-adopted-api-version). Check the link for the release version.
For details on what AKS supported operating systems support Federal Information Process Standard (FIPS), see [aka.ms/aks/GetFIPSOSOptions](https://aka.ms/aks/GetFIPSOSOptions).

### Release Notes

* Features:
  * Generally Available - AKS supports [disabling Windows OutboundNAT](https://learn.microsoft.com/azure/aks/nat-gateway#disable-outboundnat-for-windows).
  * Generally Available - [Automated Deployments](https://learn.microsoft.com/azure/aks/automated-deployments).
  * Generally Available - [Security patch channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-os-image?tabs=azure-cli) for VHD updates.
  * Generally Available - [Azure Kubernetes Fleet Manager workload orchestration](https://learn.microsoft.com/azure/kubernetes-fleet/quickstart-resource-propagation)
  * AKS Patch version 1.28.9 is now available. It fixes [Bug - OpenAPI handler fails on duplicated path](https://github.com/kubernetes/kubernetes/issues/122668). 

* Preview Features  
  * Deployment Safeguards now supports [mutations in Enforcement mode](https://learn.microsoft.com/azure/aks/deployment-safeguards). 
  * Enable [Native sidecars](https://learn.microsoft.com/azure/aks/istio-native-sidecar) mode for Istio-based service mesh addon in AKS.
  * [AKS Automatic](https://learn.microsoft.com/azure/aks/intro-aks-automatic). Visit the [AKS engineering blog](https://aka.ms/aks/hello-automatic) post.
  * [Node Initialization Taints](https://learn.microsoft.com/azure/aks/use-node-taints#use-node-initialization-taints-preview).
  * [Advanced Container Networking Services](https://learn.microsoft.com/azure/aks/advanced-container-networking-services-overview) can be enabled on Cilium-enabled clusters with Kubernetes v1.29.0 or greater, and on Retina-enabled clusters with Kubernetes v1.21.0 or greater for Advanced Network Observability.
  * Allow [disabling NPM](https://learn.microsoft.com/azure/aks/use-network-policies#uninstall-azure-network-policy-manager-or-calico-preview) for existing clusters with "networkPolicy=none" for stable api version 2024-05-01. 
  * [Property-based scheduling](https://learn.microsoft.com/azure/kubernetes-fleet/intelligent-resource-placement) in Azure Kubernetes Fleet Manager.
  * [Cluster resource overrides](https://learn.microsoft.com/azure/kubernetes-fleet/cluster-resource-override) in Azure Kubernetes Fleet Manager.
  * [Service Connector on AKS cluster](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/simplify-your-azure-kubernetes-service-connection-configuration/ba-p/4151594). It simplifies the connection configuration experience for AKS workloads and Azure backing services such as [Azure Key Vault](https://learn.microsoft.com/azure/service-connector/tutorial-python-aks-keyvault-csi-driver), [Storage account](https://learn.microsoft.com/azure/service-connector/tutorial-python-aks-storage-workload-identity) and [Azure OpenAI](https://learn.microsoft.com/azure/service-connector/tutorial-python-aks-openai-workload-identity).
 
* Behavioral Changes:
  * [Node upgrade (reimage)](https://learn.microsoft.com/azure/aks/auto-upgrade-node-os-image) will wait for disk detach to complete (to prevent very slow disk detach).
  * Default [network policy](https://learn.microsoft.com/azure/aks/use-network-policies) is "networkPolicy=none" when network policy is not set on new clusters starting from API version 2024-05-01.
  * Customized apiserver subnet must be empty when migrating a cluster to enable [apiserver-vnet-integration](https://learn.microsoft.com/azure/aks/api-server-vnet-integration). If the subnet has resources in it, the migration won't be allowed.
   
* Bug Fixes:
  * Metrics Server v0.6.3 will be used to prevent frequent OOMKills, reverting from v0.7.1.
  * [Allowing zonal NodeClaims to facilitate NodeClaims and node creation](https://github.com/Azure/karpenter-provider-azure/issues/339) on [Node Auto Provisioning](https://learn.microsoft.com/azure/aks/node-autoprovision) for workloads with zone affinity constraints. 
  * Fixed a bug where the [SSHAccess property of a node pool](https://learn.microsoft.com/azure/aks/manage-ssh-node-access) would be reset to LocalUser(SSHAccess:LocalUser) on a partial put. Henceforth, SSHAccess property will retain the current value (SSHAccess:current value).
  * Fixed bug where the eTag property in 2024-02-02 preview, 2024-03-02 preview, and 2024-04-02 preview APIs was returned with the wrong case (returned etag, should have been eTag).

* Component Updates: 
  * Istio-based service mesh add-on revision asm-1-19 has been upgraded to patch v1.19.10, asm-1-20 has been upgraded to patch v.1.20.6, and asm-1-21 has been upgraded to patch v1.21.2. Users can restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade).
  * Linux and Windows addon-token-adapter image for Azure monitoring metrics is updated to mcr.microsoft.com/aks/msi/addon-token-adapter:master.240510.2. The updated image patches [CVE-2023-4911](https://www.cve.org/CVERecord?id=CVE-2023-4911),[CVE-2024-2961](https://www.cve.org/CVERecord?id=CVE-2024-2961), [CVE-2024-33599](https://www.cve.org/CVERecord?id=CVE-2024-33599), [CVE-2024-33600](https://www.cve.org/CVERecord?id=CVE-2024-33600), [CVE-2024-33601](https://www.cve.org/CVERecord?id=CVE-2024-33601), [CVE-2024-33602](https://www.cve.org/CVERecord?id=CVE-2024-33602), [CVE-2023-3446](https://www.cve.org/CVERecord?id=CVE-2023-3446), [CVE-2023-3817](https://www.cve.org/CVERecord?id=CVE-2023-3817), [CVE-2023-3446](https://www.cve.org/CVERecord?id=CVE-2023-3446), [CVE-2023-3817](https://www.cve.org/CVERecord?id=CVE-2023-3817). 
  * Managed Prometheus image version updated to [images:6.8.12-main-05-21-2024](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md).
  * Azure Policy addon has been updated to [v1.4.0](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#140) for all clusters on Kubernetes version >= v1.25.
  * Updated cloud node manager to [v1.30.0](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.30.0) on AKS 1.30+, [v1.29.4](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.29.4) on AKS 1.29+, [v1.28.9](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.28.9) on AKS 1.28+, [v1.27.17](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.27.17) on AKS 1.27+. Refer [AKS version matrix for cloud node manager](https://github.com/kubernetes-sigs/cloud-provider-azure#aks-version-matrix).
  * Updated AKS App Routing operator image to [v0.2.3](https://github.com/Azure/aks-app-routing-operator/blob/main/CHANGELOG.md#023---2024-04-22).
  * Updated Azure File CSI driver to [v1.28.10](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.10)  on AKS 1.27, [v1.29.5](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.5)  on AKS 1.28, [v1.30.2](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.2) on AKS 1.29.
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202405.20.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202405.20.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202405.20.0](vhd-notes/AzureLinux/202405.20.0.txt).
  * AKS Windows Server 2019 image has been updated to [AKSWindows-2019-17763.5820.240516](vhd-notes/AKSWindows/2019/17763.5820.240516.txt).
  * AKS Windows Server 2022 image has been updated to [AKSWindows-2022-20348.2461.240516](vhd-notes/AKSWindows/2022/20348.2461.240516.txt).
 
## Release 2024-04-28

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Starting 1.30 Kubernetes version and 1.27 LTS versions, beta apis will be disabled by default, when you upgrade to [them](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#before-you-begin). There will be an option provided to explicitly enable beta apis closer to the 1.30 release.
* On 15 March 2027, Windows Server 2022 will be retired when Kubernetes 1.34 reaches the end of platform support. You won't be able to create new Windows Server 2022 node pools on Kubernetes 1.35 and above. We encourage you to make the switch before 15 March 2027 to gain the richer benefits of [Windows Server 2025](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/introducing-windows-server-2025/ba-p/4026374) or Windows Server [Annual Channel](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/windows-server-annual-channel-for-containers/ba-p/3866248). These new Windows OS versions will be supported on AKS before Windows Server 2022 is retired. For more updates, see our [AKS public roadmap](https://github.com/Azure/AKS/projects/1).
* In 2020 Docker enacted a Rate Limiting policy for all users. In-order to assist customers with the change, Microsoft worked directly with Docker to prevent users of Microsoft Azure from being impacted. However, beginning on June 30th, 2024, Azure customers will begin to be impacted by this limit. In-order for customers to mitigate the potential effects of this limit. We recommend customers begin to use the Artifact Cache feature within Azure Container Registry or sign up for a Docker Subscription.  More information is available [here](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/best-practices-for-using-azure-container-registry-and-docker-hub/ba-p/4068979)
* If you use any programming/scripting logic to list and select a minor version of Kubernetes before creating clusters with the `ListKubernetesVersions` API, note that starting from Kubernetes v1.27, the API returns `SupportPlan` as `[KubernetesOfficial, AKSLongTermSupport]`. Please ensure you update any logic to exclude `AKSLongTermSupport` versions to avoid any breaks and choose `KubernetesOfficial` support plan versions.  Otherwise, if LTS is indeed your path forward please first opt-into the Premium tier and the `AKSLongTermSupport` support plan versions from the `ListKubernetesVersions` API before creating clusters.  Refer [long term support](https://learn.microsoft.com//azure/aks/long-term-support) for more information.

### Release Notes

* Features:
  * With this release, Azure Linux 2.0 becomes a supported OS for AKS Long Term Support (LTS) with v1.27. Learn more about [Azure Linux and LTS](https://aka.ms/aks-azurelinux-lts).
  * You can now get insight into the progress of any ongoing operation, such as create, upgrade, and scale, using any preview API version after `2024-01-02-preview` using the Get/List operations call. Refer to [Long running operations on an Azure Kubernetes Service (AKS) cluster](https://learn.microsoft.com/azure/aks/manage-abort-operations?tabs=azure-cli) for more information.
  * AKS patch version 1.29.4 is now available.

* Behavioral Changes:
  * Manually added Labels, Taints, and Annotations on nodes will no longer be copied to nodes during surged upgrade. To ensure any Label or Taint is present in new nodes please use the [Labels and/or Taints](https://learn.microsoft.com/azure/aks/use-labels) functionality provided by AKS. 
  * The Istio-based service mesh add-on now skips validation of its compatibility with cluster version unless mesh upgrade or cluster upgrade is attempted.
  * Effective starting with Kubernetes version 1.29, when you deploy Azure Kubernetes Service (AKS) clusters across multiple availability zones, AKS now utilizes zone-redundant storage (ZRS) to create managed disks within built-in storage classes. ZRS ensures synchronous replication of your Azure managed disk across multiple Azure availability zones in your chosen region. This redundancy strategy enhances the resilience of your applications and safeguards your data against datacenter failures. Refer to [Storage concept](https://learn.microsoft.com/azure/aks/concepts-storage#storage-classes) for more information.
  
* Bug Fixes:
  * Fixed a bug that incorrectly calculated number of free IPs in a subnet when upgrading an agent pool using Azure CNI with Dynamic IP allocation.
  * Fixed a bug to allow correct IP address calculation for subnets with Private Link Service.
  * Fixed a bug where the ordering of the system environment variables injected into pods could change.
  * Fixed a bug in clusters that use [Node Autoprovisioning](https://learn.microsoft.com/azure/aks/node-autoprovision) for stateful workloads deployments that use availability zones.
  * Fixed a bug in clusters that use [Node Autoprovisioning](https://learn.microsoft.com/azure/aks/node-autoprovision) and managed identity to authenticate Azure Container Registry.
  * Fixed an issue where clusters using Pod Identity would fail to migrate to Azure CNI.
  * The Istio-based service mesh add-on components can now tolerate running on the [system node pools](https://learn.microsoft.com/azure/aks/use-system-pools?tabs=azure-cli#system-and-user-node-pools) with the `CriticalAddonsOnly` taint.
  * Fixed an issue where the ephemeral disk placement was incorrectly modified/updated on an existing nodepool.

* Component Updates: 
  * Upgraded Kubernetes Secrets Store CSI Driver to [v1.4.3](https://github.com/kubernetes-sigs/secrets-store-csi-driver/releases/tag/v1.4.3) and Azure Key Vault Provider for Secrets Store CSI Driver to [v1.5.2](https://github.com/Azure/secrets-store-csi-driver-provider-azure/releases/tag/v1.5.2)  
  * The Istio-based service mesh add-on has been patched to versions [1.19.9](https://istio.io/latest/news/releases/1.19.x/announcing-1.19.9/), [1.20.5](https://istio.io/latest/news/releases/1.20.x/announcing-1.20.5/), and [1.21.1](https://istio.io/latest/news/releases/1.21.x/announcing-1.21.1/) to address [CVE-2024-27919](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-27919), [CVE-2024-30255](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-30255), [CVE-2023-45288](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-45288), and [GHSA-3mh5-6q8v-25wj](https://github.com/envoyproxy/envoy/security/advisories/GHSA-3mh5-6q8v-25wj). Workloads need to be restarted by user to consume the latest version of `istio-proxy` sidecar. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade).
  * Open Service Mesh (OSM) add-on has been patched to address [GHSA-3mh5-6q8v-25wj](https://github.com/envoyproxy/envoy/security/advisories/GHSA-3mh5-6q8v-25wj).
  * Upgraded Azure Disk CSI driver to [v1.30.1](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.1) on AKS 1.29.
  * Upgraded Azure File CSI driver to [v1.29.7](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.7) on AKS 1.27 and to [v1.29.4](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.4) on AKS 1.28 on Windows nodes.
  * Upgraded Azure Blob CSI driver to [v1.24.1](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.24.1) on AKS 1.28 and to [v1.22.6](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.22.6) on AKS 1.27.
  * Upgraded Azure workload identity to [v1.2.2](https://github.com/Azure/azure-workload-identity/releases/tag/v1.2.2).
  * Upgraded Azure Monitor Container Insights image to [v3.1.20](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#04222024--)
  * Windows Server 2019 has been updated to [Windows Server 17763.5696.240423](vhd-notes/AKSWindows/2022/17763.5696.240423.txt).
  * Windows Server 2022 has been updated to [Windows Server 20348.2402.240423](vhd-notes/AKSWindows/2022/20348.2402.240423.txt).
  * Azure Linux image has been updated to [Azure Linux 202404.16.0](vhd-notes/AzureLinux/202404.16.0.txt)
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202404.16.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202404.16.0.txt)

## Release 2024-04-11

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Support upgrade version skew policy between core node and control plane components from n-2 to n-3 to match [related upstream policy change starting Kubernetes version 1.28](https://kubernetes.io/blog/2023/08/15/kubernetes-v1-28-release/). AKS docs available [here](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#what-is-the-allowed-difference-in-versions-between-control-plane-and-node-pool).
* Starting 1.30 Kubernetes version and 1.27 LTS versions, beta apis will be disabled by default, when you upgrade to [them](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#before-you-begin). There will be an option provided to explicitly enable beta apis closer to the 1.30 release.
* On 15 March 2027, Windows Server 2022 will be retired when Kubernetes 1.34 reaches the end of platform support. You won't be able to create new Windows Server 2022 node pools on Kubernetes 1.35 and above. We encourage you to make the switch before 15 March 2027 to gain the richer benefits of [Windows Server 2025](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/introducing-windows-server-2025/ba-p/4026374) or Windows Server [Annual Channel](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/windows-server-annual-channel-for-containers/ba-p/3866248). These new Windows OS versions will be supported on AKS before Windows Server 2022 is retired. For more updates, see our [AKS public roadmap](https://github.com/Azure/AKS/projects/1).
* Kubernetes version 1.32 is the next [Long Term Support Version](https://learn.microsoft.com/azure/aks/long-term-support) after 1.27. Customers will get a minimum 6 months of overlap between 1.27 LTS and 1.32 LTS versions to plan upgrades.
* Kubernetes version 1.26 is now removed. Refer to  for [platform support](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) timeline.
* In 2020 Docker enacted a Rate Limiting policy for all users. In-order to assist customers with the change, Microsoft worked directly with Docker to prevent users of Microsoft Azure from being impacted. However, beginning on June 30th, 2024, Azure customers will begin to be impacted by this limit. In-order for customers to mitigate the potential effects of this limit. We recommend customers begin to use the Artifact Cache feature within Azure Container Registry or sign up for a Docker Subscription.  More information is available [here](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/best-practices-for-using-azure-container-registry-and-docker-hub/ba-p/4068979)

### Release Notes

* Features:
  * [Dual-stack networking (IPv4/IPv6) with CNI Overlay](https://learn.microsoft.com/azure/aks/azure-cni-overlay?tabs=kubectl#dual-stack-networking) is now generally available (GA) for Linux node pools.
  * Using [Alias Minor Version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#alias-minor-version) is now allowed on preview versions.
    
* Behavioral Changes:
  * This introduces the constraint template validation behavior change called out in November's release notes
[2023-11-28](https://github.com/Azure/AKS/releases/tag/2023-11-28) . It also improves cleanup of the addon, as called out in  Issue [#3541](https://github.com/Azure/AKS/issues/3541) , and patches  CVE-2024-24786 in the addon.
  * Added resource  `nodes/proxy` to  `microsoft-defender-operator` role
  * AKS will be fixing a behavior where manually added Labels, Taints and Annotations are incorrectly copied to surged upgrade nodes. To ensure any Label or Taint is present in new nodes please use the [Labels and/or Taints](https://learn.microsoft.com/azure/aks/use-labels) functionality provided by AKS. 

* Bug Fixes:
  * Fixes a bug where a PUT operation(Update) on nodepool without a specified version in LTS clusters would have an internal error.
  * Error message improved to specify that it is only allowed to update public SSH key in preview API versions.
  * Clusters running Kubernetes 1.29 or later will have `kubernetes.azure.com/managedby=aks`  label to tigera-operator deployment in Calico clusters 

* Component Updates: 
  * Upgraded Azure file CSI driver to [v1.29.4](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.4) on AKS 1.28, [v1.30.1](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.30.1) on AKS 1.29
  * Upgraded Azure Blob CSI driver to [v1.23.4](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.23.4) on AKS 1.28 and 1.29
  * Upgraded Azure Monitor Metrics April release to [v.6.8.7](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-04-08-2024)
  * Istio revision asm-1-21 is now available with Istio-based service mesh add-on. More information on performing canary upgrade for the new minor revision of Istio can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade)
  * Upgraded Windows containerd v1.7 to [v1.7.14](https://github.com/containerd/containerd/releases/tag/v1.7.14) in k8s v1.28+
  * Windows Server 2019 Image has been updated to [Windows Server 2019-17763.5696.240410](vhd-notes/AKSWindows/2019/17763.5696.240410.txt)  and  [Windows Server 2019-17763.5576.240401](vhd-notes/AKSWindows/2019/17763.5576.240401.txt)
  * Windows Server 2022 has been updated to [Windows Server 20348.2340.240410](vhd-notes/AKSWindows/2022/20348.2402.240410.txt) and [Windows Server 2022-20348.2340.240401](vhd-notes/AKSWindows/2022/20348.2340.240401.txt)
  * Azure Linux image has been updated to [Azure Linux 202404.01.0](vhd-notes/AzureLinux/202404.01.0.txt)
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202404.01.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202404.01.0.txt)
  * Upgraded Azure Monitor Container Insights image to [v3.1.19](https://github.com/microsoft/Docker-Provider/releases/tag/3.1.19)
  * Upgraded Kaito images to [v0.2.2](https://github.com/Azure/kaito/releases/tag/v0.2.2)

## Release 2024-03-31

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Support upgrade version skew policy between core node and control plane components from n-2 to n-3 to match [related upstream policy change starting Kubernetes version 1.28](https://kubernetes.io/blog/2023/08/15/kubernetes-v1-28-release/) AKS docs available [here](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#what-is-the-allowed-difference-in-versions-between-control-plane-and-node-pool).
* Starting 1.30 Kubernetes version and 1.27 LTS versions, beta apis will be disabled by default, when you upgrade to [them](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#before-you-begin). There will be an option provided to explicitly enable beta apis closer to the 1.30 release.
* On 15 March 2027, Windows Server 2022 will be retired when Kubernetes 1.34 reaches the end of platform support. You won't be able to create new Windows Server 2022 node pools on Kubernetes 1.35 and above. We encourage you to make the switch before 15 March 2027 to gain the richer benefits of [Windows Server 2025](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/introducing-windows-server-2025/ba-p/4026374) or Windows Server [Annual Channel](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/windows-server-annual-channel-for-containers/ba-p/3866248). These new Windows OS versions will be supported on AKS before Windows Server 2022 is retired. For more updates, see our [AKS public roadmap](https://github.com/Azure/AKS/projects/1).

### Release Notes
* Features:
  * [AKS Cost Analysis](https://learn.microsoft.com/azure/aks/cost-analysis) is now generally available. View the aggregated costs for all your AKS clusters and namespaces in a subscription and drill into infrastructure and namespaces costs of a cluster directly in Azure Portal.
  * [Trusted Access on AKS cluster](https://learn.microsoft.com/azure/aks/trusted-access-feature) is generally available now.
  * [Host Network NSG Control](https://learn.microsoft.com/azure/aks/use-node-public-ips#allow-host-port-connections-and-add-node-pools-to-application-security-groups) is now generally available.
  * [IP Based Load Balancer](https://learn.microsoft.com/azure/aks/load-balancer-standard#change-the-inbound-pool-type) is now generally available.
  * [Public IP Tags](https://learn.microsoft.com/azure/aks/use-node-public-ips#use-public-ip-tags-on-node-public-ips) is now generally available.

* Preview Features:
  * [Disable SSH](https://learn.microsoft.com/azure/aks/manage-ssh-node-access?tabs=node-shell#disable-ssh-overview) is in preview now. Users can disable/enable the SSH access on nodepool level. 
  * Calico can now be disabled for an AKS cluster through the update operation. More info [here](https://learn.microsoft.com/azure/aks/use-network-policies#uninstall-azure-network-policy-manager-or-calico-preview).

* Behavioral Changes:
  * Customizations to HorizontalPodAutoscaler (HPA) for `istiod` and Istio ingress gateways are now allowed. User can directly edit the HPAs in aks-istio-system and aks-istio-ingress namespaces to customize the HPA. Note that HPA changes that violate minReplicas specified in the existing PDB will be rejected/reset.

* Bug Fixes: 
  * Fixed missing CalicoBlockSize when uninstalling Calico. This fixes a bug that can cause the disablement of Calico Network Policies to fail.
  * Fixed an issue where node image upgrade or nodepool deletion might result in node auto provisioning to stop provisioning new nodes.
  * Fixed bug where the RP would sometimes normalize the case of networkProfile.loadBalancerSku from the case the user input, such as 'standard' to 'Standard', which may have caused diffs in Terraform state files or other client tools that perform diffs.

* Component Updates:
  * Upgraded coredns to [v1.9.4-hotfix.20240327](https://github.com/aks-lts/coredns/releases/tag/v1.9.4-hotfix.20240327) to fix a couple CVEs in golang.org packages.
  * Upgraded secrets-store-csi-driver to [version 1.4.2](https://github.com/kubernetes-sigs/secrets-store-csi-driver/releases/tag/v1.4.2).
  * Upgraded azure-cloud-controller-manager to version [1.29.3](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.29.3), [1.28.8](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.28.8), [1.27.16](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.27.16), [1.26.22](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.26.22).
  * Azure Linux image has been updated to [Azure Linux - 202403.25.0](vhd-notes/AzureLinux/202403.25.0.txt).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202403.25.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202403.25.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202403.25.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202403.25.0.txt).

## Release 2024-03-17

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Starting in March, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy addon](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks) will now no longer support the validation for constraint template. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/)).
* Starting with Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes is cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.
* The ContainerService's [ListOrchestratorProfiles](https://learn.microsoft.com/rest/api/aks/container-services/list-orchestrators?view=rest-aks-2019-08-01&tabs=HTTP) API has been deprecated. Please use the ManagedCluster's [ListKubernetesVersion](https://learn.microsoft.com/rest/api/aks/managed-clusters/list-kubernetes-versions?view=rest-aks-2024-01-01&viewFallbackFrom=rest-aks-2023-11-01&tabs=HTTP) API.
* Changes to kube-reserved memory reservations are now in effect in AKS 1.29. The optimized reservation logic reduces kube-reserved memory by up to 20% depending on the node configuration. For existing 1.29 node pools created prior to 2/26, please perform a node pool update or recreate to see these changes. [Learn more.](https://learn.microsoft.com/azure/aks/concepts-clusters-workloads#memory)
* On 15 March 2027, Windows Server 2022 will be retired when Kubernetes 1.34 reaches the end of platform support. You won't be able to create new Windows Server 2022 node pools on Kubernetes 1.35 and above. We encourage you to make the switch before 15 March 2027 to gain the richer benefits of [Windows Server 2025](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/introducing-windows-server-2025/ba-p/4026374) or Windows Server [Annual Channel](https://techcommunity.microsoft.com/t5/windows-server-news-and-best/windows-server-annual-channel-for-containers/ba-p/3866248). These new Windows OS versions will be supported on AKS before Windows Server 2022 is retired. For more updates, see our [AKS public roadmap](https://github.com/Azure/AKS/projects/1).

### Release notes

* Features
  * Kubernetes 1.29 is [GA](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli).
  * [5,000 Node Limit by Default](https://azure.microsoft.com/updates/generally-available-azure-kubernetes-service-aks-support-for-5k-node-limit-by-default-for-standard-tier-clusters/) is generally available in AKS. This limit is available for Standard tier and Premium tier clusters. The rollout for this feature will be separate from the 3/17 release. Please follow [this GitHub issue](https://github.com/Azure/AKS/issues/4178) for the most up to date regions where this feature has been rolled out.
  * [Gen 2 VMs](https://learn.microsoft.com/azure/aks/cluster-configuration) are now generally available for Windows on AKS. Azure [Generation 2 (Gen2) virtual machines (VMs)](https://learn.microsoft.com/azure/virtual-machines/generation-2) support key features not supported in generation 1 VMs (Gen1).
  * [Custom kubelet configuration](https://learn.microsoft.com/azure/aks/custom-node-configuration?tabs=windows-node-pools) is now generally available for Windows on AKS. To request additional kubelet parameters supported by Windows, create a feature request on [AKS Github Issues](https://github.com/Azure/AKS/issues).
  * [Outbound type migration](https://azure.microsoft.com/updates/generally-available-outbound-type-migration-in-aks/) is now generally available on AKS. You can migrate egress outbound types on existing clusters without having to recreate a cluster.

* Preview features
  * [Deployment Safeguards](https://learn.microsoft.com/azure/aks/deployment-safeguards) on AKS is now available in preview with two levels of configuration: `Warning` and `Enforcement`
  * [Windows GPU Support](https://learn.microsoft.com/azure/aks/use-windows-gpu) on AKS is now available in preview.
  * [Trusted Launch Support](https://learn.microsoft.com/azure/aks/use-trusted-launch) on AKS is now available in preview.

* Behavioral change
  * Workload Identity is now supported as a setting for static PVs on Managed Blob/File CSI drivers in 1.29.
  * Starting with the `2024-03-01` api, `OSType` will reject unknown inputs.

* Bug fixes
  * Fixed a bug where clusters with legacy hard taints on system pools could not run any operations.
  * Fixed a bug where node taints may be overwritten on certain PUT requests.
  * Fixed a bug where clusters running LTS could get a list of non-LTS versions to upgrade to.
  * Fixed a bug with Application Gateway Ingress Controller where it is unable to fetch secret objects during cluster upgrade.

* Component updates
  * Upgraded Azure CNI from 1.4.39/1.4.43 to [1.4.52](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.52) and 1.5.11 to [1.5.23](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.23).
  * Upgraded Linux Network Policy Manager from 1.4.45.3 to [1.5.23](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.23).
  * AKS clusters with Kubernetes version 1.27 and running Cilium will be upgraded to Cilium 1.13 due to Cilium 1.12 EOL.
  * Azure Linux image has been updated to [Azure Linux - 202403.19.0](vhd-notes/AzureLinux/202403.19.0.txt).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202403.19.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202403.19.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202403.19.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202403.19.0.txt).
  * Windows Server 2019 Image has been updated to [Windows Server 2019 - 17763.5576.240316](vhd-notes/AKSWindows/2019/17763.5576.240316.txt).
  * Windows Server 2022 Image has been updated to [Windows Server 2022 - 20348.2340.240316](vhd-notes/AKSWindows/2022/20348.2340.240316.txt).

## Release 2024-02-26

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Starting in March, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy addon](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks) will now no longer support the validation for constraint template. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/)).
* Starting with Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes is cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.

### Release notes

* Features
  * [Istio-based service mesh addon for AKS](https://learn.microsoft.com/azure/aks/istio-about) is now generally available.
  * AKS patch versions 1.27.9 and 1.28.5 are now available with 1.27.9 used as the default version for new clusters.

* Preview features
  * [AI toolchain operator (KAITO) add-on](https://aka.ms/aks/ai-toolchain-operator) can be enabled to automate large language model (LLM) deployment on your AKS cluster.

* Behavioral change
  * `ignoreUnfixed` is now set to `false` in [scanner options](https://eraser-dev.github.io/eraser/docs/customization#scanner-options) for [Image Cleaner](https://learn.microsoft.com/azure/aks/image-cleaner) so that images with vulnerabilities are deleted even if there is no fix/patch available for it yet.
  * Label `kubernetes.azure.com/managedby: aks` has been introduced to all managed addon components on cluster. Related issue can be found [here](https://github.com/Azure/AKS/issues/2683)
  * In Kubernetes versions >= 1.29, AMA metrics profile automatically enables basic network observability metrics at the node level. This involves installing the Retina agent on your clusters to gather and process network data. To learn more visit [Monitor AKS](https://learn.microsoft.com/en-us/azure/aks/monitor-aks#network-observability)

* Bug fixes
  * [Pod overhead](https://kubernetes.io/docs/concepts/containers/runtime-class/#pod-overhead) of memory 2Gi added to `kata-cc-isolation` [RuntimeClass](https://kubernetes.io/docs/concepts/containers/runtime-class/) to address issue where too many pods being created to use too much of the node's memory was resulting in random processes being OOM killed.
  * Fixed issue that was causing PUT operations on AKS clusters that were using [Bring your own Container Network Interface (CNI) plugin](https://learn.microsoft.com/azure/aks/use-byo-cni?tabs=azure-cli) to fail when the request didn't contain the `networkProfile.podCIDR` property.
  * In AKS clusters of version >= 1.27.0, fixed a race condition in the iptables mode of kube-proxy that could result in some updates getting lost (for example, when a service gets a new endpoint).
  * Fixed a race condition that could cause [upgrade from kubenet to Azure CNI Overlay](https://learn.microsoft.com/azure/aks/azure-cni-overlay?tabs=kubectl#kubenet-cluster-upgrade) to fail.

* Component updates
  * Istio revision asm-1-20 is now available with Istio-based service mesh add-on. More information on performing canary upgrade for the new minor revision of Istio can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade). Istio revision asm-1-18 is no longer supported.
  * Open Service Mesh upgraded to v1.2.8 with Envoy upgraded to v1.26.7 to address vulnerabilities CVE-2024-23324, CVE-2024-23325, CVE-2024-23322, CVE-2024-23323, and CVE-2024-23327.
  * For Node Auto Provisioning, Karpenter is upgraded to [v0.33.0](https://github.com/kubernetes-sigs/karpenter/releases/tag/v0.33.0) and its Azure provider is upgraded to [v0.3.0](https://github.com/Azure/karpenter-provider-azure/releases/tag/v0.3.0).
  * Upgraded Azure Disk CSI driver version to [v1.26.9](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.26.9)  on AKS 1.26, [v1.28.6](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.28.6)  on AKS 1.27, [v1.29.3](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.3)  on AKS 1.28.
  * Upgraded Azure File CSI driver version to [v1.26.11](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.11)  on AKS 1.26, [v1.28.8](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.8)  on AKS 1.27, [v1.29.3](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.3)  on AKS 1.28.
  * Upgraded Azure Blob CSI driver version to [v1.21.7](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.21.7)  on AKS 1.26, [v1.22.5](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.22.5)  on AKS 1.27, [v1.23.3](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.23.3)  on AKS 1.28.
  * Upgraded `kappie-agent` Linux and Windows images used in [AKS Network Observability](https://learn.microsoft.com/azure/aks/network-observability-overview) to v0.1.4 and v0.1.3 respectively.
  * Upgraded ACI provider for the Virtual Kubelet to [v1.6.1](https://github.com/virtual-kubelet/azure-aci/releases/tag/v1.6.1)
  * Cilium version has been updated to [1.14.4](https://github.com/cilium/cilium/releases/tag/v1.14.4) for AKS clusters with kubernetes versions >= 1.29.0.
  * Azure Linux image has been updated to [Azure Linux - 202402.12.0](vhd-notes/AzureLinux/202402.12.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202402.12.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202402.12.0.txt).
  * Windows Server 2019 Image has been updated to [Windows Server 2019 - 17763.5458.240218](vhd-notes/AKSWindows/2019/17763.5458.240218.txt).
  * Windows Server 2022 Image has been updated to [Windows Server 2022 - 20348.2322.240218](vhd-notes/AKSWindows/2022/20348.2322.240218.txt).

## Release 2024-02-07

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Starting in March, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy addon](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks) will now no longer support the validation for constraint template. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/)).
* Starting with Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.
* All current AKS API versions silently ignore unknown fields. An unknown field is a field that isn't part of the AKS API. AKS API version 2024-01-01, 2024-01-02-preview and all subsequent API versions will change this behavior. Unknown fields in a request will result in the request being rejected with an error stating that the unknown field is not understood. This change only impacts new API versions and won't impact you unless you update to use an API version 2024-01-01 or later. Existing API calls (via Azure Resource Manager templates or otherwise) will continue to function as-is.
 
### Release notes

* Features
  * [Planned Maintenance](https://learn.microsoft.com/azure/aks/planned-maintenance) and [node-image upgrade channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-os-image) are available in Azure Portal. 
  * [Associate capacity reservation groups to node pools](https://learn.microsoft.com/azure/aks/manage-node-pools#associate-capacity-reservation-groups-to-node-pools) is now generally available. 
  * Ability to set a [node soak duration](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#set-node-soak-time-value) during upgrade for node pools is now generally available.

* Preview features
  * AKS 1.29.0 is in preview. 
  * [Control Plane Metrics (API server, ETCD, Cluster Autoscaler, etc)](https://learn.microsoft.com/azure/aks/monitor-control-plane-metrics) for AKS now available in preview on [Azure Monitor managed service for Prometheus](https://learn.microsoft.com/azure/azure-monitor/essentials/prometheus-metrics-overview). 

* Bug Fixes 
  * Enable [HonorPVReclaimPolicy](https://kubernetes.io/blog/2021/12/15/kubernetes-1-23-prevent-persistentvolume-leaks-when-deleting-out-of-order/#pv-reclaim-policy-with-kubernetes-v1-23) for CSI drivers on AKS 1.27+ to align with upstream behavior. 
  * Node Auto Provision can now be enabled when aadProfiles, including ServerAppID, ClientAppID, ServerAppSecret, are being set.

* Component Updates
  * Istio-based service mesh add-on's istiod and ingress images updated to 1.18.7-hotfix.20240210 and 1.19.7 for asm-1-18 and asm-1-19 respectively. User needs to restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. Vulnerabilities CVE-2024-23322, CVE-2024-23323, CVE-2024-23324, CVE-2024-23325, and CVE-2024-23327 have been addressed in these patch versions. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade).
  * For the cloud-provider-node-manager-windows component, the following versions have been updated:
    * v1.29.0 for >=1.29.0 version
    * v1.28.5 for >=1.28.0 version
    * v1.27.13 for >=1.27.0 version
    * v1.26.19 for >=1.26.0 version
    * v1.25.24 for >=1.25.0 version
  * Upgraded konnectivity-agent image version from v0.0.33-hotfix.20221110 to to v0.1.6-hotfix.20240116. 
  * Upgraded Cilium to v1.13.10 for kubernetes v1.28.0+. 
  * Upgraded Tigera Operator to [v1.30.7](https://github.com/tigera/operator/releases/tag/v1.30.7), azurefile-csi-driver to [v1.29.3](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.3), and Microsoft Defender for Cloud Low Level Collector to v.2.0.0 starting with Kubernetes v1.29 preview. 
    * Calico [v3.26.3](https://github.com/projectcalico/calico/releases/tag/v3.26.3) is installed when using Tigera Operator v1.30.7. 
    * Microsoft Defender for Cloud Low Level Collector v.2.0.0 includes a new process collection engine, optimized and reduced CPU & Memory usage.
  * Upgraded Network Observability (Retina) to v0.1.3 with minor bug fixes. 
  * Upgraded gatekeeper to [v3.14.0](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.14.0) and policy addon [v1.3.0](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#130)
    * Azure Policy Changes
      * Introduces error state for policies in error, enabling them to be distinguished from policies in noncompliant states. 
      * Adds support for v1 constraint templates and use of the excludedNamespaces parameter in mutation policies. 
      * Adds an error status check on constraint templates post-installation.
  * Upgraded container insights agent to [v3.1.17](https://github.com/microsoft/Docker-Provider/releases/tag/3.1.17).
  * Upgraded Microsoft Defender for Cloud Security Publisher to 1.0.78 with improved logging, fixed a small bug related to cgroupv2. 
  * Azure Linux image has been updated to [Azure Linux - 202402.07.0](vhd-notes/AzureLinux/202402.07.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202402.07.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202402.07.0.txt).
  * Azure Windows 2019 Image has been updated to [Azure Windows 2019 - 17763.5329.240202](vhd-notes/AKSWindows/2019/17763.5329.240202.txt).
  * Azure Windows 2022 Image has been updated to [Azure Windows 2022 - 20348.2227.240202](vhd-notes/AKSWindows/2022/20348.2227.240202.txt).

## Release 2024-01-23

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Kubernetes 1.25 was deprecated on January 14, 2024 and support transitions to [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy). Please upgrade to Kubernetes version 1.26 or above.
* Starting with Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.
* All current AKS API versions silently ignore unknown fields. An unknown field is a field that isn't part of the AKS API. AKS API version 2024-01-01, 2024-01-02-preview and all subsequent API versions will change this behavior. Unknown fields in a request will result in the request being rejected with an error stating that the unknown field is not understood. This change only impacts new API versions and won't impact you unless you update to use an API version 2024-01-01 or later. Existing API calls (via Azure Resource Manager templates or otherwise) will continue to function as-is.
 
### Release notes

* Features
  * Ability to update an existing [Kubenet based AKS cluster](https://learn.microsoft.com/azure/aks/configure-kubenet) to use [Azure CNI Overlay](https://learn.microsoft.com/azure/aks/azure-cni-overlay) is now generally available. More information can be found [here](https://learn.microsoft.com/azure/aks/azure-cni-overlay?tabs=kubectl#upgrade-an-existing-cluster-to-cni-overlay).
  * Prometheus metrics are now exposed for [vertical pod autoscaling](https://learn.microsoft.com/azure/aks/vertical-pod-autoscaler) addon.

* Preview features
  * Istio revision 1.19 is now available with Istio-based service mesh add-on. More information on performing canary upgrade for the new minor revision of Istio can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade). Default revision of the Istio service mesh add-on for new clusters has been updated to 1.18. Istio 1.17 version is no longer supported.
  * Istio based service mesh addon now supports plugin CA to allow users to provide their own certificates and keys for signing workload certificates. More information can be found [here](https://aka.ms/asm-plugin-ca).
  * When troubleshooting AKS nodes, for developers not having access to Kubernetes API but having access to node ARM API, node IP and node name information are now made available in this API. More information on accessing the nodes using the private IPs can be found [here](https://learn.microsoft.com/azure/aks/node-access#create-an-interactive-shell-connection-to-a-node-using-the-ip-address).
  * The application routing add-on can now manage multiple public and internal NGINX ingress controllers. [Advanced ingress controller configuration](https://learn.microsoft.com/azure/aks/app-routing-nginx-configuration) is possible via a Custom Resource Definition (CRD).
  * AKS extension in VS Code has been updated to [1.4.1](https://github.com/Azure/vscode-aks-tools/releases/tag/1.4.1).

* Bug Fixes 
  * Fixed an issue that was previously preventing AKS Infiniband support for Standard_HB120-16rs_v3 SKU.
  * Fixed nodeAffinity in calico-node DaemonSet to prevent scheduling on virtual kubelet nodes.
  * Added `appgw.ingress.azure.io` api-group to ` ingress-appgw-cr` ClusterRole to address missing api-group permissions error in [Application Gateway Ingress Controller addon](https://learn.microsoft.com/azure/application-gateway/ingress-controller-overview) container.

* Behavioral Change
  * [Network observability addon](https://learn.microsoft.com/azure/aks/network-observability-overview) updated with following: 
    * increased limits for CPU (500m) and Memory (300Mi). 
    * Fixed  issue of networking observability agent crashing issue on Windows node pool of AKS clusters version >= 1.28.
    * Introduced a new `init-kappie` init container as part of `kappie-agent` DaemonSet.
    * api-resources `nodes` and `namespaces` added to `kappie-cluster-reader` ClusterRole.
  * Starting this month, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy addon](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks) will now no longer support the validation for constraint template. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/).

  
* Component Updates
  * `azure-cloud-controller-manager` updated to [v1.28.5](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.28.5), [v1.27.13](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.27.13), [v1.26.19](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.26.19), and [v1.25.24](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.25.24) for Kubernetes versions 1.28, 1.27, 1.26, and 1.25 respectively.
  * Upgraded Azure Disk CSI driver version to [v1.26.8](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.26.8)  on AKS 1.26, [v1.28.5](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.28.5)  on AKS 1.27, [v1.29.2](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.2)  on AKS 1.28.
  * Upgraded Azure File CSI driver version to [v1.26.10](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.10)  on AKS 1.26, [v1.28.7](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.7)  on AKS 1.27, [v1.29.2](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.2)  on AKS 1.28.
  * Upgraded Azure File CSI driver version on Windows node to [1.24.11](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.11)  on AKS 1.25.
  * Upgraded Azure Blob CSI driver version to [v1.21.6](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.21.6)  on AKS 1.26, [v1.22.4](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.22.4)  on AKS 1.27, [v1.23.2](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.23.2)  on AKS 1.28.
  * Upgraded [application routing addon](https://learn.microsoft.com/azure/aks/app-routing?tabs=default%2Cdeploy-app-default) version to [0.2.0](https://github.com/Azure/aks-app-routing-operator/blob/main/CHANGELOG.md#020---2023-01-11).
  * Upgraded Azure Monitor Metrics to v.6.8.3. Full changelog can be found [here](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-01-09-2024).
  * Upgraded ip-masq-agent-v2 to [v0.1.9](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.9).
  * Upgraded aad-pod-identity/nmi image from v1.8.17-1 to v1.8.18 to address golang.org/x/net CVEs - [CVE-2023-39325](https://nvd.nist.gov/vuln/detail/CVE-2023-39325), [CVE-2023-3978](https://nvd.nist.gov/vuln/detail/CVE-2023-3978), and [CVE-2023-44487](https://nvd.nist.gov/vuln/detail/CVE-2023-44487)
  * Upgraded Azure workload identity image to [v1.2.0](https://github.com/Azure/azure-workload-identity/releases/tag/v1.2.0).
  * Upgraded vertical pod autoscaler recommender image to [v0.12.0](https://github.com/kubernetes/autoscaler/releases/tag/vertical-pod-autoscaler-0.12.0).
  * Azure Linux image has been updated to [Azure Linux - 202401.17.0](vhd-notes/AzureLinux/202401.17.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202401.17.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202401.17.0.txt).
  * Azure Windows 2019 Image has been updated to [Azure Windows 2019 - 17763.5329.240110](vhd-notes/AKSWindows/2019/17763.5329.240110.txt).
  * Azure Windows 2022 Image has been updated to [Azure Windows 2022 - 20348.2227.240110](vhd-notes/AKSWindows/2022/20348.2227.240110.txt).

## Release 2024-01-08

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* [CIS Kubernetes V1.27 Benchmark](https://learn.microsoft.com/azure/aks/cis-kubernetes) is published which covers AKS 1.21.x through AKS 1.27.x.
* Kubernetes 1.25 is being deprecated on January 14, 2024 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy). Please upgrade to Kubernetes version 1.26 or above.
* Starting January 2024, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy Add-On](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks:~:text=exception%20YAML.-,Install%20Azure%20Policy%20Add%2Don%20for%20AKS,-Before%20you%20install) will now no longer support the validation for constraint template. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/).
* Starting with Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.
* Changes to reduce the kube-reserved memory reservation and eviction threshold will not be available in 1.28 as previously shared due to a release issue. These optimizations will be releasing with AKS Kubernetes minor version 1.29, which previews in January 2024. See [release calendar](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar:~:text=patched%20and%20supported.-,AKS%20Kubernetes%20release%20calendar,-View%20the%20upcoming).
 
### Release notes

* Bug Fixes 
  * PUT managedCluster operations on API versions that didn't support serviceMeshProfile resulted in "invalid mode" error response to the API requests. This issue has now been fixed.
  * A wrong MCR URL for [KEDA](https://learn.microsoft.com/azure/aks/keda-about) image in Air Gapped Cloud was previously used resulting in potential failures in enabling the KEDA addon. This issue has now been fixed.

* Behavioral Change
  *  [Artifact Streaming](https://learn.microsoft.com/azure/aks/artifact-streaming) can now be enabled on an existing node pool.
  * Starting with the `2024-01-01` and `2024-01-02-preview` APIs, we will begin to reject unknown fields in the request payloads.
  * The memory limit for [Azure Key Vault provider for Secrets Store CSI Driver](https://learn.microsoft.com/azure/aks/csi-secrets-store-driver) is now increased from 200 Mi to 300Mi.
  * Expanders flag is [removed](https://github.com/Azure/azure-rest-api-specs/pull/26440?expand=1&template=data_plane_template.md) from [AutoscalerProfile](https://learn.microsoft.com/azure/aks/hybrid/work-with-autoscaler-profiles#profile-settings) from 2023-11-01-preview API since it may cause confusion with existing Expanders.
    
* Component Updates
  * Windows Kubernetes CVE fixes for [CVE-2023-5528](https://github.com/advisories/GHSA-hq6q-c2x6-hmch).
  * Update ama-logs addon image to [3.1.16](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md).
  * Windows Server 2019 image has been updated to [17763.5206.231213](https://github.com/Azure/AKS/blob/2024-01-08/vhd-notes/AKSWindows/2019/17763.5206.231213.txt).
  * Windows Server 2022 Image has been updated to [20348.2159.231213](https://github.com/Azure/AKS/blob/2024-01-08/vhd-notes/AKSWindows/2022/20348.2159.231213.txt).
  * Azure Linux image has been updated to [Azure Linux - 202401.03.0](vhd-notes/AzureLinux/202401.03.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202401.03.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202401.03.0.txt).

## Release 2023-11-28

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Kubernetes 1.25 is being deprecated on January 14, 2024 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy). Please upgrade to Kubernetes version 1.26 or above.
* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from December 1st, 2023. We recommend updating your cluster with AKS-managed Azure AD before December 1st, 2023. This way you can manage the API server downtime during non-business hours.
* Starting January 2024, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy Add-On](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks:~:text=exception%20YAML.-,Install%20Azure%20Policy%20Add%2Don%20for%20AKS,-Before%20you%20install) will now no longer support this. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/).
* Starting Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.
* Staring with the `2024-01-01` and `2024-01-02-preview` APIs, we will begin to reject unknown fields in the request payloads.
* Changes to reduce the kube-reserved memory reservation and eviction threshold will not be available in 1.28 as previously shared due to a release issue. These optimizations will be releasing with AKS Kubernetes minor version 1.29, which previews in January 2024.
 
### Release notes

* Preview Features
  * [Artifact Streaming (Preview)](https://learn.microsoft.com/azure/aks/artifact-streaming) can now be enabled on node pools.
  * [Cluster Autoscaler profile](https://learn.microsoft.com/azure/aks/cluster-autoscaler?tabs=azure-cli#use-the-cluster-autoscaler-profile) parameters ignore-daemonsets-utilization, daemonset-eviction-for-empty-nodes, daemonset-eviction-for-occupied-nodes can now be configured 
  * [Setting node soak time value](https://learn.microsoft.com/azure/aks/upgrade-aks-cluster?tabs=azure-cli#set-node-soak-time-value-preview) - Node soak duration helps to stagger a node upgrade in a controlled manner and minimize application downtime during an upgrade

* Bug Fixes 
  * Under some conditions it was possible to upgrade to Azure CNI Overlay from Kubenet while using the Calico network policy. This scenario is now blocked.

* Behavioral Change
  * Updated AKS add-on affinity to run on system-pool when Node Auto Provisioning is enabled.
  * Resource group names, Azure Virtual Network names, and subnet names for clusters with [Azure CNI Overlay networking](https://learn.microsoft.com/azure/aks/azure-cni-overlay) or [Dynamic IP allocation](https://learn.microsoft.com/azure/aks/configure-azure-cni-dynamic-ip-allocation) can't be longer than 63 characters.


* Component Updates
  * Update Eraser version to [v1.2.3](https://github.com/eraser-dev/eraser/releases/tag/v1.2.3). This update has 3 CVE fixes:
      * [CVE-2023-39325](https://github.com/advisories/GHSA-4374-p667-p6c8)
      * [CVE-2023-3978](https://github.com/advisories/GHSA-2wrh-6pvc-2jm9)
      * [CVE-2023-5043](https://github.com/advisories/GHSA-qppj-fm5r-hxr3)
  * Azure Linux image has been updated to [Azure Linux - 202311.22.0](vhd-notes/AzureLinux/202311.22.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202312.06.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202312.06.0.txt).
  * Azure Windows 2019 Image has been updated to [Azure Windows 2019 - 17763.5122.231115](vhd-notes/AKSWindows/2019/17763.5122.231115.txt)
  * Azure Windows 2022 Image has been updated to [Azure Windows 2022 - 20348.2113.231115](vhd-notes/AKSWindows/2022/20348.2113.231115.txt)

## Release 2023-11-05

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Kubernetes 1.25 is being deprecated on January 14, 2024 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy).
* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from December 1st, 2023. We recommend updating your cluster with AKS-managed Azure AD before December 1st, 2023. This way you can manage the API server downtime during non-business hours.
* Starting January 2024, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy Add-On](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks:~:text=exception%20YAML.-,Install%20Azure%20Policy%20Add%2Don%20for%20AKS,-Before%20you%20install) will now no longer support this. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/).
* [Windows containerd v1.7](https://github.com/Azure/AKS/issues/3975) will be the default container runtime for k8s v1.28+ on AKS Windows nodes. Windows Host Process (HPC) containers is GA in Windows containerd v1.7 and it has some [breaking changes](https://github.com/kubernetes/enhancements/tree/master/keps/sig-windows/1981-windows-privileged-container-support#container-mounts).
* Starting Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.

### Release notes

* Features
  * Kubernetes 1.28 is [GA](https://azure.microsoft.com/updates/ga-kubernetes-128-support-in-azure-kubernetes-service-aks/)
  * Added kubernetes patch versions 1.25.15, 1.26.10, 1.27.7
  * KEDA addon is [GA](https://azure.microsoft.com/updates/ga-kubernetes-eventdriven-autoscaling-keda-addon-for-aks/)
* Preview Features
  * Cluster network settings can be updated to enable Kubenet -> CNI Overlay migration - available in the [CLI](https://github.com/Azure/azure-cli-extensions/pull/6936)
* Bug Fixes 
  * Incorporated fix for irqbalance [#275](https://github.com/Irqbalance/irqbalance/issues/275) a node image upgrade from 202310.19.2 will resolve the unbalanced IRQs
  * Under some conditions it was possible to set `max_surge=0` which may interfere with upgrades.  Now `max_surge` must be > 0.  See [Customize node surge upgrade](https://learn.microsoft.com/azure/aks/operator-best-practices-run-at-scale#cluster-upgrade-considerations-and-best-practices) for more information about the setting.
  * Fixed an issue where PUT operations on managedClusters or agentPools see long latency in the overall operation due to an internal network issue.
  * PATCH operations were allowed on managedClusters in a non-terminal provisioningState.  This could cause an eTag mismatch and inconsistent results or failures.  PATCH operations will now be block for managedClusters in a non-terminal provisioningState.
* Behavioral Change
  * Changes to reduce the kube-reserved memory reservation and eviction threshold will not be available in 1.28 as previously shared due to a release issue. These optimizations will be releasing with AKS Kubernetes minor version 1.29, which previews in January 2024.
* Component Updates
  * Update the aks-app-routing-operator to [version 0.0.7](https://github.com/Azure/aks-app-routing-operator/blob/main/CHANGELOG.md#007---2023-11-04) which includes notable changes in [version 0.0.6](https://github.com/Azure/aks-app-routing-operator/blob/main/CHANGELOG.md#006---2023-10-27).
    * This update has 3 CVE fixes for the nginx ingress controller.
      * [CVE-2023-5044](https://nvd.nist.gov/vuln/detail/CVE-2023-5044)
      * [CVE-2022-4886](https://nvd.nist.gov/vuln/detail/CVE-2023-4886)
      * [CVE-2023-5043](https://nvd.nist.gov/vuln/detail/CVE-2023-5043)
    * The following changes are also included:
      * The AJP protocol is no longer supported.
      * The `whitelist-source-range` annotation has been renamed to `allowlist-source-range`. Both are currently supported but it is recommended to move to the new annotation `allowlist-source-range`.
    * The `custom-http-errors` annotation now only supports errors between 400 and 599.
  * Azure Monitor Metrics [November release](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-11-03-2023) to v.6.8.1
  * Update [gatekeeper to v3.13.3](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.13.3) and policy addon 1.2.1
    * Azure Policy Changes
      * Introduce warn for policies, available in select upcoming built-in policy experiences
      * Show an exempt ComplianceReasonCode in the portal for exempt policies.
  * Update Azure Disk CSI driver version to [v1.29.1](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.29.1) on AKS 1.28, to v1.28.4 on AKS 1.27, to v1.26.7 on AKS 1.26 and 1.25
  * Update Azure File CSI driver version to [v1.29.1](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.29.1) on AKS 1.28, to v1.28.6 on AKS 1.27, to v1.26.9 on AKS 1.26 and 1.25
  * Update Azure Blob CSI driver version to [v1.23.1](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.23.1) on AKS 1.28, to v1.22.3 on AKS 1.27, to v1.21.5 on AKS 1.26 and 1.25
  * Update cloud-controller-manager image to v1.27.11, v1.26.17, v1.25.22 ([release notes](https://cloud-provider-azure.sigs.k8s.io/blog/))
  * Update to dropgz [v0.0.15](https://github.com/Azure/azure-container-networking/releases/tag/dropgz%2Fv0.0.15) to include azure-ipam v0.0.6
  * Azure Linux image has been updated to [Azure Linux - 202311.07.0](vhd-notes/AzureLinux/202311.07.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202311.07.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202311.07.0.txt).


## Release 2023-10-29

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Kubernetes 1.25 is being deprecated at the end of January 2024 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy).
* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from December 1st, 2023. We recommend updating your cluster with AKS-managed Azure AD before December 1st, 2023. This way you can manage the API server downtime during non-business hours.
* Starting January 2024, due to Gatekeeper Upstream removing validation for constraint template contents at create/update time, [the Azure Policy Add-On](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks:~:text=exception%20YAML.-,Install%20Azure%20Policy%20Add%2Don%20for%20AKS,-Before%20you%20install) will now no longer support this. The Azure Policy Add-On will report [‘InvalidConstraint/Template’ compliance reason code](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons) for detected errors after constraint template admission. This change does not impact [other compliance reason codes](https://learn.microsoft.com/azure/governance/policy/how-to/determine-non-compliance#aks-resource-provider-mode-compliance-reasons). Customers are encouraged to continue to follow best practices when updating Azure Policy for Kubernetes definitions (i.e. [Gator CLI](https://open-policy-agent.github.io/gatekeeper/website/docs/gator/).
* [Windows containerd v1.7](https://github.com/Azure/AKS/issues/3975) will be the default container runtime for k8s v1.28+ on AKS Windows nodes. Windows Host Process (HPC) containers is GA in Windows containerd v1.7 and it has some [breaking changes](https://github.com/kubernetes/enhancements/tree/master/keps/sig-windows/1981-windows-privileged-container-support#container-mounts).
* Starting Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2) for cgroupsv2.
* AKS sent out an advisory regarding [CVE-2023-29332](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2023-29332) on September 13, 2023, which impacts AKS agent nodes. Recommended mitigation is to upgrade AKS cluster and AKS node image. If impacted clusters are not upgraded, AKS will apply mitigation on customer's next cluster update operation including node OS updates and node rolling upgrades, which may cause workload disruption. 

### Release notes 
* Preview Features
  * [Windows Disable Outbound NAT (Preview)](https://learn.microsoft.com/azure/aks/nat-gateway#disable-outboundnat-for-windows-preview:~:text=identity%20%24IDENTITY_ID-,Disable%20OutboundNAT%20for%20Windows%20(preview),-Windows%20OutboundNAT%20can) now supports WS2019 and WS2022.
* Bug Fixes 
  * Corrected issue where on tainted/dedicated system pools the Vertical Pod Autoscaler (VPA) deployment could end up on non-system pools.
  * Fix for issue where a Certificate Authority bundle mismatch could produce an update on the image version of the VPA webhook.
  * Fix for possible [deadlock scenario between Container Network Service and Azure CNI](https://github.com/Azure/azure-container-networking/tree/master/docs/feature/async-delete) where pod IPs would not release on pod delete and new pods would not get an IP.
  * Fix for Windows NPM crashes in k8s 1.28 with Containerd 1.7. Bug was a result of Windows NPM DaemonSet referencing a file that did not exist in its current directory.[Containerd 1.7](https://github.com/Azure/AKS/issues/3975).
  * Fix for fleet clusters, so they will now be correctly set to NRG-Lockdown RestrictionLevel Restricted, instead of Unspecified. Additionally, fleet clusters within one of the undesired Unspecified states will be fixed on reconcile.
  * Fix to prevent conflict between Open Service Mesh and AKS Admission Enforcer.
  * Fix to improve response time and reduce long mc and agentpool operation latency.
* Behavioral Change
  * All AKS managed namespaces now have a ["kubernetes.azure.com/managedby:" AKS label](https://github.com/Azure/AKS/issues/1417). 
  * For exceptional cases, AKS now allows customer to update the requests and limits of VPA Updater and Recommender pods.
* Component Updates
  * Microsoft Defender for Cloud publisher image has been updated to 1.0.68 (now distroless)
  * Microsoft Defender for Cloud OldFileCleaner image has been updated to 1.4.68
  * Azure Linux image has been updated to [Azure Linux - 202310.26.0](vhd-notes/AzureLinux/202310.26.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202310.26.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202310.26.0.txt).


## Release 2023-10-22

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Kubernetes 1.25 is being deprecated at the end of January 2024 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy).
* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from December 1st, 2023. We recommend updating your cluster with AKS-managed Azure AD before December 1st, 2023. This way you can manage the API server downtime during non-business hours.

### Release notes 
* Bug Fixes 
  * Fix for some abnormal slow put managedClusters/agentPool operations caused by hanging connections.
  * Fix for some throttling issue by increasing secrets store AKV provider cpu limit from 50m to 100m.
  * Fix for CVE by upgrading Azure file driver version to [v1.24.11](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.11) on AKS 1.25.
  * Fix for Azure CNI Overlay when using Linux Kernel 6.2+ and K8s 1.28+. This fix prevents the CNI from setting up pod networking incorrectly.

* Behavioral Change
  * Introduced `acn-multitenancy-editor` ClusterRole to give azure-cns permissions on "multitenantpodnetworkconfigs", "podnetworkinstances", and "podnetworks" resources.

* Component Updates
  * Bumped cloud-controller-manager image to [v1.28.2](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.28.2), [v1.27.10](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.27.10), [v1.26.16](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.26.16) and [v1.25.20](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.25.20).
  * Updated Windows podsubnet and overlay CNI with signed version (v1.4.39.2) from v1.4.39.1.
  * Azure Linux image has been updated to [Azure Linux - 202310.19.0](vhd-notes/AzureLinux/202310.19.0.txt).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202310.19.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202310.19.0.txt).

## Release 2023-10-15

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.
* [CVE-2023-29332](https://github.com/Azure/AKS/issues/3904) Microsoft Azure Kubernetes Service Elevation of Privilege Vulnerability. An attacker who successfully exploited this vulnerability could gain Cluster Administrator privileges. Please update your AKS VHD to at least VHD version 230801 as mentioned in the issue

### Release notes 
* Feature 
  * AKS supports to use annotations to configure the [load balancer health probe](https://learn.microsoft.com/azure/aks/load-balancer-standard#customize-the-load-balancer-health-probe) for different service ports
* Bug Fixes 
  * Fix for preventing [cilium-operator](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium)  from restarting unmanaged coredns pods
  * Fix for - [CVE-2023-44487](https://github.com/Azure/AKS/issues/3947) - The HTTP/2 protocol allows a denial of service (server resource consumption) because request cancellation can reset many streams quickly
  * Fix for AKS Not Honoring/ Returning [PrivateEndpointConnection](https://learn.microsoft.com/azure/aks/private-clusters?tabs=azure-portal#use-a-private-endpoint-connection) description field
  * Fix for PUT on ManagedCluster allowing more than the [maximum tag limit](https://learn.microsoft.com/Azure/azure-resource-manager/management/tag-resources#limitations) of 50 in some rare cases
  * Fix for Failure to create multiple agent pools concurrently when using the same PodSubnetID- [Dynamic IP Allocation mode](https://learn.microsoft.com/azure/aks/configure-azure-cni-dynamic-ip-allocation)

* Behavioral Changes
  * Change in Key Vault error codes - KeyVaultEncryptKeyFailed will now be KeyVaultEncryptFailed and KeyVaultDecryptKeyFailed will now be KeyVaultDecryptFailed
 
* Component Updates
  * Updates ama-logs addon to version 3.1.15 [10/13/2023](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md)
  * Azure Linux image has been updated to [Azure Linux - 202310.09.0](vhd-notes/AzureLinux/202310.09.0.txt)
  * Azure Windows 2019 Image has been updated to [Azure Windows - 17763.4974.231011](vhd-notes/AKSWindows/2019/17763.4974.231011.txt)
  * Azure Windows 2022 Image has been updated to [Azure Windows - 20348.2031.231011](vhd-notes/AKSWindows/2022/20348.2031.231011.txt)
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202310.09.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202310.09.0.txt)

## Release 2023-10-08

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.

### Release notes 

* Features
  * [Stop cluster upgrades automatically on API breaking changes](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli#stop-cluster-upgrades-automatically-on-api-breaking-changes) is now generally available.
  * The AKS vscode extension has released four new features: A brand new user experience for cluster create and visual kubectl commands as well as several internal enhancements. To read more and engage with the team directly, visit the [GitHub repository](https://github.com/Azure/vscode-aks-tools/releases/tag/1.3.15)

* Bug Fixes
  * Microsoft Defender for Containers has been updated to image version 1.3.81 to support kernel versions 6.2 or higher.

* Behavioral Changes
  * With the [release](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#10042023--) of Container Insights 3.1.14, default 1-year tokens will be set to 1-hour expiry and refreshed at 10 minutes.
  * A warning has been added for clusters utilizing dual-stack networking and outbound type user-defined routing if the associated route table does not have a default IPv6 route in place. Visit [Dual-stack kubenet networking](https://learn.microsoft.com/azure/aks/configure-kubenet-dual-stack) for full details.
  * Customers can now [disable Windows GMSA](https://learn.microsoft.com/azure/aks/use-group-managed-service-accounts#disable-gmsa-on-an-existing-cluster) on an existing cluster.
  * Node OS Auto Upgrade now has a [built-in Policy Definition](https://learn.microsoft.com/azure/aks/policy-reference#microsoftcontainerservice) that can be used to validate and enforce whether it is enabled on an AKS cluster.

* Component Updates
  * Windows CNI has been updated to v1.4.39.1 for Azure CNI Overlay and Azure CNI with dynamic allocation.
  * Azure Monitor Metrics for AKS has been updated to image version 6.7.7. Please see their [release notes](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md?plain=1#L3) for full details.
  * The AKS vscode extension [v1.3.15](https://github.com/Azure/vscode-aks-tools/releases/tag/1.3.15) has been released 



## Release 2023-10-01

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.

### Release notes 

* Features
  * Support for IP address changes for [Azure Blob NFS mounts](https://learn.microsoft.com/azure/storage/blobs/network-file-system-protocol-support-how-to#step-5-install-the-aznfs-mount-helper-package) on AKS 1.27+.
  * Configurable resource group for the [Private Link Service (PLS)](https://learn.microsoft.com/azure/aks/internal-lb?tabs=set-service-annotations#create-a-private-link-service-connection) creation using the *"ServiceAnnotationPLSResourceGroup = "service.beta.kubernetes.io/azure-pls-resource-group"* annotation.
  * The [vertical pod autoscaling (VPA)](https://learn.microsoft.com/azure/aks/vertical-pod-autoscaler) add-on for AKS is now generally available. 
  * [Bring your own keys (BYOK)](https://learn.microsoft.com/azure/aks/azure-disk-customer-managed-keys#register-customer-managed-key-preview-feature) support to encrypt Azure Ephemeral disks is now generally available in AKS.

* Bug Fixes 
  * Fix for some events during an upgrade such as "Deleting node" not appearing in kubectl get events.
  * Fix for [metricDefinitions](https://learn.microsoft.com/rest/api/monitor/metric-definitions/list?tabs=HTTP) operation not exposed in Azure China.
  * Fix for [Cluster Autoscaler](https://github.com/kubernetes/autoscaler/releases/tag/cluster-autoscaler-1.28.0) condition where nodes that VPA pods are scheduled to could not be evicted.

* Behavioral Changes
  * The pod CPU request from [ama-metrics](https://learn.microsoft.com/azure/azure-monitor/essentials/prometheus-metrics-overview) daemonsets will be reduced in Windows from 500m to 150m and in Linux from 75m to 50m.
  * AKS will now validate, and block if necessary, service CIDRs placed in [public and multicast IP address ranges](https://learn.microsoft.com/azure/virtual-network/virtual-networks-faq#what-address-ranges-can-i-use-in-my-virtual-networks).
  * If the ama-logs add-on is enabled, host port 28330 will be mounted to the ama-logs daemonset in order to facilitate syslog collection.
  * To reduce vertical pod autoscaling (VPA) out of memory (OOM) errors, the vpa-recommender CPU limit will increase to 1000m, memory limit to 2000Mi, and memory request to 800Mi from 200m, 1000m, and 500Mi respectively.
  * The default [max surge](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli#customize-node-surge-upgrade) value during upgrades will be changed from 1 to 10% for AKS 1.28+ on new clusters to improve upgrade latency. 

* Component Updates
  * Linux Network Policy Manager (NPM) version has been rebuilt to [v1.4.45.2](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.45.2), containing patches for Ubuntu CVEs.
  * ip-masq-agent-v2 onboarded to semantic versioning and has been updated to [v0.1.8](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.8).
  * Upgraded Azure File CSI driver to [v1.24.10](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.10) on AKS 1.25, [v1.26.8](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.8) on AKS 1.26, and [v1.28.5](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.5) on AKS 1.27.
  * Blob CSI driver upgraded to [v1.22.2](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.22.2) on AKS 1.27+ to support AZNFS mount helper. 


## Release 2023-09-24

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.

### Release notes 
* Behavioral changes
  * If your VM SKU does not support ephemeral or PremiumSSD OS disks, AKS will now use StandardSSD as the default OS disk type as compared to Standard HDD previously.
  * [Azure Kubernetes Clusters should enable node os auto-upgrade - Microsoft Azure (Audit)](https://ms.portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F04408ca5-aa10-42ce-8536-98955cdddd4c) policy to include  the [Configure Node OS Auto upgrade on Azure Kubernetes Cluster - Microsoft Azure (DINE)](https://ms.portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F40f1aee2-4db4-4b74-acb1-c6972e24cca8) policy to allow customers to enforce that Node OS Auto Upgrade is configured on a cluster, where before they could only Audit that a cluster was configured without Node OS Auto Upgrade.

* Preview Features
  * [Image Integrity](https://learn.microsoft.com/azure/aks/image-integrity) allows you to sign container images via a process that ensures their authenticity and integrity.
    
* Bug Fixes 
  * Fix for the [Private Link Service (PLS)](https://learn.microsoft.com/azure/aks/private-clusters?tabs=azure-portal) creation failure that can occur if the customer selects a subnet name or PLS name that is too long. 

* Component Updates
  * Microsoft Defender Publisher container (part of defender for containers solution) image version has been updated to 1.0.67 from 1.0.64 which improves memory utilizaiton to reduce pod restarts due to OOMKills 
  * Cilium version has been updated to [1.13.5](https://github.com/cilium/cilium/releases/tag/v1.13.7) for AKS clusters with kubernetes versions 1.28 or greater
  * Azure File CSI driver updated to version [v1.24.9](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.9) for clusters with kubernetes version [1.25](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.7), [v1.26.7](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.7) for clusters with kubernetes version 1.26 and [v.1.28.4](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.4) for clusters with kubernetes version 1.27
  * Hotfix: There were 3 CVE's in the upstream Kubernetes related to insufficient input sanitiztion which leads to privilege escalation. AKS Patched the AKS cluster nodes for clusters version 1.24.9, 1.24.10, 1.24.15, 1.25.5, 1.25.6, 1.25.11, 1.26.0, 1.26.3, 1.26.6, 1.27.3. CVE links - [CVE-2023-3676](https://github.com/Azure/AKS/issues/3869), [CVE-2023-3955](https://github.com/Azure/AKS/issues/3870), and [CVE-2023-3893](https://github.com/Azure/AKS/issues/3871). Update your AKS cluster's node images if the cluster does not have [node OS auto-upgrade](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image#using-node-os-auto-upgrade) feature enabled.

 
## Release 2023-09-17

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.

### Release notes 
* Behavioral changes
  * After you set the [node OS auto-upgrade channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image#using-node-os-auto-upgrade) to "None", AKS doesn't automatically reimage nodes in your node pools. But when you set the node OS auto-upgrade channel to "Unmanaged", AKS will reimage all nodes in your node pools.

* Features
  * [HTTP Proxy URLs](https://learn.microsoft.com/azure/aks/http-proxy#updating-proxy-configurations) can now be updated post cluster creation.

* Component Updates
  * Azure Monitor container insights addon updated to [09/15/2023](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#09152023--) release.
  * Updated Azure Monitor metrics addon image to [09/11/2023](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-9-11-2023) release.
  * AKS Windows 2019 image has been updated to [17763.4851.230914](vhd-notes/AKSWindows/2019/17763.4851.230914.txt).
  * AKS Windows 2022 image has been updated to [20348.1970.230914](vhd-notes/AKSWindows/2022/20348.1970.230914.txt).
  * Updated Windows Azure CNI to [v1.5.6.1](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.6.1).


## Release 2023-09-10

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.

### Release notes 

* Behavioral changes
  * Update admissions enforcer to ignore "kubernetes.azure.com/managedby" and "control-plane" namespaces to fix [this issue](https://github.com/Azure/AKS/issues/1771).
  * "kubernetes.azure.com/managedby" label added to aks managed namespaces (kube-system, gatekeeper-system, tigera-system, calico-system)
  * Stopped nodepools will be upgraded during an Auto Upgrade operation.  The upgrade will apply to nodes when the nodepool is started.
  * Added priorityClassName system-node-critical property to all KEDA add-on pods to fix [this issue](https://github.com/Azure/AKS/issues/3780).
  * We will now check that your cluster has less than 400 nodes when an upgrade operation is requested and using Kubenet (400 being the node limit for Kubenet).

* Bug Fixes
  * Enable [HonorPVReclaimPolicy](https://kubernetes.io/blog/2021/12/15/kubernetes-1-23-prevent-persistentvolume-leaks-when-deleting-out-of-order/) for Azure Disk CSI driver 1.28, fixing an issue where in some Bound Persistent Volume (PV) – Persistent Volume Claim (PVC) pairs, the ordering of PV-PVC deletion determines whether the PV delete reclaim policy is honored.
 
* Component Updates
  * Updated Azure Disk CSI version to [v1.28.3](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.28.3) on K8S 1.27.
  * Updated Azure File CSI version to [v1.28.3](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.3) on K8S 1.27, [v1.26.6](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.6) on K8S 1.26, [v1.24.7](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.7) on K8S 1.25.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202309.06.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202309.06.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202309.06.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202309.06.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202309.06.0](vhd-notes/AzureLinux/202309.06.0.txt).


## Release 2023-09-03

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
* Asia East has now been changed to the 2nd release region. New release changes will reach to Asia East after US West Central, and before UK South.   Follow this via [AKS-Release-Tracker](https://releases.aks.azure.com/).
* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.
* To avoid disruptions stemming from unmanaged Canonical nightly security updates, AKS will disable unmanaged Canonical nightly updates by 2 September 2023, on clusters that haven’t specified an update option explicitly, mapping to the option `None` in the [node OS upgrade](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) channel feature. AKS strongly recommends proactively moving to [auto-upgrade node-image](https://learn.microsoft.com/azure/aks/auto-upgrade-cluster) or [node OS upgrade channel - SecurityPatch or NodeImage options](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image); you can set [maintenance windows](https://learn.microsoft.com/azure/aks/planned-maintenance) for these channels.

### Release notes
* Preview Features
  * AKS 1.28 [version](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.28.md#v1280) is now available in preview.
  * Now customers can disable OutboundNAT for Windows nodes as long as the cluster's outbound type is not Load Balancer. This [change](https://learn.microsoft.com/azure/aks/nat-gateway#disable-outboundnat-for-windows-preview) enables customers to disable OutboundNAT in conjunction with User Defined Routes (UDR) and Azure firewall. Before the modification, customers could only disable OutboundNAT for Windows nodes when the cluster's outbound type was NAT Gateway. 
* Features
  * [Node OS Upgrade Channel - NodeImage](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) is now generally available.
  * [Outbound IP](https://learn.microsoft.com/azure/aks/outbound-rules-control-egress) can now be a combination of ip/ipprefix and managed ones.
* Behavioral changes
  * The taint added by AKS [node auto repair](https://learn.microsoft.com/azure/aks/node-auto-repair) will change from `remediator.aks.microsoft.com/unschedulable` to `remediator.kubernetes.azure.com/unschedulable`.
  * After you update [SSH key](https://learn.microsoft.com/azure/aks/node-access#update-ssh-public-key-on-an-existing-aks-cluster-preview), AKS doesn't automatically [reimage](https://learn.microsoft.com/azure/aks/node-image-upgrade#upgrade-all-node-images-in-all-node-pools) your node pool, you can choose anytime to perform the reimage operation . Only after reimage is complete, does the update SSH key operation take effect.
* Component Updates
  * [Image Cleaner](https://learn.microsoft.com/azure/aks/image-cleaner) now has eraser version bumped to [v1.2.1](https://github.com/eraser-dev/eraser/releases/tag/v1.2.1).
  * Updated Windows [gmsa](https://learn.microsoft.com/azure/aks/use-group-managed-service-accounts) webhook to [v0.7.1](https://github.com/kubernetes-sigs/windows-gmsa/tree/v0.7.1) which supports multi-arch (amd64 and arm64).
  * Bumped version of Azure Workload Identity to [1.1.0](https://github.com/Azure/azure-workload-identity/releases/tag/v1.1.0).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202308.28.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202308.28.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202308.28.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202308.28.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202308.28.0](vhd-notes/AzureLinux/202308.28.0.txt).

## Release 2023-08-27

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.
* Please review the following CVEs that impact all Windows node pools in AKS clusters - [CVE-2023-3676](https://github.com/Azure/AKS/issues/3869), [CVE-2023-3955](https://github.com/Azure/AKS/issues/3870), and [CVE-2023-3893](https://github.com/Azure/AKS/issues/3871). Please update your Windows nodes to the VHD version 230809 as mentioned in these issues.
* To avoid disruptions stemming from unmanaged Canonical nightly security updates, AKS will disable unmanaged Canonical nightly updates by 2 September 2023
on clusters that haven’t specified an update option explicitly, mapping to the option `None` in the [node OS upgrade](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) channel feature. AKS strongly recommends proactively moving to [auto-upgrade node-image](https://learn.microsoft.com/azure/aks/auto-upgrade-cluster) or [node OS upgrade channel - SecurityPatch](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image); you can set [maintenance windows](https://learn.microsoft.com/azure/aks/planned-maintenance) for these channels.

### Release notes
  
* Behavioral changes
  * Previously AKS returned only 1 random node's failure even if multiple nodes had drain failures, in the error response. Now all the node drain failures are appended to the error response and returned for easier troubleshooting.

* Bug Fixes
  * Customers using Azure Monitor Managed Prometheus Service for AKS Clusters may have experienced issues with metrics add-on being disabled, missing metrics and alerts, in case both Container Insights log and Managed Prometheus are enabled on the clusters. These [hotfixes](https://github.com/Azure/prometheus-collector/blob/main/Hotfix-417488465-08272023.md) fix that issue.
  * A bug was fixed that prevented clusters using [Azure CNI Powered by Cilium](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium) from [starting after being stopped](https://learn.microsoft.com/azure/aks/start-stop-cluster?tabs=azure-cli).

* Component Updates
  * Updated Azure File CSI driver to [v1.24.5](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.5) on AKS versions >= 1.24.0 and < 1.26.
  * Bump [cloud-controller-manager image](https://cloud-provider-azure.sigs.k8s.io/blog/) v1.25.18, v1.26.14, v1.27.8 and v1.28.0.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202308.22.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202308.22.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202308.22.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202308.22.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202308.22.0](vhd-notes/AzureLinux/202308.22.0.txt).
  * AKS Windows 2019 image has been updated to [17763.4737.230809](vhd-notes/AKSWindows/2019/17763.4737.230809.txt).
  * AKS Windows 2022 image has been updated to [20348.1906.230809](vhd-notes/AKSWindows/2022/20348.1906.230809.txt).

## Release 2023-08-20

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.
* Please review the following CVEs that impact all Windows node pools in AKS clusters - [CVE-2023-3676](https://github.com/Azure/AKS/issues/3869), [CVE-2023-3955](https://github.com/Azure/AKS/issues/3870), and [CVE-2023-3893](https://github.com/Azure/AKS/issues/3871). Please update your Windows nodes to the VHD version 230809 as mentioned in these issues.
* To avoid disruptions stemming from unmanaged Canonical nightly security updates, AKS will disable unmanaged Canonical nightly updates by 2 September 2023
on clusters that haven’t specified an update option explicitly, mapping to the option `None` in the [node OS upgrade](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) channel feature. AKS strongly recommends proactively moving to [auto-upgrade node-image](https://learn.microsoft.com/azure/aks/auto-upgrade-cluster) or [node OS upgrade channel - SecurityPatch](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image); you can set [maintenance windows](https://learn.microsoft.com/azure/aks/planned-maintenance) for these channels.

### Release notes

* Features
  * Image Cleaner [https://learn.microsoft.com/azure/aks/image-cleaner] is now generally available.
  * [Planned maintenance](https://learn.microsoft.com/azure/aks/planned-maintenance) is now generally available.
  * [Azure AD workload identity with AKS](https://learn.microsoft.com/azure/aks/workload-identity-overview) has been made available in the following regions - `eastus, australiacentral, australiaeast, brazilsouth, canadacentral, centralindia, eastasia, eastus2, francecentral, germanywestcentral, japaneast, jioindiawest, koreacentral, northcentralus, northeurope, norwayeast, qatarcentral, southafricanorth, swedencentral, switzerlandnorth, uaenorth, ukwest, westus2`.
  * networkPolicy to 'none' (no network policy engine is installed) as a default value if unspecified when creating a cluster. Setting networkPolicy to 'none' is blocked for API versions prior to 2023-09-02-preview.
  
* Behavioral changes
  * `Microsoft.ContainerService/locations/{location}/kubernetesVersions` operation will now return `isDefault: true` on default version.

* Component Updates
  * Azure Monitor container insights addon updated to [08/17/2023](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#08172023--) release.
  * Updated Azure Monitor metrics addon image to [08/11/2023](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-08-11-2023) release.
  * Updated Azure Disk CSI driver to [v1.26.6](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.26.6) on AKS versions >= 1.24.0 and < 1.27. Updated Azure Disk CSI driver to [v1.28.2](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.28.2) on AKS versions >= 1.27.0.
  * Updated Azure File CSI driver to [v1.24.4](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.4) on AKS versions >= 1.24.0 and < 1.26. Updated Azure Disk CSI driver to [v1.26.4](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.4) on AKS versions >= 1.26.0.
  * Updated [Azure CNS](https://github.com/Azure/azure-container-networking) to [v1.4.44.4](https://github.com/Azure/azure-container-networking/compare/v1.4.44.3...v1.4.44.4)
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202308.16.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202308.16.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202308.16.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202308.16.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202308.16.0](vhd-notes/AzureLinux/202308.16.0.txt).

## Release 2023-08-13

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* No new clusters can be created with [Azure AD Integration (legacy)](https://learn.microsoft.com/azure/aks/azure-ad-integration-cli). Existing AKS clusters with Azure Active Directory integration will keep working. All Azure AD Integration (legacy) AKS clusters will be migrated to [AKS-managed Azure AD](https://learn.microsoft.com/azure/aks/managed-azure-ad) automatically starting from 1st Dec. 2023. We recommend updating your cluster with AKS-managed Azure AD before 1 Dec 2023. This way you can manage the API server downtime during non-business hours.

### Release notes

* Features
  * [Azure Container Networking Interface (CNI) Overlay](https://learn.microsoft.com/azure/aks/azure-cni-overlay) now fully supports Windows Server 2019 and 2022.
  
* Behavioral changes
  * Azure monitor metrics addon image is reverted from [07-28-2023 release](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-07-28-2023) back to the [06-26-2023 release](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-06-26-2023) because 07-28-2023 release contains an issue that configmap processing is broken for $ in regex fields.
  * [Automate the creation](https://learn.microsoft.com/azure/aks/internal-lb#create-a-private-link-service-connection) and connection of a [Private Link Service](https://learn.microsoft.com/azure/private-link/private-link-service-overview) to an Azure LoadBalancer, only requiring users to create Private Endpoint connections for private connectivity.

* Component Updates
  * AKS Image cleaner eraser image bumped to [v1.2.0](https://github.com/eraser-dev/eraser/releases/tag/v1.2.0).
  * Linux Network Policy Manager （NPM） version bumped to [v1.4.45.1](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.45.1) for [nftables performance improvements](https://github.com/Azure/azure-container-networking/pull/1969) and security patches.
  * ACI connector addon (virtual node) bumped to [v1.6.0](https://github.com/virtual-kubelet/azure-aci/releases/tag/v1.6.0).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202308.10.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202308.10.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202308.10.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202308.10.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202308.10.0](vhd-notes/AzureLinux/202308.10.0.txt).
  * AKS Windows 2019 image has been updated to [17763.4737.230808](vhd-notes/AKSWindows/2019/17763.4737.230808.txt).
  * AKS Windows 2022 image has been updated to [20348.1906.230808](vhd-notes/AKSWindows/2022/20348.1906.230808.txt).

## Release 2023-08-06

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* New v1.27+ AKS clusters will have KMS v2 configured by default when KMS is enabled. Customers with clusters on v1.26 and below with KMS enabled will not be able to upgrade to v1.27. To upgrade, follow the steps outlined in this [documentation](https://learn.microsoft.com/azure/aks/use-kms-etcd-encryption#migration-to-kms-v2) for migrating from KMS v1 to v2, and then proceed with upgrading the cluster to version v1.27.
* The pod security policy feature was deprecated on 1st August 2023 and removed since AKS version 1.25. We recommend you migrate to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) or [Azure Policy](https://learn.microsoft.com/azure/aks/policy-reference) to stay within Azure support.

### Release notes

* Preview Features
  * [Network Observability add-on](https://learn.microsoft.com/azure/aks/network-observability-overview) plugin is a new public preview feature that will scrape useful metrics from Kubernetes workloads and emit actionable networking observability data into industry standard Prometheus format, which can then be visualized in Grafana.
* Behavioral changes
  * New built-in [policy](https://ms.portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe1352e44-d34d-4e4d-a22e-451a15f759a1) for planned maintenance.
  * Customers will now be able to use [node public IP](https://learn.microsoft.com/azure/aks/use-node-public-ips) with [authorized IP ranges](https://learn.microsoft.com/azure/aks/api-server-authorized-ip-ranges) and [API Server VNet integration](https://learn.microsoft.com/azure/aks/api-server-vnet-integration). Previously this functionality was blocked.
  * Customers can now install [Azure Service Mesh](https://learn.microsoft.com/azure/aks/istio-deploy-addon) on AKS clusters with Cilium.
  * Configure exponential backoff in calls from the [Cilium daemonset](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium) to the Kubernetes apiserver in Azure CNI Powered by Cilium to improve recovery from OOM kills. 

 * Bug Fixes
  * Fixed a bug where the addon-token-adapter may get a staled long connection to apiserver causing network connection errors.
  * Added validation to check if pobSubnet is associated with NAT Gateway when cluster outbound type is [userAssignedNATGateway](https://learn.microsoft.com/azure/aks/nat-gateway) and pobSubnet in agentpoolProfile is not empty.
  * [Azure CNS]((https://github.com/Azure/azure-container-networking/releases)) will write the CNI conflict on the VM only after the networking goal state has been programmed for that VM. This means that Nodes will stay in a NotReady state with status "network plugin not initialized" until after DNC has created the NC and the Azure host has programmed it.

* Component Updates
  * [Windows CNS](https://github.com/Azure/azure-container-networking/releases) updated to v1.4.44.4
  * Envoy Proxy (part of OSM and Istio) has been updated to [1.26.4](https://github.com/envoyproxy/envoy/releases/tag/v1.26.4) to fix CVE-2023-35941 and CVE-2023-35944.
  * OMSAgent for Azure monitor updated to [3.1.11](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#06082023--:~:text=Release%20History-,08/03/2023,-%2D)
  * [Cluster Autoscaler](https://github.com/kubernetes/autoscaler/releases) images are releasing new versions for 1.25.x, 1.26.x, 1.27.x.
  * Azure File CSI Driver has been updated to [v1.28.1](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.28.1) on AKS 1.27.
  * Updated [wasm containerd shims](https://github.com/deislabs/containerd-wasm-shims/releases) to v0.8.0, and added wasm worker server shim.
  * Cloud provider Azure versions are bumped to [v1.25.17](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.25.17), [v1.26.13](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.26.13), [v1.27.7](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.27.7) for the corresponding patch versions with the following changes: Health probe port can be any port assigned by customer, Increase limit for TCP Idle Timeout to 100 minutes, Virtual node will always exists.
  * Azure Monitor Metrics addon image updated in [07-28-2023](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-07-28-2023) release
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202308.01.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202308.01.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202308.01.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202308.01.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202308.01.0](vhd-notes/AzureLinux/202308.01.0.txt).

## Release 2023-07-30

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Windows 2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows 2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* For AKS clusters built at version v1.27+ and enable KMS, KMS v2 is configured by default. However, for clusters with KMS enabled at versions below v1.27, upgrading to v1.27 will be blocked. To upgrade, follow the steps outlined in this [documentation](https://learn.microsoft.com/azure/aks/use-kms-etcd-encryption#migration-to-kms-v2) for migrating from KMS v1 to v2, and then proceed with upgrading the cluster to version v1.27.
* The pod security policy feature was deprecated on 1st August 2023 and removed since AKS version 1.25. We recommend you migrate to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) or [Azure Policy](https://learn.microsoft.com/azure/aks/policy-reference) to stay within Azure support.

### Release notes

* Features
  * The [AKS Vscode extension](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-aks-tools) now supports [cluster creation](https://azure.github.io/vscode-aks-tools/features/show-properties-azureportal-start-stop.html).
  * [Kubernetes version 1.27](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.27.md#changelog-since-v1270) is now Generally Available (GA).

* Behavioral changes
  * Kubernetes version 1.24 is now deprecated.
  * During [Outbound Type](https://learn.microsoft.com/azure/aks/egress-outboundtype) Migration, Public IPs are released when it doesn't meet Outbound IP goal.
  * During [Outbound Type](https://learn.microsoft.com/azure/aks/egress-outboundtype) Migration, NAT Gateway Profile is set to `1` when Outbound Type is set to something other than Managed NAT Gateway.

* Component Updates
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202307.27.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202307.27.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202307.27.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202307.27.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202307.27.0](vhd-notes/AzureLinux/202307.27.0.txt).
  * Istio-based service mesh add-on's istiod and ingress images updated to v1.17.5. User needs to restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade).
  * Updated Windows Azure CNI to [v1.5.6](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.6).
  * Updated microsoft-defender-pod-collector image to 1.0.73.

## Release 2023-07-23

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Windows 2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows 2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* Kubernetes 1.24 is being deprecated end of July 2023 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy). 
* Starting Kubernetes 1.25, the default cgroups implementation on AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ][https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2] for cgroupsv2.
* A [known issue](https://github.com/Azure/AKS/issues/3718) in Kubernetes version 1.24 is causing name resolution failures in Windows pods. Customers experiencing this issue should upgrade their cluster to Kubernetes version 1.25.
* CVE-2023-35945 has been found in Envoy Proxy (part of OSM and Istio).  We are rolling out a fix to all affected customers, please follow the [instructions](https://github.com/Azure/AKS/issues/3814) to monitor the rollout and restart your proxies.
* For AKS clusters built at version v1.27+ and enable KMS, KMS v2 is configured by default. However, for clusters with KMS enabled at versions below v1.27, upgrading to v1.27 will be blocked. To upgrade, follow the steps outlined in this [documentation](https://learn.microsoft.com/azure/aks/use-kms-etcd-encryption#migration-to-kms-v2) for migrating from KMS v1 to v2, and then proceed with upgrading the cluster to version v1.27.


### Release notes

* Features
  * New K8s patch versions
     * Removed 1.24.9, added 1.24.15.
     * Removed 1.25.5, added 1.25.11.
     * Removed 1.26.0, added 1.26.6.
     * Added 1.27.3(preview).

* Behavioral changes
  * CNI V2 maxpods increased from 16 to 60 for clusters with more than 1000 nodes and to 40 for clusters with less than 1000 nodes. 

* Bug Fixes
  * Fixed a bug that custom kubelet identity was not working on VMAS clusters.

* Component Updates
  * Cloud Provider Azure versions are bumped to [1.24.22](https://cloud-provider-azure.sigs.k8s.io/blog/2023/07/20/v1.27.6/), [1.25.16](https://cloud-provider-azure.sigs.k8s.io/blog/2023/07/21/v1.25.16/), [1.26.12](https://cloud-provider-azure.sigs.k8s.io/blog/2023/07/20/v1.26.12/), and [1.27.6](https://cloud-provider-azure.sigs.k8s.io/blog/2023/07/20/v1.27.6/)
  * CNS for AKS CNI PodSubnet and CNI Overlay bumped to [1.4.44](https://pkg.go.dev/github.com/Azure/azure-container-networking@v1.4.44/cns)
  * AKS Image cleaner eraser image bumped to [v1.1.1](https://github.com/Azure/eraser/releases/tag/v1.1.1)
  
## Release 2023-07-16

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Windows 2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows 2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* Kubernetes 1.24 is being deprecated end of July 2023 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy). 
* Starting Kubernetes 1.25, the default cgroups implementation on AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance, and read the [FAQ][https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/aks-increased-memory-usage-cgroup-v2] for cgroupsv2.
* A [known issue](https://github.com/Azure/AKS/issues/3718) in Kubernetes version 1.24 is causing name resolution failures in Windows pods. Customers experiencing this issue should upgrade their cluster to Kubernetes version 1.25.
* A CVE has been found in Envoy Proxy (part of OSM and Istio).  We are rolling out a fix to all affected customers, please follow the [instructions](https://github.com/Azure/AKS/blob/master/examples/envoy-ghsa-jfxv-29pc-x22r/README.md) to monitor the rollout and restart your proxies.

### Release notes

* Features
  * [Bring your own keys](https://learn.microsoft.com/azure/aks/azure-disk-customer-managed-keys) (BYOK) with Azure disks is GA 

* Behavioral changes
  * Remove the [deprecated label](https://learn.microsoft.com/azure/aks/use-labels#deprecated-labels) kubernetes.io/role=agent from ama-logs-windows daemonset and ama-logs-rs deployment. 
  * Allow existing AKS clusters to enable [Azure CNI Powered By Cilium](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium) by setting networkDataplane=cilium.

* Component Updates
  * Upgrade Azure File CSI driver to [v1.24.3](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.3) to fix [CVE](https://github.com/advisories/GHSA-xc8m-28vv-4pjc)
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202307.12.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202307.12.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202307.12.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202307.12.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202307.12.0](vhd-notes/AzureLinux/202307.12.0.txt).
  * AKS Windows 2019 image has been updated to [17763.4645.230712](vhd-notes/AKSWindows/2019/17763.4645.230712.txt).
  * AKS Windows 2022 image has been updated to [20348.1850.230712](vhd-notes/AKSWindows/2022/20348.1850.230712.txt).

## Release 2023-07-09

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Windows 2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows 2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* Kubernetes 1.24 is being deprecated end of July 2023 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy). 
* Starting Kubernetes 1.25, the default cgroups implementation on AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance.
* A [known issue](https://github.com/Azure/AKS/issues/3718) in Kubernetes version 1.24 is causing name resolution failures in Windows pods. Customers experiencing this issue should upgrade their cluster to Kubernetes version 1.25.

### Release notes

* Bug Fixes
  * A node restriction bug has been fixed that caused issues with Windows Server container pods while using inline volume for 1.24+ clusters.

* Component Updates
  * Update KEDA addon to [v2.10.1](https://github.com/kedacore/keda/blob/main/CHANGELOG.md#v2101) for versions less than Kubernetes version 1.27 and [KEDA v2.11](https://github.com/kedacore/keda/blob/main/CHANGELOG.md#v2110) for Kubernetes version 1.27.
  * Update Azure Monitor for Containers to [v3.1.10](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md).
  * Hotfixes for Kubernetes images v1.24.9, v1.24.10, v1.25.5, v1.25.6, v1.26.0, v1.26.3, and v1.27.1.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202307.04.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202307.04.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202307.04.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202307.04.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202307.04.0](vhd-notes/AzureLinux/202307.04.0.txt).

## Release 2023-07-02

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Windows 2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows 2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* Kubernetes 1.24 is being deprecated end of July 2023 and support will transition to our [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy). 
* Starting Kubernetes 1.25, the default cgroups implementation on AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance.
* A [known issue](https://github.com/Azure/AKS/issues/3718) in Kubernetes version 1.24 is causing name resolution failures in Windows pods. Customers experiencing this issue should upgrade their cluster to Kubernetes version 1.25.

### Release notes

* Preview Features
  * Added annotations to App Routing add-on for Prometheus automatic discovery and scraping of the [nginx ingress controller metrics](https://kubernetes.github.io/ingress-nginx/user-guide/monitoring/).
  * Support for [changing pod CIDR](https://learn.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-update) for bring your own CNI plugin.

* Behavior Changes
  * The default OS disk type for non-ephemeral OS disks is now Standard SSD.

* Bug Fixes
  * Disabled auto mounting of service account token for ip-masq-agent.
  * Fixed an issue that can incorrectly override the custom certificate authority trust on a nodepool update.

* Component Updates
  * Update Azure Monitor metrics addon image to release [06-26-2023](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-06-26-2023).
  * Update Azure Blob Storage CSI driver version to [1.22.1](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.22.1) on Kubernetes 1.27+
  * Update Azure CNS to [v1.4.44.2](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.44.2) and [v1.5.5](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.5); adding CNI v1.5.5, and adding dropgz [v0.0.9](https://github.com/Azure/azure-container-networking/releases/tag/dropgz%2Fv0.0.9).
  * Update App Routing add-on image to use [ingress-nginx 1.3.0](https://github.com/kubernetes/ingress-nginx/releases/tag/controller-v1.3.0)
  * Hotfixes for Kubernetes images v1.24.9, v1.24.10, v1.25.5, v1.25.6, v1.26.0, v1.26.3, and v1.27.1.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202306.26.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202306.26.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202306.26.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202306.26.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202306.26.0](vhd-notes/AzureLinux/202306.26.0.txt).

## Release 2023-06-25

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* [Kubernetes 1.24 is the last version of Kubernetes supported by AKS Engine](https://github.com/Azure/aks-engine#project-status). Kubernetes 1.24 goes end-of-life in July, at which point Upstream will stop releasing patches for AKS Engine and archive the project. Please consider using [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/products/kubernetes-service/#overview) for managed Kubernetes or [Cluster API Provider Azure](https://github.com/kubernetes-sigs/cluster-api-provider-azure) for self-managed Kubernetes.
* Because of [Ubuntu 22.04 FIPS](https://ubuntu.com/security/fips) certification status, we'll [switch AKS FIPS nodes from 18.04 to 20.04](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-components-breaking-changes-by-version) from 1.27 preview onwards.
* After May 31, 2023, Ubuntu 18.04 will reach end of life. AKS will continue to update the host OS from Canonical into the Kubernetes 1.24 VHD images. Customers will not receive daily security updates from Canonical past the end of May, but will be able to consume those through a node image update only. 
* Windows 2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows 2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* Kubernetes 1.24 is being deprecated end of July 2023. From Kubernetes 1.25 the default cgroups implementation on AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance.

### Release notes

* Behavior Changes
  * Added node anti affinity for Cilium and Azure Linux so that Network Observability extension does not run on these environments that are not supported.
  * Cluster create check that the size of Kubenet based clusters will not exceed 400 nodes (Kubenet on Azure limit)

* Bug Fixes
  * [Enable failure-domain.beta.kubernetes.io](https://github.com/Azure/AKS/issues/3668) labels on K8S 1.26+ nodes by default to resolve issue with in tree CSI drivers.  Will be removed from K8S 1.28

* Component Updates
  * Upgrade Secret store driver to [v1.3.4](https://github.com/kubernetes-sigs/secrets-store-csi-driver/releases/tag/v1.3.4)
  * Upgrade Azure Disk CSI driver version to [1.26.5](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.26.5) on K8S 1.24+
  * Upgrade AGIC addon to version [1.7.1](https://github.com/Azure/application-gateway-kubernetes-ingress/releases/tag/1.7.1) for K8S >= 1.27
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202306.19.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202306.19.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202306.19.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202306.19.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202306.19.0](vhd-notes/AzureLinux/202306.19.0.txt).


## Release 2023-06-18

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* [Kubernetes 1.24 is the last version of Kubernetes supported by AKS Engine](https://github.com/Azure/aks-engine#project-status). Kubernetes 1.24 goes end-of-life in July, at which point Upstream will stop releasing patches for AKS Engine and archive the project. Please consider using [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/products/kubernetes-service/#overview) for managed Kubernetes or [Cluster API Provider Azure](https://github.com/kubernetes-sigs/cluster-api-provider-azure) for self-managed Kubernetes.
* Because of Ubuntu 22.04 FIPS certification status, we'll [switch AKS FIPS nodes from 18.04 to 20.04](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-components-breaking-changes-by-version) from 1.27 preview onwards.
* After May 31, 2023, Ubuntu 18.04 will reach end of life. AKS will continue to update the host OS from Canonical into the Kubernetes 1.24 VHD images. Customers will not receive daily security updates from Canonical past the end of May, but will be able to consume those through a node image update only. 
* Windows2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* Kubernetes 1.24 is being deprecated end of July. From Kubernetes 1.25 the default cgroups implementation on AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads. Please test your applications for cgroupsv2 compliance.

### Release notes

* Preview Features
  * Existing AKS private clusters can now be converted to [API Server VNet Integration](https://learn.microsoft.com/azure/aks/api-server-vnet-integration) clusters.

* Behavior Changes
  * Added node affinity for ebpf-dataplane=cilium to Azure CNI Powered by Cilium pod.
  * Introduced `overlay-vpa-webhook-generation` and `overlay-vpa-cert-webhook-check` jobs to cleanup and generate [Vertical Pod Autoscaling](https://learn.microsoft.com/azure/aks/vertical-pod-autoscaler) secrets and webhook.
  * Change the default OS disk to Standard SSD instead of Standard HDD for VM SKUs that do not support ephemeral OS disks.
  * Starting 2023-06-02-preview API, pod CIDR is returned when network plugin is none.
  * Updated custom node configuration to change allowed value range for the following:
    * sysctls
      * netIpv4TcpkeepaliveIntvl - Previously: 10-75. New: 10-90.
      * netIpv4IpLocalPortRange - Previously: First (1024 - 60999) and Last (32768 - 65000). New: First (1024 - 60999) and Last (32768 - 65535).
      * netNetfilterNfConntrackMax - Previously: 131072 - 1048576. New: 131072 - 2097152.
      * netNetfilterNfConntrackBuckets - Previously: 65536 - 147456. New: 65536 - 524288.
    * ulimits
      * maxLockedMemory - Previously: unlimited. New: values > 0.
      * noFile - Previously: 1024. New: Values > 1024.
  * Removed unnecessary `kubernetes.io/os: linux` nodeSelector from Cilium daemonset in Azure CNI Powered By Cilium clusters.
  * `kube-proxy-replacement-healthz-bind-address` set to `0.0.0.0:10256` in `cilium-config` ConfigMap on Azure CNI Powered By Cilium clusters.
  * Default for [node os upgrade channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) updated to `NodeImage` in 2023-06-01 and 2023-06-02-preview APIs.
  * [Registration of NodeOSUpgradeChannelPreview feature flag](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image#register-the-nodeosupgradechannelpreview-feature-flag) is only required to use `SecurityPatch` Channel.

* Bug Fixes
  * Fix a bug that could cause nodepool creation to retry unnecessarily in [Azure CNI enhanced subnet support clusters](https://learn.microsoft.com/azure/aks/configure-azure-cni-dynamic-ip-allocation).
  * Increased CSI snapshot timeout to 600s to fix the azure disk cross region snapshot timeout issue.

* Component Updates
  * cloud-node-manager updated to [v1.24.21](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.24.21), [v1.25.15](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.25.15), [v1.26.11](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.26.11) and [v1.27.5](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.27.5) on respective AKS versions.
  * Updated azure-cns version to [1.5.3](https://github.com/Azure/azure-container-networking/releases/tag/v1.5.3).
  * Updated cluster-auto-scaler version to 1.26.5.
  * Updated virtual kubelet Azure ACI connector image to [1.4.16](https://github.com/virtual-kubelet/azure-aci/releases/tag/v1.4.16)
  * Updated Cilium version to [1.12.10](https://github.com/cilium/cilium/releases/tag/v1.12.10) in Azure CNI Powered by Cilium.
  * Updated Blob CSI driver to [1.21.4](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.21.4).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202306.13.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202306.13.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202306.13.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202306.13.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202306.13.0](vhd-notes/AzureLinux/202306.13.0.txt).
  * AKS Windows 2019 image has been updated to [17763.4499.230614](vhd-notes/AKSWindows/2019/17763.4499.230614.txt).
  * AKS Windows 2022 image has been updated to [20348.1787.230614](vhd-notes/AKSWindows/2022/20348.1787.230614.txt).

## Release 2023-06-11

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* With [AKS Components Breaking Changes by Version](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-components-breaking-changes-by-version), you can note version changes for components, addons and any breaking changes to be aware of before upgrading versions.
* [Kubernetes 1.24 is the last version of Kubernetes supported by AKS Engine](https://github.com/Azure/aks-engine#project-status). Kubernetes 1.24 goes end-of-life in July, at which point Upstream will stop releasing patches for AKS Engine and archive the project. Please consider using [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/products/kubernetes-service/#overview) for managed Kubernetes or [Cluster API Provider Azure](https://github.com/kubernetes-sigs/cluster-api-provider-azure) for self-managed Kubernetes.
* Because of Ubuntu 22.04 FIPS certification status, we'll [switch AKS FIPS nodes from 18.04 to 20.04](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-components-breaking-changes-by-version) from 1.27 preview onwards.
* After May 31, 2023, Ubuntu 18.04 will reach end of life. AKS will continue to update the host OS from Canonical into the Kubernetes 1.24 VHD images. Customers will not receive daily security updates from Canonical past the end of May, but will be able to consume those through a node image update only. 
* [SecurityPatch OS Servicing channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) is not supported on Azure Linux when running on NVIDIA GPU enabled VM sizes.
* Windows2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).
* The support policy for clusters in a deleted subscription has been updated. To align with Azure data retention policies, all clusters in a deleted subscription will be deleted immediately. See [support policy](https://learn.microsoft.com/azure/aks/support-policies#stopped-or-de-allocated-clusters:~:text=Stopped%2C%20deallocated%2C%20and%20Not%20Ready%20nodes) for more details.

### Release notes

* Features
  * [Migrating a cluster from Azure CNI V1 to CNI Overlay](https://learn.microsoft.com/azure/aks/azure-cni-overlay#upgrade-an-existing-cluster-to-cni-overlay) is GA now. 

* Preview Features
  * [Kubernetes version 1.27](https://kubernetes.io/blog/2023/04/11/kubernetes-v1-27-release/) is now Public Preview with AKS. [AKS will deprecate Kubernetes version 1.24 by July 30th 2023](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar). Please upgrade your AKS clusters to version 1.25 or above before 1.24 deprecation.
  * [KMS V2 is configured by default since AKS version 1.27 with the KMS feature enabled](https://learn.microsoft.com/azure/aks/use-kms-etcd-encryption#kms-v2-support). With KMS V2, you aren't limited to the 2,000 secrets support. For more information, you can refer to the [KMS V2 Improvements](https://kubernetes.io/blog/2023/05/16/kms-v2-moves-to-beta/).

* Component Updates
  * ip-masq-agent-v2 has been updated to [v0.1.7](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.7)
  * omsagent/ama-logs addon has been updated to [v3.1.9](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#06082023--)
  * OSM addon has been updated from v1.2.4 to v.1.2.5 on clusters running AKS >= v1.24.0. This patch release allows users to set the http and tcp idle timeouts via MeshConfig.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202306.07.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202306.07.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202306.07.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202306.07.0.txt).
  * Azure Linux image has been updated to [AzureLinux-202306.07.0](vhd-notes/AzureLinux/202306.07.0.txt).

## Release 2023-06-04

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Docker container runtime for Windows nodepools has been retired as of May 1, 2023. You may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd. In alignment with this retirement, AKS has deleted all published windows 2019 docker images. 
* After May 31, 2023, Ubuntu 18.04 will reach end of life. AKS will continue to update the host OS from Canonical into the Kubernetes 1.24 VHD images. Customers will not receive daily security updates from Canonical past the end of May, but will be able to consume those through a node image update only.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* Unattended Upgrades are disabled on Azure Linux when running on a NVIDIA GPU enabled VM sizes.
* [SecurityPatch OS Servicing channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) is not supported on Azure Linux when running on NVIDIA GPU enabled VM sizes.
* Windows2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).

### Release notes

* Behavior Changes
   * Automatic upgrades will now be blocked on clusters that have clients [using deprecated API versions](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli#stop-cluster-upgrades-automatically-on-api-breaking-changes-preview). This will be logged into the cluster's activity log. Upgrades will be retried during each upgrade interval and will succeed when usage of deprecated APIs has stopped. Clusters can also be [upgraded manually with the deprecated API validation bypassed](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli#bypass-validation-to-ignore-api-changes).
   * Konnectivity will now be deployed into clusters using BYOCNI or API Server VNet Integration in combination with Azure CNI Overlay.

* Component Updates
  * ip-masq-agent-v2 has been upgraded to [v0.1.6](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.6).
  * Azure Blob Storage CSI driver has been upgraded to [v1.21.3](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.21.3) for AKS 1.26+.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202306.01.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202306.01.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202306.01.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202306.01.0.txt).

## Release 2023-05-28

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Docker container runtime for Windows nodepools has been retired as of May 1, 2023. You may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd. In alignment with this retirement, AKS has deleted all published windows 2019 docker images.  
* After May 31, 2023, Ubuntu 18.04 will reach end of life. AKS will continue to update the host OS from Canonical into the Kubernetes 1.24 VHD images. Customers will not receive daily security updates from Canonical past the end of May, but will be able to consume those through a node image update only.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* Unattended Upgrades are disabled on Azure Linux when running on a NVIDIA GPU enabled VM sizes.
* [SecurityPatch OS Servicing channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) is not supported on Azure Linux when running on NVIDIA GPU enabled VM sizes.
* Windows2019 will be retired in Kubernetes v1.33 and above (ETA March 2026). Customers should [upgrade to Windows2022](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022).

### Release notes

* Features
  * Azure Linux is now generally available as a container host OS on AKS. The Build announcement can be found [here](https://build.microsoft.com/sessions/379005cd-6a55-4511-8d43-c5ceaad34e60?source=sessions) and the documentation for deploying Azure Linux can be found [here](https://learn.microsoft.com/azure/azure-linux/intro-azure-linux).
  * FIPS image support is now enabled for Azure Linux.
  * The [AKS devX extension](https://github.com/Azure/aks-devx-tools) now supports the creation of GitHub Actions.
  * [Managed Prometheus](https://learn.microsoft.com/azure/azure-monitor/essentials/prometheus-metrics-overview) is now Generally available.
  * [Kubernetes Apps](https://aka.ms/deployK8sApp) is now Generally available.

* Preview Features
  * [Generation 2 VMs](https://learn.microsoft.com/azure/aks/cluster-configuration#:~:text=Generation%202%20virtual%20machines%20on%20Windows%20(preview)) are now supported for Windows node pools. 
  * [Custom Node Configuration for kubelet parameters](https://learn.microsoft.com/azure/aks/custom-node-configuration?tabs=linux-node-pools#:~:text=Prerequisites%20for%20Windows%20kubelet%20custom%20configuration%20(preview)) is now supported for Windows node pools.
  * [Automated deployments](https://learn.microsoft.com/azure/aks/automated-deployments) now supports draft. Take your application and automatically create dockerfiles, kubernetes manifests, and github actions to deploy it onto your AKS cluster with ease.

* Behavior Changes
  * PodSecurityPolicy is removed in AKS clusters v1.25 and higher. Customers may not upgrade to v1.25 and above if PSP is enabled, an error will occur if attempted. PSP needs to be disabled before upgrading.
  * Added [installhint](https://kubernetes.io/docs/reference/config-api/kubeconfig.v1/#ExecConfig:~:text=clobber%20unknown%20fields-,ExecConfig,-Appears%20in%3A) to help guide users to install kubelogin if not already in their PATH. Users will see this hint when they get the user kubeconfig for their cluster in exec format and when a tool they use in conjunction with that kubeconfig chooses to display that hint.
  * Added configmap hash to cilium agent and operator annotations. The configmap hash will appear in the k8s manifests for [cilium-operator and cilium-agent](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium). 
  * Improved error messages and public documentation for errors [50](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/error-code-outboundconnfailvmextensionerror), [51](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/error-code-k8sapiserverconnfailvmextensionerror), and [52](https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes/error-code-k8sapiserverdnslookupfailvmextensionerror). Now when customers encounter these errors, they should be able to resolve them by accessing the appropriate section in our troubleshooting documentation.
  * Web Application Routing now supports configuration through the Azure portal.
  * During cluster upgrade to v1.26.0 or a later version, disk PV node affinity check will cause the upgrade to fail if there are disk PVs still using deprecated labels: failure-domain.beta.kubernetes.io/zone and failure-domain.beta.kubernetes.io/region
   
* Bug Fixes
  * Fixed a bug to resolve an upstream issue where the volume is not detached after the pod and PVC objects are deleted. See resolved issue [here](https://github.com/kubernetes/kubernetes/issues/114207).

* Component Updates
  * Azure File CSI driver has been upgraded to [v1.24.2](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.2).
  * Azure Linux image has been updated to [AzureLinux-202305.24.0](vhd-notes/AzureLinux/202305.24.0.txt).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202305.24.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202305.24.0.txt). 
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202305.24.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202305.24.0.txt).

## Release 2023-05-21

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Docker container runtime for Windows nodepools has been retired as of May 1, 2023. You may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd. In alignment with this retirement, AKS has deleted all published windows 2019 docker images.  
* After May 31, 2023, Ubuntu 18.04 will reach end of life. AKS will continue to update the host OS from Canonical into the Kubernetes 1.24 VHD images. Customers will not receive daily security updates from Canonical past the end of May, but will be able to consume those through a node image update only.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* Unattended Upgrades are disabled on Mariner when running on a NVIDIA GPU enabled VM sizes.
* SecurityPatch OS Servicing channel is not supported on Mariner when running on NVIDIA GPU enabled VM sizes.

### Release notes

* Behavior Changes
   * Added get permissions for ciliumnetworkpolicy, ciliumclusterwidenetworkpolicy,ciliumendpoint ciliumidentity, and ciliumnode api-resources to the aks-service ClusterRole to enable support workflows.
   * After a cluster has been stopped for 30 days, etcd backup storage is no longer deleted. Deletion of etcd backup now only happens when the cluster is deleted.
   * For arm clients that use the location header instead of the async-operation header, return bad request 400 if the async operation failed for a client error rather than 500 according to this [spec](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/async-api-reference.md).
   * Enable the toggle to use ForcePodDrain option in Stop MC operation to give some grace period for the pod to stop before deleting the node. 

* Bug Fixes
   * Fixed bug that will recreate IPv6 SLB backend pools if missing on dual-stack clusters.
   * Fixed bug to prevent customers from listing secrets in agent nodes. 
   * Fixed a bug where [disabling the Open Service Mesh add-on](https://learn.microsoft.com/azure/aks/open-service-mesh-uninstall-add-on) was leaving behind the HorizontalPodAutoscaler resources `osm-controller-hpa` and `osm-injector-hpa`

* Component Updates
   * Decrease default CPU request of Image Cleaner's vulnerability scanner from 1 core to half core which may cause client's scanning take longer time.
   * Updated `azure-cns` image to [v1.4.44_hotfix](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.44_hotfix)
   * Update container insights addon to version [3.1.8](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md). 
   * Upgrade Azure Disk CSI driver to [v1.26.4](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.26.4) to fix CVE.
   * AKS Mariner image has been updated to [AKSMariner-202305.15.0](vhd-notes/AKSMariner/202305.15.0.txt).
   * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202305.15.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202305.15.0.txt). 
   * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202305.15.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202305.15.0.txt).

## Release 2023-05-14

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Docker container runtime for Windows nodepools has been retired as of May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* AKS is gradually rolling out a change that will rotate the token in the kubeconfig credentials. It shall not incur any impact since kubeconfig has the client certificate. Should you see any issue, retrieve the kubeconfig again with `az aks get-credentials`.
* Unattended Upgrades are disabled on Mariner when running on a NVIDIA GPU enabled VM sizes.
* SecurityPatch OS Servicing channel is not supported on Mariner when running on NVIDIA GPU enabled VM sizes.

### Release notes

* Behavior Changes
   * Customers can now upgrade AKS private clusters to [apiserver vnet integrated clusters](https://learn.microsoft.com/azure/aks/api-server-vnet-integration#limitations) in all public cloud regions.
* Bug Fixes
   * Now returning a clientError "Could not find the Public IP in resource group %s in subscription %s" when creating agent pool with invalid nodePublicIPPrefixID.
   * For Node Restriction enabled clusters running window calico, we added a new role "windows-calico-node-role" to grant windows containers permission to get secret from calico-system only.
   * Now returning a clientError "Could not find any load balancer in resource group %s in subscription %s" when Stop Cluster fails with ScaleVMSSAgentPoolFailed when there is no LB on the cluster.

* Component Updates
   * Blob CSI driver upgraded to [v1.21.2](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.21.2) for AKS 1.26.
   * CSI image liveness-probe upgraded to [v2.10.0](https://github.com/kubernetes-csi/livenessprobe/releases/tag/v2.10.0) and the node-driver-registrar image upgraded to [v2.8.0](https://github.com/kubernetes-csi/node-driver-registrar/releases/tag/v2.8.0) for CVE fixes.
   * Azure File CSI driver upgraded to [v1.24.1](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.1)  for AKS 1.24, 1.25.
   * CoreDNS upgraded to [1.9.4](https://github.com/coredns/coredns/releases/tag/v1.9.4) for AKS clusters of versions >= 1.24.0.
   * AKS Windows 2019 image has been updated to [17763.4377.230510](vhd-notes/AKSWindows/2019/17763.4377.230510.txt).
   * AKS Windows 2022 image has been updated to [20348.1726.230510](vhd-notes/AKSWindows/2022/20348.1726.230510.txt).

## Release 2023-05-07

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Docker container runtime for Windows nodepools has been retired as of May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* The Docker Bridge CIDR field in the AKS API was made redundant during our change from Docker to containerD in Kubernetes version 1.19. Starting with the 2023-04-01 AKS API version, the Docker Bridge CIDR field will be removed. 
* AKS is gradually rolling out a change that will rotate the token in the kubeconfig credentials. It shall not incur any impact since kubeconfig has the client certificate. Should you see any issue, retrieve the kubeconfig again with `az aks get-credentials`.

### Release notes

* Preview Features
   * Mariner is now supported in [NodeOSUpgradeChannel (preview)](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image). This change is currently rolling out and expected to be in all regions by May 12th.

* Bug Fixes
   * Updated 'cilium', 'cilium-operator', 'cilium-pre-flight' ClusterRoles to include 'update' permission for 'ciliumidentities' api-resource. This addresses the issue where cilium-operator and cilium-agent could not garbage collect unused identities. [identities](https://github.com/cilium/cilium/commit/2adf5f4313d42ae055528b09eb8dff4c19e74a98).
   * Now returning a clientError, when you do a Stop/RunCommand action on a cluster that was never successfully provisioned and was stuck in failed state. Error message returned is "The cluster is being deleted or hasn't been fully provisioned yet.".
   * The CPU limit of Windows DaemonSet for Azure Monitor Metrics Addon is updated from 200m to 500m to fix throttling issue.
   * In cases where an Azure CNI Overlay cluster's podCIDR becomes exhausted (i.e does not have enough ip addresses for the node count across all nodepools)then based on nodepools.MaxCount value only for those nodepools that have AutoScaling enabled - customer will get an already existing error message 'i18n.InsufficientSubnetSize error Target fieldnames.NetworkProfile_PodCIDR'.
   * In case customer deploys an Azure CNI Overlay cluster into a nodeCIDR, where the nodeCIDR doesn't have enough ip addresses for the number of nodes across the nodepools on the same subnet. Then for nodepools that have autoscaling enabled and based on maxcount, customer will get the same 'i18n.InsufficientSubnetSize error message with an error target fieldnames.AgentPoolProfile_VnetSubnetID'.

* Component Updates
  * Open Service Mesh add-on images updated from v1.2.3 to [v1.2.4](https://github.com/openservicemesh/osm/releases/tag/v1.2.4) for AKS clusters of versions >= 1.24.0.
  * Istio-based service mesh add-on's istiod and ingress images updated from v1.17.1 to v1.17.2. User needs to restart the workload pods to trigger re-injection of the newer patch version of istio-proxy. More information can be found [here](https://learn.microsoft.com/azure/aks/istio-upgrade).
  * Cilium upgraded to [1.12.8](https://github.com/cilium/cilium/releases/tag/v1.12.8) for [AKS clusters with Azure CNI Powered by Cilium](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium).
  * Blob csi driver upgraded to [v1.19.5](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.19.5) on AKS 1.24, 1.25 to fix blobfuse install failures.
  * Csi-provisioner version updated to v3.5.0 in order to fix a volume deletion issue, [details](https://github.com/kubernetes/kubernetes/issues/100485#issuecomment-1497878875)
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202305.08.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202305.08.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202305.08.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202305.08.0.txt).
  * AKS Mariner image has been updated to [AKSMariner-202305.08.0](vhd-notes/AKSMariner/202305.08.0.txt).

## Release 2023-04-30

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Starting on March 21, 2023, traffic to k8s.gcr.io will be redirected to registry.k8s.io, following the [community announcement](https://kubernetes.io/blog/2023/03/10/image-registry-redirect/).
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* We are no longer offering support for Azure Disk and Azure File in-tree drivers in Kubernetes 1.26. Please [migrate to csi](https://learn.microsoft.com/azure/aks/csi-migrate-in-tree-volumes).

### Release notes

* Preview Features
  * Mariner is now supported in [NodeOSUpgradeChannel (preview)](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image). This change is currently rolling out and expected to be in all regions by May 12th.
  
* Component Updates
  * AKS Container Insights monitoring addon has been updated to [v3.1.7](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#04262023--).
  * The Prometheus addon image for linux and windows has been updated to [v6.6.0-main-04-25-2023-2eb2a81c and v6.6.0-main-04-25-2023-2eb2a81c-win](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md) respectively.
  * Metrics-server has been updated to [version v0.6.3](https://github.com/kubernetes-sigs/metrics-server/releases/tag/v0.6.3).
  * Linux NPM has been updated to version v1.4.45.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202304.24.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202304.24.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202304.24.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202304.24.0.txt).
  * AKS Mariner image has been updated to [AKSMariner-202304.24.0](vhd-notes/AKSMariner/202304.24.0.txt).

## Release 2023-04-23

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Starting on March 21, 2023, traffic to k8s.gcr.io will be redirected to registry.k8s.io, following the [community announcement](https://kubernetes.io/blog/2023/03/10/image-registry-redirect/).
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* We are no longer offering support for Azure Disk and Azure File in-tree drivers in 1.26. Please [migrate to csi](https://learn.microsoft.com/azure/aks/csi-migrate-in-tree-volumes).

### Release notes

* Behavior Changes
  * Added certificate validation for the reset service principal operation for cert rotation operations where both certs and service principal are expired.
  * Changed the maxUnavailable pod to 5% from 2% for Large Scale clusters upgrade issues when running Cilium.
  * Mariner is now rebranded to Azure Linux. Customers can deploy with Mariner or Azure Linux, as both point to the same sku.
  * The Azure Kubernetes Service RBAC Admin role definition has been updated to contain explicit references to dataActions instead of the broad "Microsoft.ContainerService/managedClusters/*" dataAction. This role is now equivalent to the permissions specified in the Kubernetes built-in admin role.

* Component Updates
  * Updated Blob CSI driver to [1.19.4](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.19.4) on AKS clusters of versions >= 1.24.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202304.20.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202304.20.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202304.20.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202304.20.0.txt).
  * AKS Mariner image has been updated to [AKSMariner-202304.20.0](vhd-notes/AKSMariner/202304.20.0.txt).

## Release 2023-04-16

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Starting on March 21, 2023, traffic to k8s.gcr.io will be redirected to registry.k8s.io, following the [community announcement](https://kubernetes.io/blog/2023/03/10/image-registry-redirect/).
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* Each Kubernetes version is supported for 12 months. After 12 months, the minor version will shift to platform support only. Our new [platform support policy](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#platform-support-policy) provides customers with Azure infrastructure support while the cluster is in an n-3 version (where n is the latest supported AKS GA minor version). Platform support does not include anything related to Kubernetes functionality and components, but provides customers with additional support beyond what was previously provided for unsupported versions.
* Microsoft has joined OpenCost community as a contributing partner, with initial contributions focused on bringing Azure resource pricing, cost allocation, and cost export to OpenCost and AKS. More details can be found [here](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/leverage-opencost-on-azure-kubernetes-service-to-understand-and/ba-p/3796813).

### Release notes

* Features
  * [Dual-stack networking (IPv4/IPv6) on kubenet](https://learn.microsoft.com/azure/aks/configure-kubenet-dual-stack?tabs=azure-cli%2Ckubectl) is now generally available.

* Preview Features
  * [Istio-based service mesh add-on](https://learn.microsoft.com/azure/aks/istio-about) for Azure Kubernetes Service is now available in preview.

* Bug Fix
  * Fixed an issue that prevented the user-assigned managed identity of the AKS cluster from being updated from identity to another user-assigned managed identity.
  * Disabled kubelet-registration-probe on Windows nodes of AKS version 1.26 to reduce [CPU consumption](https://github.com/kubernetes-csi/node-driver-registrar/issues/229).
  * For clusters using [Image Cleaner preview feature](https://learn.microsoft.com/azure/aks/image-cleaner), the unused role `eraser-leader-election-role` and rolebinding `eraser-leader-election-rolebinding` have been deleted.
  * Reduced Azure Blob CSI driver memory limit on agent node from 2100Mi to 400Mi.
  * For dual-stack networking (IPv4/IPv6) clusters, fixed an issue where the Standard Load Balancer couldn't have IPv6 public prefixes.

* Component Updates
  * Azure cloud controller manager image updated to [v1.23.30](https://cloud-provider-azure.sigs.k8s.io/blog/2023/03/13/v1.23.30/), [v1.24.17](https://cloud-provider-azure.sigs.k8s.io/blog/2023/03/13/v1.24.17/), [v1.25.11](https://cloud-provider-azure.sigs.k8s.io/blog/2023/03/13/v1.25.11/) and [v1.26.7](https://cloud-provider-azure.sigs.k8s.io/blog/2023/03/13/v1.26.7/).
  * Updated Azure Disk CSI driver to [1.26.3](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.26.3) on AKS clusters of versions >= 1.24.
  * Azure Monitor Container Insights image has been updated to [3.1.6](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#04072023--)
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202304.10.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202304.10.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202304.10.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202304.10.0.txt).
  * AKS Mariner image has been updated to [AKSMariner-202304.10.0](vhd-notes/AKSMariner/202304.10.0.txt).
  * AKS Windows 2019 image has been updated to [17763.4252.230412](vhd-notes/AKSWindows/2019/17763.4252.230412.txt).
  * AKS Windows 2022 image has been updated to [20348.1668.230412](vhd-notes/AKSWindows/2022/20348.1668.230412.txt).
  
## Release 2023-04-09

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Starting on March 21, 2023, traffic to k8s.gcr.io will be redirected to registry.k8s.io, following the [community announcement](https://kubernetes.io/blog/2023/03/10/image-registry-redirect/).
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* [Kubernetes version 1.26](https://kubernetes.io/blog/2022/12/09/kubernetes-v1-26-release/) is now Generally Available with AKS. AKS has [deprecated](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) Kubernetes version 1.23 on April 2, 2023. Please upgrade your AKS clusters to version 1.24 or above.

### Release notes

* Features
  * [AAD workload identity](https://learn.microsoft.com/azure/aks/workload-identity-overview) is now Generally Available.

* Preview Features
  * [Stop cluster minor version upgrades on API breaking changes](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli) is now available in preview. AKS will fail fast on minor version manual upgrades if it detects usages of deprecated APIs in the target version of the upgrade. This is available when target cluster for upgrade is >= 1.26.0, when the API request for cluster is using API version >= 2023-01-02-preview, and when usage of API breaking changes has been detected in the 12 hours prior to the upgrade.

* Bug Fix
  * Fixed an [issue](https://github.com/kubernetes/kubernetes/pull/115179) where `kube-scheduler` would crash on AKS clusters of version 1.25+ when there are inline volumes in the cluster.
  * Fixed an issue where it was not possible to [rotate certificates](https://learn.microsoft.com/azure/aks/certificate-rotation) for [stopped AKS clusters](https://learn.microsoft.com/azure/aks/start-stop-cluster?tabs=azure-cli).
  * When installing [Cilium Enterprise through Azure Marketplace](https://learn.microsoft.com/azure/aks/cilium-enterprise-marketplace), AKS validates that if the extension is from an Isovalent offer, then the extension name must be "cilium". The extension name error message has been clarified to reflect this requirement.

* Component Updates
  * [Azure Monitor managed service for Prometheus addon](https://learn.microsoft.com/azure/azure-monitor/essentials/prometheus-metrics-overview)'s `kube-state-metrics` image has been updated from 2.6.0 to [2.8.1](https://github.com/kubernetes/kube-state-metrics/releases/tag/v2.8.1). 
  * [Kubernetes Event-driven Autoscaling (KEDA) add-on](https://learn.microsoft.com/azure/aks/keda-deploy-add-on-cli) has been updated to version [2.10.0](https://github.com/kedacore/keda/releases/tag/v2.10.0) and is now available on AKS version 1.26.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202304.05.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202304.05.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202304.05.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202304.05.0.txt).
  * AKS Mariner image has been updated to [AKSMariner-202304.05.0](vhd-notes/AKSMariner/202304.05.0.txt).

## Release 2023-04-02

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Starting on March 21, 2023, traffic to k8s.gcr.io will be redirected to registry.k8s.io, following the [community announcement](https://kubernetes.io/blog/2023/03/10/image-registry-redirect/).
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* AKS has [deprecated](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) Kubernetes version 1.23 on April 2, 2023. Please upgrade your AKS clusters to version 1.24 or above.

### Release notes

* Feature
  * [Terminating a long running operation](https://learn.microsoft.com/azure/aks/manage-abort-operations?tabs=azure-cli) on an AKS cluster is now Generally available.
  
* Bug Fix
  * Fixed an issue that [network connectivity lost on systemd-networkd restart](https://github.com/cilium/cilium/issues/18706).

* Behavior Changes
  * [L7 proxy for Azure CNI powered by Cilium is disabled](https://learn.microsoft.com/azure/aks/azure-cni-powered-by-cilium#limitations) and not supported for GA

* Component Updates
  * Workload Identity has been updated to version [v1.0.0](https://github.com/Azure/azure-workload-identity/releases/tag/v1.0.0).
  * Azure File CSI driver has been updated to version [v1.26.1](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.26.1)on AKS 1.26. Azure File CSI driver v1.26 has CVE fixes and adds action to clean up orphaned disks in node management group. These disks were created by VMAS node and will not be used after VMs are deleted.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202303.28.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.03.28.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202303.28.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2023.03.28.txt).
  * AKS Mariner image has been updated to [AKSMariner-202303.28.0](vhd-notes/AKSMariner/2023.03.28.txt).

## Release 2023-03-26

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Starting on March 21, 2023, traffic to k8s.gcr.io will be redirected to registry.k8s.io, following the [community announcement](https://kubernetes.io/blog/2023/03/10/image-registry-redirect/).
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* AKS will [deprecate](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) Kubernetes version 1.23 on April 2, 2023. Please upgrade your AKS clusters to version 1.24 or above.
* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
  * Two in-tree driver persistent volumes won't be supported in AKS: kubernetes.io/azure-disk, kubernetes.io/azure-file.
  * All AKS clusters on version 1.26+ will use the latest coreDNS version [v1.10.1.](https://github.com/coredns/coredns/releases/tag/v1.10.1).
    * For all AKS clusters on version 1.26+, coreDNS health plugin will use lameduck 5s to minimizes DNS resolution failures during coreDNS pod restart or deployment rollout. 
    * For all AKS clusters on version 1.26+, coreDNS will use ttl 30 as default TTL for DNS records.
* Starting with Kubernetes 1.27:
  * The Max Surge default value will change on newly created nodepools from 1 node to 10% of the node pool size.

### Release notes

* Features
  * New k8s patch versions
    * Removed 1.24.6, added 1.24.10.
    * Removed 1.25.4, added 1.25.6.
* Preview Features
  * [Custom kubelet configuration for Windows](https://learn.microsoft.com/azure/aks/custom-node-configuration#prerequisites-for-windows-kubelet-custom-configuration-preview) is now in preview.
* Bug Fixes
  * Fixed a bug where clusters with multiple node pools using the same pod subnet could get stuck during deletion.
* Component Updates
  * AKS v1.26 clusters have been reverted to CoreDNS v1.9.4 to fix a regression in v1.10.1.
  * Azure CNI has been updated to version [v1.4.44](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.44).
  * Azure Monitor Agent Windows logs pod has been updated to [v3.1.5](https://github.com/microsoft/Docker-Provider/blob/3.1.5/ReleaseNotes.md).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202303.22.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202303.22.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202303.22.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202303.22.0.txt).
  * AKS Windows 2019 image has been updated to [17763.4131.230315](vhd-notes/AKSWindows/2019/17763.4131.230315.txt).
  * AKS Windows 2022 image has been updated to [20348.1607.230315](vhd-notes/AKSWindows/2022/20348.1607.230315.txt).

## Release 2023-03-19

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Starting on March 21, 2023, traffic to k8s.gcr.io will be redirected to registry.k8s.io, following the [community announcement](https://kubernetes.io/blog/2023/03/10/image-registry-redirect/).
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired, you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* AKS will [deprecate](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) Kubernetes version 1.23 on April 2, 2023. Please upgrade your AKS clusters to version 1.24 or above.
* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
  * Two in-tree driver persistent volumes won't be supported in AKS: kubernetes.io/azure-disk, kubernetes.io/azure-file.
  * All AKS clusters on version 1.26+ will use the latest coreDNS version [v1.10.1.](https://github.com/coredns/coredns/releases/tag/v1.10.1).
    * For all AKS clusters on version 1.26+, coreDNS health plugin will use lameduck 5s to minimizes DNS resolution failures during coreDNS pod restart or deployment rollout. 
    * For all AKS clusters on version 1.26+, coreDNS will use ttl 30 as default TTL for DNS records.
* Starting with Kubernetes 1.27:
  * The Max Surge default value will change on newly created nodepools from 1 to 10%.

### Release notes

* Bug Fix
  * Fixed an issue where default Linux sysctls were not applied if users specified any [Linux OS custom configuration](https://learn.microsoft.com/azure/aks/custom-node-configuration#:~:text=Linux%20OS%20custom%20configuration). If the following sysctls were not specified, the defaults may previously have changed unintentionally: net.core.somaxconn, net.ipv4.tcp_max_syn_backlog, net.ipv4.neigh.default.gc_thresh1, net.ipv4.neigh.default.gc_thresh2, and net.ipv4.neigh.default.gc_thresh3. A [node image upgrade](https://learn.microsoft.com/azure/aks/node-image-upgrade) is recommended to restore the previous behavior.
  * Fixed an issue where CAs passed during provisioning would not be added to trust store correctly. This fix is already applied and should be reflected in all new create operations. New scale operations will require a [node image upgrade](https://learn.microsoft.com/azure/aks/node-image-upgrade).
  * Fixed an issue that when client installed oss version of Image Cleaner or Workload Identity, AKS addon manager deleted their roles, service accounts, etc. which blocked its running.

* Behavior Changes
  * Default memory for Windows pods increased from 600mi to 700mi.

* Component Updates
  * Container Insights has been updated to [3.1.4](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#03012023--:~:text=the%20release%203.1.4-,03/01/2023%20%2D,-Version%20microsoft/oms).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202303.13.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202303.13.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202303.13.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202303.13.0.txt).
  * AKS Mariner image has been updated to [AKSMariner-202303.13.0](vhd-notes/AKSMariner/202303.13.0.txt).

## Release 2023-03-05

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements
 
* Windows Server 2019 will be retired with Kubernetes version 1.32 EOL on March 1, 2026. Follow the detailed steps
[in our documentation](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022)to transition to Windows Server 2022.
* Docker container runtime will be retired for Windows nodepools on May 1, 2023. After docker container runtime is retired,you may remain on existing deployed instances but scaling operations will fail, nodepool creation will fail, and you will be out of support. Follow the detailed steps [in our documentation](https://learn.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli) to upgrade to containerd.
* The Docker Bridge CIDR field in the AKS API was made redundant during our change from Docker to containerD in Kubernetes version 1.19. Starting in April 2023 with the 2023-04-01 AKS API version, the Docker Bridge CIDR field will be removed. It will continue to be supported (but ignored) in all preexisting API versions.
* The KEDA addon currently supports aks versions 1.23, 1.24 and 1.25. the managed KEDA addon will not be supported on 1.26 GA at launch. If you use the KEDA addon, please do not upgrade to 1.26. If you use auto-upgrade with the rapid channel enabled as well as the KEDA addon, please switch off the rapid channel and update manually. 
* AKS will [deprecate](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) Kubernetes version 1.23 on April 2nd 2023. Please upgrade your AKS clusters to version 1.24 or above.
* Java/JDK support for cgroups v2 is available in [JDK 11 (patch 11.0.16 and later) or JDK 15 and above](https://bugs.java.com/bugdatabase/view_bug.do?bug_id=8230305).  AKS Kubernetes 1.25+ uses cgroups v2.  Please migrate your workloads to the new JDK.
* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
  * Two in-tree driver persistent volumes won't be supported in AKS : kubernetes.io/azure-disk, kubernetes.io/azure-file.
* Starting with Kubernetes 1.27:
  * The Max Surge default value will change on newly created nodepools from 1 to 10%.
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.

### Release notes

* Preview Features
  * [Azure Backup for AKS](https://azure.microsoft.com/updates/backupforakspublicpreview/) Public Preview is now available.
  * [Azure CNI Overlay](https://learn.microsoft.com/azure/aks/azure-cni-overlay) Public Preview is now available in ALL Azure Public Cloud Regions.
* Bug Fix
  * Fixed issue with Linux node outbound connectivity failing due to HTTP_PROXY/HTTPS_PROXY not fully respected.
  * Fix to allow a stopped AKS cluster to rotate certificates.
* Behavior Changes
  * Customer applied tags on Azure cloud provider managed resources (LB, publicIP, NSG, PLS, etc) under node resource group will now be overwritten when a task to adjust the current state to match the desired state is invoked. Please follow [AKS docs](https://learn.microsoft.com/azure/aks/use-tags)
and apply the tags on the cluster if the tags are required on the AKS managed resources.
  * Increased qps limits and worker threads for CSI driver on azuredisk v2.
  * the token credential will gradually be rotated. it shall not incur any impact since kubeconfig has the client certificate. should you see any issue, call az aks get-credentials again.
  * For customers using the Web App Routing add-on (Preview), we added an "identity" field in the API response exposing the managed service identity creates by the add-on. You can grant that identity permissions to manage other Azure resources used by the add-on, such as Azure DNS and Azure Key Vault.
  * Extended cluster tag support to Standard Load Balancer and Public IP.
  * Bumped the memory limit for the Container Insights Add-on for Windows to 1Gb.
  * Customer applied tags on Azure cloud provider managed resources (LB, publicIp, NSG, PLS, etc) under node resource group would be overwritten allowing the tags to removed from the resources when requested.
* Component Updates
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-202303.06.0](vhd-notes/aks-ubuntu/AKSUbuntu-1804/202303.06.0.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-202303.06.0](vhd-notes/aks-ubuntu/AKSUbuntu-2204/202303.06.0.txt).
  * AKS Mariner image has been updated to [AKSMariner-2023.03.06](vhd-notes/AKSMariner/2023.03.06.txt).


## Release 2023-02-26

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS will [deprecate](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) Kubernetes version 1.23 on April 2nd 2023. Please upgrade your AKS clusters to version 1.24 or above.
* Java/JDK support for cgroups v2 is available in [JDK 15](https://bugs.java.com/bugdatabase/view_bug.do?bug_id=8230305) and above.  Kubernetes 1.25+ and on AKS uses cgroups.  Please migrate your workloads to the new JDK.
* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Starting with Kubernetes 1.27:
  * The Max Surge default value will change on newly created nodepools from 1 to 10%.
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.

### Release notes

* Preview Features
  * Support for [Pod Sandboxing](https://learn.microsoft.com/en-gb/azure/aks/use-pod-sandboxing) workloads
  * Enable windows metrics collection from the Azure Monitor Metrics
  * [Node OS auto-upgrade channel](https://learn.microsoft.com/azure/aks/auto-upgrade-node-image) for automatically applying OS security patches promptly
  
* Bug Fix
  * In 2023-01-01 Azure API, a hot fix is released to fix this [bug](https://github.com/Azure/AKS/issues/3481) and returns 400 error on PUT requests to "Base" or "Standard" parameters, allowing customers to still use "Basic" parameter in ManagedClusterSKUName with "Free" or "Paid" parameters in ManagedClusterSKUTier.
  * Fix Agent Pool stop issue when powerstate reporting is inconsistent
  * Fix blobfuse2 backward compatibility issue on AKS 1.25
  * Fix cluster autoscaler scheduler bug which is causing CA to crash
  * Update node label with Security Patch versions from VHD 
* Behavior Changes
  * Removed 5 minute back off when attemptng to delete a node pool with an existing operation taking place
* Component Updates
  * Azure Blob CSI driver updated to version v1.19.1
  * Update Prometheus Add-on to [02-22-2023](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-02-22-2023)
  * AKS Windows 2019 image has been updated to [17763.4010.230223](vhd-notes/AKSWindows/2019/17763.4010.230223.txt).
  * AKS Windows 2022 image has been updated to [20348.1547.230223](vhd-notes/AKSWindows/2022/20348.1547.230223.txt).

## Release 2023-02-19

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS will [deprecate](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar) Kubernetes version 1.23 on April 2nd 2023. Please upgrade your AKS clusters to version 1.24 or above.
* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Starting with Kubernetes 1.27:
  * The Max Surge default value will change on newly created nodepools from 1 to 10%.
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure Policy will be updated to [GateKeeper 3.11](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.11.0) on Feb 20th for AKS 1.24 and up.

### Release notes

* Bug Fix
  * In 2023-01-01 Azure API, a hot fix is released and currently rolling out to fix this (bug)[https://github.com/Azure/AKS/issues/3481] and returns 400 error on PUT requests to "Base" or "Standard" parameters, allowing customers to still use "Basic" parameter in ManagedClusterSKUName with "Free" or "Paid" parameters in ManagedClusterSKUTier.
* Behavior Changes
  * Clusters on upgrade-channel nodeimage or nodeos-channel will no longer pull security updates through unattended upgrade. They will now get security updates through the weekly node image upgrade.
  * Clusters with automatic [node image upgrades](https://learn.microsoft.com/azure/aks/auto-upgrade-cluster#using-cluster-auto-upgrade) (node-image auto-upgrade channel) will have nightly in-place patches turned off. You can set your own schedule (via [upgrade schedules](https://learn.microsoft.com/azure/aks/planned-maintenance#add-a-maintenance-window-configuration-with-a-json-file)).
* Component Updates
  * Azure Disk CSI driver has been upgraded to v1.26.2.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2023.02.15](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.02.15.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2023.02.15](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2023.02.15.txt).
  * AKS Windows 2019 image has been updated to [17763.4010.230216](vhd-notes/AKSWindows/2019/17763.4010.230216.txt).
  * AKS Windows 2022 image has been updated to [20348.1547.230216](vhd-notes/AKSWindows/2022/20348.1547.230216.txt).
  * AKS Mariner image has been updated to [AKSMariner-2023.02.15](vhd-notes/AKSMariner/2023.02.15.txt).

## Release 2023-02-12

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Starting with Kubernetes 1.27:
  * The Max Surge default value will change on newly created nodepools from 1 to 10%.
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure Policy will be updated to [GateKeeper 3.11](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.11.0) on Feb 20th for AKS 1.24 and up.
* Starting from the release of 2023-02-19 clusters with automatic [node image upgrades](https://learn.microsoft.com/azure/aks/auto-upgrade-cluster#using-cluster-auto-upgrade) (node-image auto-upgrade channel) will have nightly in-place patches turned off. Node image auto upgrade offers a better idempotent way to receive these fixes on a schedule (via [upgrade schedules](https://learn.microsoft.com/azure/aks/planned-maintenance#add-a-maintenance-window-configuration-with-a-json-file)). Clusters not using the node-image auto-upgrade channel remain unchanged in preparation for the release of the [OS Upgrade Channel](https://github.com/Azure/AKS/issues/2181) functionality.

### Release notes

* Preview Features
  * Kubernetes 1.26.0 is now Public Preview.
* Behavior Changes
  * Auto-upgrade Patch channel can now be set in any patch version of a supported Kubernetes minor version and it will bring the cluster to the latest supported patch.
* Component Updates
  * Azure CNI for Windows has been updated to version [1.4.41](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.41).
  * Windows Calico updated to [v3.24.0](https://github.com/projectcalico/calico/blob/release-v3.24/calico/_includes/release-notes/v3.24.0-release-notes.md) for Kubernetes v1.24+.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2023.02.09](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.02.09.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2023.02.09](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2023.02.09.txt).
  * AKS Mariner image has been updated to [AKSMariner-2023.02.09](vhd-notes/AKSMariner/2023.02.09.txt).

## Release 2023-02-05

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS introduces a new [Standard tier](https://learn.microsoft.com/azure/aks/free-standard-pricing-tiers) which includes the previous standalone uptime SLA in addition to improved capabilities over the Free tier. Read the [blog](https://aka.ms/aks/standard-tier-blog) to learn more about the launch of the Standard tier. Azure API is updated to include the new “Standard” tier, as a result, "Basic" and "Paid" will be removed in the 2023-07-01 API version, and this will be a breaking change in API version 2023-07-01 or newer. If you use automated scripts, CD pipelines, ARM templates, Terraform, or other third-party toolings that rely on the above parameters, please be sure to make the necessary changes before upgrading to the 2023-07-01 or newer API version. From API version 2023-01-01 and newer, you can start transitioning to the new API parameters "Base" and "Standard".
* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Starting with Kubernetes 1.27:
  * The Max Surge default value will change on newly created nodepools from 1 to 10%. 
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure Policy will be updated to [GateKeeper 3.11](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.11.0) on Feb 20th for AKS 1.24 and up.
* Workload Identity: Application pods using workload identity will need the following label added `azure.workload.identity/use` starting with the 2023-01-29 release. Add the label to your running pods/deployments to avoid pods from failing at restart. See more [here](https://learn.microsoft.com/azure/aks/workload-identity-overview#service-account-labels).
* The aks swagger api specs now moved under a subfolder per the [issue](https://github.com/Azure/azure-sdk/issues/5385).

### Release notes

* Bug Fix
  * [HTTP Proxy](https://learn.microsoft.com/azure/aks/http-proxy##updating-proxy-configurations) Fixed an issue on the "No Proxy" update -  where the cluster FQDN would be removed from noProxy on updates. 
* Component Updates
  * Add support for [defender agent](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-kubernetes-introduction#what-are-the-benefits-of-microsoft-defender-for-kubernetes) to run on FIPS machines.
  * Managed Prometheus addon image release. See [release notes](https://github.com/Azure/prometheus-collector/blob/main/RELEASENOTES.md#release-01-31-2023).
  * Clients (e.g. portal / CLI / powershell) can now discover the trusted access role bindings operations on available operations.
  * AKS Ubuntu 18.04 image [AKSUbuntu-1804-2023.01.26](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.01.26.txt) addresses an [issue](https://github.com/Azure/AgentBaker/pull/2714) where fips_enabled would be set to 0 while running on a fips kernel.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2023.02.01](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.02.01.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2023.02.01](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2023.02.01.txt).
  * AKS Mariner image has been updated to [AKSMariner-2023.02.01](vhd-notes/AKSMariner/2023.02.01.txt).
  
## Release 2023-01-29

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure Policy will be updated to [GateKeeper 3.11](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.11.0) on Feb 20th for AKS 1.24 and up.
* Workload Identity: Application pods using workload identity will need the following label added `azure.workload.identity/use` starting with the 2023-01-29 release. Add the label to your running pods/deployments to avoid pods from failing at restart. See more [here](https://learn.microsoft.com/azure/aks/workload-identity-overview#service-account-labels).

### Release notes

* Features
  * New k8s patch versions for 1.23: Added 1.23.15, removed 1.23.8
  * [HTTP Proxy](https://learn.microsoft.com/azure/aks/http-proxy##updating-proxy-configurations) now allows updating the "No Proxy" configuration after cluster deployment using aks update. 
* Preview Feature
  * Azure CNI Overlay now available in uksouth, australiaeast 
* Component Updates
  * Container Insights addon upgraded to [ciprod01182023](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#01182023--).
  * Azure NPM addon upgraded to [v1.4.32](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.32) in SOV Clouds.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2023.01.25](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.01.25.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2023.01.25](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2023.01.25.txt).
  * AKS Mariner image has been updated to [AKSMariner-2023.01.25](vhd-notes/AKSMariner/2023.01.25.txt).

## Release 2023-01-22

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure Policy will be updated to [GateKeeper 3.11](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.11.0) on Feb 20th for AKS 1.24 and up.
* Workload Identity: Application pods using workload identity will need the following label added `azure.workload.identity/use` starting with the 2023-01-29 release. Add the label to your running pods/deployments to avoid pods from failing at restart. See more [here](https://learn.microsoft.com/azure/aks/workload-identity-overview#service-account-labels).

### Release notes

* Features
  * New k8s patch versions for 1.24 and 1.25: Added 1.24.9, removed 1.24.3; added 1.25.5, removed 1.25.2
* Preview Feature
  * New AKS Auto Upgrade Schedule "aksmanagedAutoUPgradeSchedule" that offers better controls, flexibility like quarterly , biweekly, bimonthly etc. [Read more](https://learn.microsoft.com/azure/aks/planned-maintenance#creating-a-maintenance-window) 
* Bug Fix
  * Add multiple replicas for the OSM injector for clusters versioned lower than 1.24. Initially AKS added an HPA and removed the explicit replicas count, but the HPA was conditionally added only for clusters >= 1.24. The fix ensures that the replica count will continue to exist for lower version clusters.
* Component Updates
  * The Managed Prometheus addon now supports ARM64 nodepools.
  * Workload Identity addon upgraded to [0.15.0](https://github.com/Azure/azure-workload-identity/releases/tag/v0.15.0)
  * CSI Secret Store addon upgraded to [v1.4](https://github.com/Azure/secrets-store-csi-driver-provider-azure/releases/tag/v1.4.0)
  * Cilium AKS addon upgraded to [1.12.5](https://github.com/cilium/cilium/releases/tag/v1.12.5)
  * CSI-proxy upgraded to [v1.0.2](https://github.com/kubernetes-csi/csi-proxy/releases/tag/v1.0.2) on Windows node
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2023.01.19](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.01.19.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2023.01.19](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2023.01.19.txt).
  * AKS Mariner image has been updated to [AKSMariner-2023.01.19](vhd-notes/AKSMariner/2023.01.19.txt).
  
## Release 2023-01-15

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Starting with Kubernetes 1.26:
  * HostProcess Containers will be GA
  * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* AKS began pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.

### Release notes

* Behavior Changes
  * AKS clusters deployed in [dual-stack (IPv4/IPv6) mode](https://learn.microsoft.com/azure/aks/configure-kubenet-dual-stack) and utilizing [instance-level public IP addresses](https://learn.microsoft.com/azure/aks/use-node-public-ips) will now receive both IPv4 and IPv6 public IP addresses. This enables the instance-level public IP feature for IPv6, whereas previously IPv6 traffic would still egress the cluster via the standard outbound configuration.
  * The `nosharesock` option has been added to the default Azure Files dynamic storage class to address [this GitHub issue](https://github.com/kubernetes-sigs/azurefile-csi-driver/issues/1137).
* Component Updates
  * Azure Monitor Managed Prometheus has been updated to [release 01-11-2023](https://github.com/Azure/prometheus-collector/blob/07ba2a930b61ea6f1deae91fb16b05da19cf1f1a/RELEASENOTES.md#release-01-11-2023).
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2023.01.10](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2023.01.10.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2023.01.10](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2023.01.10.txt).
  * AKS Mariner image has been updated to [AKSMariner-2023.01.10](vhd-notes/AKSMariner/2023.01.10.txt).
  * AKS Windows 2022 image has been updated to [20348.1487.230111](vhd-notes/AKSWindows/2022/20348.1487.230111.txt).
  * AKS Windows 2019 image has been updated to [17763.3887.230111](vhd-notes/AKSWindows/2019/17763.3887.230111.txt).
  * Azure Policy will be updated to [GateKeeper 3.11](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.11.0) on Feb 20th for AKS 1.24+.
  * Containerd on Windows has been updated to [v1.6.14](https://github.com/containerd/containerd/releases/tag/v1.6.14).
  * Open Service Mesh Addon has been updated to [v1.2.3](https://github.com/openservicemesh/osm/releases/tag/v1.2.3) for clusters running AKS 1.24+.

## Release 2023-01-08

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Kubernetes 1.25 has finished rolling out in all non-sovereign regions.
* AKS begins pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.

### Release notes

* Features
  * Starting with Kubernetes 1.26:
    * HostProcess Containers will be GA
    * Some AKS labels will be deprecated. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
  * [Dynamic IP Allocation](https://learn.microsoft.com/azure/aks/configure-azure-cni#:~:text=AKS%20cluster%20creation%3A-,Dynamic%20allocation%20of%20IPs%20and%20enhanced%20subnet%20support,-A%20drawback%20with) is now available for Windows nodes.
* Preview Features
  * [IP based Load Balancer](https://learn.microsoft.com/azure/aks/load-balancer-standard#:~:text=Change%20the%20inbound%20pool%20type%20(PREVIEW)) is now available in preview
  * [Node public IP tags](https://learn.microsoft.com/azure/aks/use-node-public-ips) is now available in preview
  * [Host port NSG control](https://learn.microsoft.com/azure/aks/use-node-public-ips) is now available in preview
  * [Host ASG control](https://learn.microsoft.com/azure/aks/use-node-public-ips) is now available in preview
  * [Azure CNI Overlay for Windows](https://learn.microsoft.com/azure/aks/azure-cni-overlay) is now available in preview
* Behavior Changes
  * The OSM addon's osm-injector pod's autoscaler will no longer scale on memory, only on CPU. The osm-controller pod's HPA resource will be reconciled in EnsureExists mode to allow you to edit the resource.
* Bug Fixes
  * A bug regarding Kubernetes version 1.25 and the tigera operator has been [fixed](https://github.com/Azure/AKS/issues/3394). If your cluster is already running on v1.25.4, please create a new cluster or file a support ticket with AKS for any further help.
* Component Updates
  * [CIS Kubernetes v1.24 Benchmark](https://learn.microsoft.com/azure/aks/cis-kubernetes) has been published which covers AKS 1.21.x through AKS 1.24.x
  * KEDA add-on for AKS has been upgraded to [v2.9](https://github.com/kedacore/keda/blob/main/CHANGELOG.md#v290)
  * Virtual Kubelet has been upgraded to [v1.4.7](https://github.com/virtual-kubelet/azure-aci/releases/tag/v1.4.7) and [v1.4.8](https://github.com/virtual-kubelet/azure-aci/releases/tag/v1.4.8). See changelog for bug fixes and new features.
  * Azure disk csi driver has been updated to [v1.26.0](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.26.0)
  * Azure files csi driver has been updated to [v1.24.0](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.24.0)
  * Azure blob csi driver has been updated to [v1.19.0](https://github.com/kubernetes-sigs/blob-csi-driver/releases/tag/v1.19.0)
  * AKS Windows 2022 image has been updated to [20348.1366.221214](vhd-notes/AKSWindows/2022/20348.1366.221214.txt)
  * AKS Windows 2019 image has been updated to [17763.3770.221214](vhd-notes/AKSWindows/2019/17763.3770.221214.txt).

## Release 2022-12-04

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS is retiring `v1.22.x` on this (December 4th) release. Please [upgrade](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cl) your clusters to `v1.23` or (preferably) above.
* On this release (December 4th 2022), AKS is updating all patches on supported Kubernetes versions. This means that the oldest patch version on a supported minor version will be deprecated. Read more about AKS versioning and our policy [here](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy).
* Some AKS labels are being deprecated with the Kubernetes 1.26 release in January. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* AKS begins pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure NAT gateways do not support IPv6 and therefore cannot be used with dual-stack clusters as the cluster would not have a valid IPv6 outbound connection.
* [AKS clusters with Calico enabled](https://github.com/Azure/AKS/issues/3394) should not upgrade to Kubernetes v1.25.
* Starting Kubernetes v1.25 two in-tree driver persistent volumes types kubernetes.io/azure-disk, kubernetes.io/azure-file are deprecated and will no longer be supported. Removal of these drivers following its deprecation is not currently planned but all users should migrate as soon as possible to the corresponding persistent volume types, disk.csi.azure.com and file.csi.azure.com respectively. See how [here](https://learn.microsoft.com/azure/aks/csi-storage-drivers#migrate-custom-in-tree-storage-classes-to-csi).
* Workload Identity: Application pods using workload identity will need the following label added `azure.workload.identity/use` starting with the 2023-01-29 release. Add the label to your running pods/deployments to avoid pods from failing at restart. See more [here](https://learn.microsoft.com/azure/aks/workload-identity-overview#service-account-labels).
* Starting Jan 3, 2023 AKS will expand the policy of 0 node clusters, that are automatically stopped after 30d to include clusters with 0 "Ready" nodes (or all "Not Ready") and 0 Running VMs. Clusters with all nodes manually stopped (unsupported) and in "Not Ready" state after 30 days will be stopped accordingly. To re-start your cluster, run a [cluster start command](https://learn.microsoft.com/azure/aks/start-stop-cluster?tabs=azure-cli#start-an-aks-cluster). See the complete [Support Policy](https://learn.microsoft.com/azure/aks/support-policies) for more information.

### Release notes

* Features
  * Kubernetes 1.25 is now Generally available. 1.25.4 patch version was added
    * Ubuntu 22.04 for AMD and ARM64 architectures will be the default host.
    * Windows Server 2022 will be the default Windows host. Important, old windows 2019 containers will not work on windows server 2022 hosts.
* Preview Features
  * In Azure CNI powered by Cilium clusters, AKS now sets prometheus.io/port and prometheus.io/scrape annotations on the cilium-operator deployment as well as the prometheus container ports on the cilium and cilium operator manifests.
* Behavior Changes
  * AKS now provides a `kubernetes.azure.com/dedicated-host-group=<HOST GROUP ID>` label for nodes in an Azure Dedicated Host Group.
  * App Gateway Ingress Controller (AGIC) addon memory limit increased to 600 Mi to address to adjust for resourcing in clusters with large pod/secret counts.
  * The only allowed operation that can be performed on a stopped cluster is starting the cluster.
* Bug Fixes
  * Fixed an issue with cluster updates after a failed cluster start getting stuck.
  * AKS will have Accelerated Networking turned off in Azure Dedicated Host nodepools as Azure Dedicated Host placement currently doesn't correctly account for Accelerated Networking capable SKUs at the moment.
  * Fixed IPv6 casing mismatch between azure network provider and AKS.  
* Component Updates
  * Azure Monitor Container Insights updated to version [ciprod12032022-c9f3dc30](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#12032022--)
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2022.12.19](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.12.19.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2022.12.19](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2022.12.19.txt).
  * AKS Mariner image has been updated to [AKSMariner-2022.12.19](vhd-notes/AKSMariner/2022.12.19.txt).

## Release 2022-11-27

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS is retiring `v1.22.x` on December 4th 2022. Please [upgrade](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cl) your clusters to `v1.23` and above.
* On December 4th 2022, AKS is updating all patch's on supported Kubernetes versions. This means that the oldest patch version on a supported minor version will be deprecated. Read more about AKS versioning and our policy [here](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy).
* Some AKS labels are being deprecated with the Kubernetes 1.26 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation. 
* AKS begins pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure NAT gateways do not support IPv6 and therefore cannot be used with dual-stack clusters as the cluster would not have a valid IPv6 outbound connection.
* [AKS clusters with Calico enabled](https://github.com/Azure/AKS/issues/3315) should not upgrade to Kubernetes v1.25 preview.
* Starting Kubernetes v1.25 two in-tree driver persistent volumes won't be supported in AKS : kubernetes.io/azure-disk, kubernetes.io/azure-file. 

### Release notes

* Behavior Changes
  * Creation, Upgrade operations of PSP-enabled cluster if k8s version is v1.25 or above will no longer be allowed. 
  * Updated Calico to v3.23.3 when Kubernetes version is greater than or equal to v1.25.0.
* Bug Fixes
  * Fixed an issue in Kubernetes 1.24+ with dual-stack clusters causing apiserver to crash if the cluster has IPv6 listed first in the serviceCIDRs property.
* Component Updates
  * Update AKS Windows image versions to [17763.3650.221110](vhd-notes/AKSWindows/2019/17763.3650.221110.txt) for WS2019 and to [20348.1249.221110](vhd-notes/AKSWindows/2022/20348.1249.221110.txt) for WS2022 with the Windows security patch in Nov 2022. It contains an important bug fix for the hns crash issue.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2022.11.12](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.11.12.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2022.11.12](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2022.11.12.txt).
  * AKS Mariner image has been updated to [AKSMariner-2022.11.12](vhd-notes/AKSMariner/2022.11.12.txt).

## Release 2022-11-06

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS is retiring `v1.22.x` on December 4th 2022. Please [upgrade](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cl) your clusters to `v1.23` and above.
* On December 4th 2022, AKS is updating all patch's on supported Kubernetes versions. This means that the oldest patch version on a supported minor version will be deprecated. Read more about AKS versioning and our policy [here](https://learn.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy).
* Some AKS labels are being deprecated with the Kubernetes 1.26 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation. 
* AKS begins pod security policy deprecation on 2022-11-01 API. The [pod security policy](https://learn.microsoft.com/azure/aks/use-pod-security-policies) will be removed completely on 2023-06-01 API with AKS 1.25 version or higher. You can migrate pod security policy to [pod security admission controller](https://learn.microsoft.com/azure/aks/use-psa) before the deprecation deadline.
* Azure NAT gateways do not support IPv6 and therefore cannot be used with dual-stack clusters as the cluster would not have a valid IPv6 outbound connection.
* [AKS clusters with Calico enabled](https://github.com/Azure/AKS/issues/3315) should not upgrade to Kubernetes v1.25 preview.

### Release notes

* Features
  * [Azure Blob storage Container Storage Interface (CSI) driver](https://learn.microsoft.com/azure/aks/azure-blob-csi) is now generally available.
* Preview Features
  * [Updating SSH key on existing AKS cluster](https://learn.microsoft.com/azure/aks/node-access#update-ssh-key-on-an-existing-aks-cluster-preview) is now public preview.
* Security Disclosures
  * [CVE-2022-3162](https://github.com/Azure/AKS/issues/3326)
  * [CVE-2022-3294](https://github.com/Azure/AKS/issues/3327)
  * [CVE-2022-3602 and CVE-2022-3786](https://github.com/Azure/AKS/issues/3299)
* Behavior Changes
  * The OSM add-on now includes horizontal pod autoscaling for the osm-injector pod with a minimum of 2 replicas, maximum of 10. The resources for the injector pod has also been increased so request memory is now 128 MB and limit memory is now 500 MB.
* Bug Fixes
  * Fix issue that would cause a Cluster Stop operation to become stuck.
* Component Updates
  * Update Azure CNI to [v1.4.35](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.35)
  * Update AKS Windows image versions to [17763.3650.221110](vhd-notes/AKSWindows/2019/17763.3650.221110.txt) for WS2019 and to [20348.1249.221110](vhd-notes/AKSWindows/2022/20348.1249.221110.txt) for WS2022 with the Windows security patch in Nov 2022. It contains an important bug fix for the hns crash issue.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2022.11.01](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.11.01.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2022.11.01](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2022.11.01.txt).
  * AKS Mariner image has been updated to [AKSMariner-2022.11.01](vhd-notes/AKSMariner/2022.11.01.txt).


## Release 2022-10-30

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS is retiring `v1.22.x` on December 4th 2022. Please [upgrade](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cl) your clusters to `v1.23` and above.
* Some AKS labels are being deprecated with the Kubernetes 1.26 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation. `beta.kubernetes.io/arch=` and `beta.kubernetes.io/os=`  are still applied by kubelet in kubernetes code.
* AKS will be enforcing the de-allocated clusters [policy](https://learn.microsoft.com/azure/aks/support-policies#stopped-or-de-allocated-clusters) which specifies that manually de-allocating clusters renders the cluster out of support. Starting November 1, 2022 clusters with zero nodes will be stopped after 30 days.


### Release notes

* Preview Features
  * Kubernetes version 1.25 has been released in public preview for AKS and is rolling out to all region. We support Kubernetes 1.25.2.
    * Ubuntu 22.04 for AMD and ARM64 architectures will be the default host.
    * Windows Server 2022 will be the default Windows host. Important, old windows 2019 containers will not work on windows server 2022 hosts.
* Bug Fixes
  * Updated the AKS WS2022 images with 2022.10C. This update addresses an issue that causes Host Network Service to stop working, creating traffic interruptions. This fix will also be included in the AKS Windows2019 images with 2022.11B. Please see the release notes in https://github.com/Azure/AgentBaker/pull/2380 .
* Behavior Changes
  * AKS plans to disable "JobTrackingWithFinalizers" APISever feature for k8s version "1.23" in all regions. There is a bug in this feature. 1.23 by default turned on the feature and 1.24+ turned it off.
  * The fields Cloud, Environment, UnderlayClass, and UnderlayName will no longer be available in customers' log analytics workspaces.
  * The container runtime for Ubuntu VHDs now only depends on VHD version, not Kubernetes version. For supported Kubernetes versions < 1.24, this may imply an upgrade. The latest containerd version for all Ubuntu nodes will now be 1.6
* Component Updates
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2022.10.24](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.10.24.txt).
  * AKS Ubuntu 22.04 image has been updated to [AKSUbuntu-2204-2022.10.24](vhd-notes/aks-ubuntu/AKSUbuntu-2204/2022.10.24.txt).
  * AKS Mariner image has been updated to [AKSMariner-2022.10.24](vhd-notes/AKSMariner/2022.10.24.txt).
  * AKS Windows 2022 image has been updated to [20348.1194.221026](vhd-notes/AKSWindows/2022/20348.1194.221026.txt).


## Release 2022-10-23

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS is retiring `v1.22.x` on December 4th 2022. Please [upgrade](https://learn.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cl) your clusters to `v1.23` and above.
* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
* Some AKS labels are being deprecated with the Kubernetes 1.26 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation. `beta.kubernetes.io/arch=` and `beta.kubernetes.io/os=`  are still applied by kubelet in kubernetes code.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.
* Kubernetes 1.25 is in preview. We support Kubernetes 1.25.2.
* AKS will be enforcing the de-allocated clusters [policy](https://learn.microsoft.com/azure/aks/support-policies#stopped-or-de-allocated-clusters) which specifies that manually de-allocating clusters renders the cluster out of support. Starting November 1, 2022 clusters with zero nodes will be stopped after 30 days.
* Virtual Node is supported in [these](https://github.com/virtual-kubelet/azure-aci/blob/master/provider/aci.go) additional regions.

### Release notes

* Behavior Changes
  * The cpu limits for cloud-node-manager, csi drivers, and kube-proxy have been removed.
  * Fixed a bug to disallow cluster creation where both AAD and local accounts are disabled.  
  * Fixed bug where when a cluster is updated, it triggers a reconcile cluster operation which will remove the setting  aks-vnet -> subnet -> service endpoints  which is set by csi driver when provisioning volume using NFS protocol.

* Component Updates
  * Updated [workload identity image](https://github.com/Azure/azure-workload-identity/releases) to v0.14.0.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2022.10.17](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.10.17.txt).
  * AKS Windows 2019 image has been updated to [17763.3534.221019](vhd-notes/AKSWindows/2019/17763.3534.221019.txt). 
  * AKS Windows 2022 image has been updated to [20348.1131.221019](vhd-notes/AKSWindows/2022/20348.1131.221019.txt).

## Release 2022-10-16

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* AKS is retiring `v1.22.x` on December 4th 2022. Please upgrade your clusters to `v1.23` and above.
* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
* Some AKS labels are being deprecated with the Kubernetes 1.26 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation. `beta.kubernetes.io/arch=` and `beta.kubernetes.io/os=`  are still applied by kubelet in kubernetes code.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Features
  * [OIDC Issuer](https://learn.microsoft.com/azure/aks/cluster-configuration#oidc-issuer) is now Generally Available.
* Behavior Changes
  * The CPU limits for `cloud-node-manager` has been removed.
  * OMSAgent resources will be renamed to [`ama-logs`](https://techcommunity.microsoft.com/t5/azure-monitor-status-archive/name-update-for-agent-and-associated-resources-in-azure-monitor/ba-p/3576810).
* Component Updates
  * `ip-masq-agent-v2` has been updated to [v0.1.5](https://github.com/Azure/ip-masq-agent-v2/releases/tag/v0.1.5), which includes the usage of a distroless-iptables image and a reduction in image size from 75.4MB to 34.2MB.
  * AKS Ubuntu 18.04 image has been updated to [AKSUbuntu-1804-2022.10.12](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.10.12.txt)
  * AKS Windows 2019 image has been updated to [17763.3532.221012](vhd-notes/AKSWindows/2019/17763.3532.221012.txt) 
  * AKS Windows 2022 image has been updated to [20348.1129.221012](vhd-notes/AKSWindows/2022/20348.1129.221012.txt)

  
## Release 2022-10-09

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
* Some AKS labels are being deprecated with the Kubernetes 1.26 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation. `beta.kubernetes.io/arch=` and `beta.kubernetes.io/os=`  are still applied by kubelet in kubernetes code.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes


* Features
  * [AKS 5000 Node limit per cluster](https://learn.microsoft.com/azure/aks/operator-best-practices-run-at-scale) is now Generally Available.
  * [Confidential VM Node Pool](https://learn.microsoft.com/azure/aks/use-cvm) on AKS is now Generally Available.
  * [Event Grid](https://learn.microsoft.com/Azure/event-grid/event-schema-aks?tabs=event-grid-event-schema) integration with AKS is now Generally Available.
* Preview Features
  * [Azure AD Workload Identity](https://learn.microsoft.com/azure/aks/workload-identity-overview), integrated with the Kubernetes native capabilities to federated with Azure AD, is now in Public Preview.
  * [Azure Kubernetes Fleet Manager](https://learn.microsoft.com/azure/kubernetes-fleet/overview) is now in Public Preview.
  * [Kubernetes Apps on Azure MarketPlace](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/releasing-kubernetes-apps-offer-in-microsoft-cloud-marketplace/ba-p/3650628) is now in Public Preview.
* Fixes
  * Hotfixes on v1.24.3 and v1.24.6 for [Windows BUG] (https://github.com/Azure/AKS/issues/3246) has been published to all regions. You can check the k8s package version in C:\AzureData\CustomDataSetupScript.log. If neither v1.24.3-hotfix.20221006-1int.zip nor v1.24.6-hotfix.20221006-1int.zip are used, you need to upgrade your clusters or create new Windows agent pools to get the fix.
  * Fixed a bug where an AKS FIPS node may become a non-FIPS node after unattended upgrade and reboot.
  * Hotfixed a bug where we double counted windows vms in subnet size validation.
* Behavior Changes
  * Added `pid.available<2000` to kubelet flag --eviction-hard, making the effective number of allocatable PIDs = kernel.pid_max - 2000 [eviction-signals](https://kubernetes.io/docs/concepts/scheduling-eviction/node-pressure-eviction/#eviction-signals)
* Component Updates
  * CNI plugin updated to version [v1.1.1](https://github.com/containernetworking/plugins/releases/tag/v1.1.1).
  * Virtual-Node updated to version 1.4.5.
  * Updated Azure Disk CSI Driver to [v1.23.0](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.23.0) , Azure File CSI Driver to [v1.22.0](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.22.0) 
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.10.03](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.10.03.txt)

## Release 2022-10-02

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Draft is looking to get feedback. If you have used Draft or are interested in Draft, please click [here](https://github.com/Azure/draft/issues/140) to start a conversation with the AKS team.
* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
* Some AKS labels are being deprecated with the Kubernetes 1.26 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation. `beta.kubernetes.io/arch=` and `beta.kubernetes.io/os=`  are still applied by kubelet in kubernetes code.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Preview Features
  * Cluster snapshot and cluster pinning is now released in [public preview](https://learn.microsoft.com/azure/aks/managed-cluster-snapshot), allowing you to easily save cluster configurations and apply them to other clusters.
  * Vertical Pod Autoscaling is now released in [public preview](https://learn.microsoft.com/azure/aks/vertical-pod-autoscaler). VPA allows you to automatically set resource requests and limits on containers per workload based on past usage.
* Bugs
  * **Bug**: A bug has been found in Windows clusters that have been upgraded to K8s v1.24 that causes external VIP load balancing rules to reference endpoints that no longer exist. The AKS team has rolled out a block on all upgrades to K8s version 1.24 for Windows cluster as we wait for a fix from Windows upstream.
* Component Updates
  * Windows Azure CNI updated to version v1.4.35.
  * Microsoft Defender low-level-collector image updated to v1.3.57.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.09.27](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.09.27.txt)

## Release 2022-09-25

Monitor the release status by regions at [AKS-Release-Tracker](https://releases.aks.azure.com/).

### Announcements

* Draft is looking to get feedback. If you have used Draft or are interested in Draft, please click [here](https://github.com/Azure/draft/issues/140) to start a conversation with the AKS team.
* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
  * Kubernetes 1.21 version has been deprecated as of July 31st, 2022. See [documentation](https://docs.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli) on how to upgrade your cluster.
* Some AKS labels are being deprecated with the Kubernetes 1.25 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Features
  * New Kubernetes patch versions released. Kubernetes 1.22.15, 1.23.12 and 1.24.6.
  * Nodepool snapshots can now work across regions.
* Preview Features
  * THe AKS maintenance window can now also be set centrally via [Azure maintenance windows](https://learn.microsoft.com/azure/aks/aks-planned-maintenance-weekly-releases).
* Behavior Changes
  * AKS no longer uses `beta.kubernetes.io/os`, `failure-domain.beta.kubernetes.io/zone` labels in its managed components. `kubernetes.io/os` and `topology.kubernetes.io/zone` will be used instead, respectively.
  * An additional tag `aks-managed-private-dns-zone-mode:none` will be added to the nodes on private cluster scenarios when the cluster is using 'none' private DNS zone.
* Bug Fixes
  * Fixed KMS error message to clarify when Key Vault has connectivity blocked.
  * Fixed issue with Availability Set-based clusters where node IPs were double counted when performing available IP validations.
* Component Updates
  * Cloud Controller Manager updated to v1.24.7, v1.23.20 and v1.1.23 (for 1.22 and lower) for the respective kubernetes minor versions.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.09.22](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.09.22.txt)

## Release 2022-09-18

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Draft is looking to get feedback. If you have used Draft or are interested in Draft, please click [here](https://github.com/Azure/draft/issues/140) to start a conversation with the AKS team.
* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
  * Kubernetes 1.21 version has been deprecated as of July 31st, 2022. See [documentation](https://docs.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli) on how to upgrade your cluster.
* Some AKS labels have been deprecated with the Kubernetes 1.24 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Features
  * Windows Server 2022 is now GA on AKS. Take a look at our [documentation](https://learn.microsoft.com/azure/aks/upgrade-windows-2019-2022) for guidance on upgrading your workloads from Windows Server 2019 to 2022 and adding new Windows Server 2022 nodepools. Windows Server 2019 will remain default for nodepool creation until kubernetes 1.25. Important, old windows 2019 containers will not work on windows server 2022 hosts.
* Component Updates
  * Virtual Kubelet component of AKS Virtual Nodes was updated to v1.4.4 from v1.4.1 [vk1.4.4](https://github.com/virtual-kubelet/azure-aci/commit/b28784ae5d0d70919357676fda2814bd793de91c).
  * AKS Windows 2019 image has been updated to [17763.3406.220913](vhd-notes/AKSWindows/2019/17763.3406.220913.txt)
  * AKS Windows 2022 image has been updated to [20348.1006.220913](vhd-notes/AKSWindows/2022/20348.1006.220913.txt)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.09.13](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.09.13.txt)

## Release 2022-09-11

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Draft is looking to get feedback. If you have used Draft or are interested in Draft, please click [here](https://github.com/Azure/draft/issues/140) to start a conversation with the AKS team.
* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
  * Windows Server 2022 will be the default Windows host. Important, old windows 2019 containers will not work on windows server 2022 hosts.
  * Azure Cloud Provider for Azure will use [v1.25](https://cloud-provider-azure.sigs.k8s.io/blog/2022/09/05/v1.25.0/)
* Kubernetes 1.21 version has been deprecated as of July 31st, 2022. See [documentation](https://docs.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli) on how to upgrade your cluster.
* Some AKS labels have been deprecated with the Kubernetes 1.24 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Features
  * [AKS as an EventGrid event source](https://docs.microsoft.com/Azure/event-grid/event-schema-aks?tabs=event-grid-event-schema) is now Generally Available.
  * [Updating the Kubelet managed identity](https://docs.microsoft.com/azure/aks/use-managed-identity#update-an-existing-cluster-using-kubelet-identity) is now generally available.
  * [Multi-instance GPU support](https://docs.microsoft.com/azure/aks/gpu-multi-instance) for AKS nodepools is now Generally Available.
  * [Disable CSI Storage Drivers](https://learn.microsoft.com/azure/aks/csi-storage-drivers#disable-csi-storage-drivers-on-a-new-cluster) is now Generally Available.
* Preview Features
  * [Azure CNI Overlay](https://docs.microsoft.com/azure/aks/azure-cni-overlay) now supports 5th generation VM SKUs (v5 SKUs) to be used as nodes.
  * [Image Cleaner](https://docs.microsoft.com/azure/aks/image-cleaner), for removal of insecure container images cached in the nodes, is now in public preview.
  * [Azure Network Policy Manager (NPM) is now supported in public preview for Windows nodepools and containers](https://docs.microsoft.com/azure/aks/use-network-policies#create-an-aks-cluster-with-azure-npm-enabled---windows-server-2022-preview) (using Windows Server 2022). Security rules from [Kubernetes Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/) resources can now be enforced on all pod traffic on/across Linux and Windows Server 2022 nodes for clusters with `--network-policy=azure`. NPM continues to be a managed solution, configurable at cluster creation.
* Behavioral Changes
  * For Kubernetes 1.24+ the services of type `LoadBalancer` with appProtocol HTTP/HTTPS will switch to use HTTP/HTTPS as health probe protocol (while before v1.24.0 it uses TCP). And `/` will be used as the default health probe request path. If your service doesn’t respond `200` for `/`, please ensure you're setting the service annotation `service.beta.kubernetes.io/port_{port}_health-probe_request-path` or `service.beta.kubernetes.io/azure-load-balancer-health-probe-request-path` (applies to all ports) with the correct request path to avoid service breakage.
* Component Updates
  * Update Windows NPM to [v1.4.34](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.34).
  * Update Azure CNI to [v1.4.32](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.32).
  * OSM updated to [v1.2.1](https://github.com/openservicemesh/osm/releases/tag/v1.2.1).
  * Azure Cloud Provider for kubernetes was updated to [v1.24.5](https://cloud-provider-azure.sigs.k8s.io/blog/2022/09/05/v1.24.5/), [v1.23.18](https://cloud-provider-azure.sigs.k8s.io/blog/2022/09/06/v1.23.18/) (for these respective kubernetes minor versions), and to [v1.1.21](https://cloud-provider-azure.sigs.k8s.io/blog/2022/09/06/v1.1.21/) for kubernetes minor version 1.22.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.09.05](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.09.05.txt)

## Release 2022-09-04

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Draft is looking to get feedback. If you have used Draft or are interested in Draft, please click [here](https://github.com/Azure/draft/issues/140) to start a conversation with the AKS team.
* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
  * Windows Server 2022 will be the default Windows host. Important, old windows 2019 containers will not work on windows server 2022 hosts.
* Kubernetes 1.21 version has been deprecated as of July 31st, 2022. See [documentation](https://docs.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli) on how to upgrade your cluster.
* Some AKS labels have been deprecated with the Kubernetes 1.24 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Docker is no longer supported as a container runtime on Windows. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Features
  * [Bring your own Container Network Interface (CNI) plugin with Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/use-byo-cni?tabs=azure-cli) is now generally available.
  * [ARM64 AKS nodepool](https://docs.microsoft.com/azure/aks/use-multiple-node-pools#add-an-arm64-node-pool) is now generally available.
  * AKS now supports aborting a [long running operation](https://docs.microsoft.com/azure/aks/manage-abort-operations?tabs=azure-rest), allowing you to take back control and run another operation seamlessly.
* Preview Features
  * [Azure CNI Overlay for AKS](https://docs.microsoft.com/azure/aks/azure-cni-overlay) is now Public Preview.
* Bug fixes
  * DNS resolution failure due to Ubuntu security patch is fixed.
* Behavior changes
  * The memory limits of liveness-probe container and node-driver-registrar container running in AzureDisk and AzureFile pods on Windows nodes are increased from 100MiB to 150MiB.
* Component Updates
  * The Open Service Mesh addon has been updated from version 1.1.1 to version [1.2.0](https://github.com/openservicemesh/osm/releases/tag/v1.2.0) for AKS clusters running 1.24.0+. Please note the breaking changes mentioned in the [version 1.2.0 release notes](https://github.com/openservicemesh/osm/releases/tag/v1.2.0)
  * The Azure File CSI driver has been updated from v1.20.0 to [v1.21.0](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.21.0)
  * Microsoft Defender for Containers images updated 1.0.70
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.08.29](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.08.29.txt)

## Release 2022-08-21

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
  * Windows Server 2022 will be the default Windows host. Important, old windows 2019 containers will not work on windows server 2022 hosts.
* Kubernetes 1.21 version has been deprecated as of July 31st, 2022. See [documentation](https://docs.microsoft.com/azure/aks/upgrade-cluster?tabs=azure-cli) on how to upgrade your cluster.
* Some AKS labels have been deprecated with the Kubernetes 1.24 release. Update your AKS labels to the recommended substitutions. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Docker will no longer be supported as a container runtime on Windows after September 1, 2022. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.
* The Open Service Mesh addon has been updated from version 1.1.1 to version [1.2.0](https://github.com/openservicemesh/osm/releases/tag/v1.2.0) for AKS clusters running 1.24.0+. Please note the breaking changes mentioned in the [version 1.2.0 release notes](https://github.com/openservicemesh/osm/releases/tag/v1.2.0)

### Release notes

* Bug fixes
  * Missing CWD(Current Working Directory) field in process creation events fixed. Update low level collector image version from 1.3.42 to 1.3.49.
* Component Updates
  * Upgrade Azure Disk V2 CSI Driver to [v2.0.0-beta.6](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v2.0.0-beta.6)
  * Upgrade Azure Disk CSI driver to [v1.22.0](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.22.0)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.08.15](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.08.15.txt)


## Release 2022-08-14

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
  * Windows Server 2022 will be the default Windows host. Important, old windows 2019 containers will not work on windows server 2022 hosts.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Docker will no longer be supported as a container runtime on Windows after September 1, 2022. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Behavioral Changes
 * Remove responseObject from kube-audit logs when its size is reaching log analytics column size limit (32K) and customer enables kube-audit/kube-audit-admin diagnostics.
* Bug fixes
 * Fix bug in processing fractional memory limits on Windows Nodes
 * Fix log loss due to inode reuse on Windows Nodes
 * Fix issue with cert rotation on Windows nodes that caused VMSS inconsistency
 * Removed `Microsoft.Resources/deployments/write`, `Microsoft.Insights/alertRules/*`, and `Microsoft.Support/*` from the [built-in Azure RBAC data plane roles for AKS](https://docs.microsoft.com/azure/aks/manage-azure-rbac#create-role-assignments-for-users-to-access-cluster).
* Component Updates
  * Azure Monitor for container insights addon updated for Windows to [win-ciprod08102022](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#08102022--)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.08.10](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.08.10.txt)
  * AKS Windows 2019 image has been updated to [17763.3287.220810](vhd-notes/AKSWindows/2019/17763.3287.220810.txt)
  * AKS Windows 2022 image has been updated to [20348.887.220810](vhd-notes/AKSWindows/2022/20348.887.220810.txt)

## Release 2022-08-07

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with Kubernetes 1.25, the following changes will be made default:
  * Ubuntu 22.04 for x86, AMD and ARM64 architectures will be the default host.
  * Windows Server 2022 will be the default Windows host. Important, old windows 2019 containers will not work on windows server 2022 hosts.
* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
  * metrics-server vertical pod autoscaler will be enabled.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Konnectivity rollout is finished in global and started in Sovereign (China, USGov).
* Docker will no longer be supported as a container runtime on Windows after September 1, 2022. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Features
  * GA of Kubernetes 1.24
* Behavioral Changes
  * Deprecation of Kubernetes 1.21
  * Increased memory request (20Mi -> 40Mi) for azuredisk and node-driver-registrar containers in azurediskcsi-azuredisk-v2-node
* Component Updates
  * Calico is updated to [v3.21.6](https://github.com/projectcalico/calico/releases/tag/v3.21.6)
  * CSI Secret Store now supports Windows Server 2022
  * Microsoft Defender for Containers images updated 1.0.67
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.08.02](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.08.02.txt).

## Release 2022-07-31

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with Kubernetes 1.25, the host VM operating system will be Ubuntu 22.04 for Intel and ARM64 architectures

* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
  * metrics-server vertical pod autoscaler will be enabled.
* Kubernetes 1.21 version deprecation will start taking effect from July 31st, 2022.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Konnectivity rollout is finished in global and started in Sovereign (China, USGov).
* Docker will no longer be supported as a container runtime on Windows after September 1, 2022. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Features
  * [Dedicated Host Support](https://docs.microsoft.com/azure/aks/use-azure-dedicated-hosts) is now generally available.
  * [KMS etcd encryption](https://docs.microsoft.com/azure/aks/use-kms-etcd-encryption) is now generally available.
  * [Confidential Virtual Machines](https://docs.microsoft.com/azure/aks/use-cvm) is now in Public Preview.
* Behavioral Changes
  * Use QuotaExceeded error code instead of OperationNotAllowed when receiving quota exceed errors from ARM
* Bug Fixes
  * Azure Monitor for Containers, fixes [issue](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#07272022--) with node allocatable cpu and memory value when limits are not set
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.07.28](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.07.28.txt).

## Release 2022-07-24

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
  * metrics-server vertical pod autoscaler will be enabled.
* Kubernetes 1.21 version deprecation will start taking effect from July 31st, 2022.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Konnectivity rollout is finished in global and started in Sovereign (China, USGov).
* Docker will no longer be supported as a container runtime on Windows after September 1, 2022. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Preview Features
  * Draft is now available in VsCode through the AKS DevX extension. To install the DevX extension for Vscode, check out the [marketplace](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.aks-devx-tools). To check out the open source code, visit the [GitHub repo](https://github.com/Azure/aks-devx-tools).
  * Automated Deployments is now Public Preview on AKS. Automated Deployments allows you to take your containerized application and deploy it to an AKS cluster easily with GitHub Actions. [Read more here](https://docs.microsoft.com/azure/aks/automated-deployments).
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.07.18](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.07.18.txt).
  * AKS Windows 2019 image has been updated to [17763.3232.220722](vhd-notes/AKSWindows/2019/17763.3232.220722.txt).
  * AKS Windows 2022 image has been added with version [20348.859.220722](vhd-notes/AKSWindows/2022/20348.859.220722.txt).

## Release 2022-07-17

Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
  * metrics-server vertical pod autoscaler will be enabled.
* Kubernetes 1.21 version deprecation will start taking effect from July 31st, 2022.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Konnectivity rollout is finished in global and started in Sovereign (China, USGov).
* Docker will no longer be supported as a container runtime on Windows after September 1, 2022. Follow these [steps](https://docs.microsoft.com/azure/aks/learn/quick-windows-container-deploy-cli#:~:text=Upgrade%20an%20existing%20Windows%20Server%20node%20pool%20to%20containerd) in our documentation to upgrade your Kubernetes cluster to change your container runtime to containerd.

### Release notes

* Preview Features
  * KEDA Addon is now supported on ARM64-based nodes.
  * [Azure Blob CSI Driver](https://github.com/kubernetes-sigs/blob-csi-driver#set-up-csi-driver-on-aks-cluster-only-for-aks-users) is now supported in public preview in AKS. Follow these [instructions](https://docs.microsoft.com/azure/aks/azure-blob-csi?tabs=NFS) to use blob csi driver as a managed addon to mount blob storage to a pod via blobfuse or NFS 3.0 options.
* Features
  * The annotation `kubernetes.azure.com/set-kube-service-host-fqdn` can now be added to pods to set the KUBERNETES_SERVICE_HOST variable to the domain name of the API server instead of the in-cluster service IP. This is useful in cases where the cluster egress is via a layer 7 firewall, like Azure Firewall with Application Rules.
* Bug Fixes
  * Fixed issue where removed nodepool labels would still incorrectly show on autoscaled nodes.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.07.11](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.07.11.txt).
  * AKS Windows 2019 image has been updated to [17763.3165.220713](vhd-notes/AKSWindows/2019/17763.3165.220713.txt).
  * AKS Windows 2022 image has been added with version [20348.825.220713](vhd-notes/AKSWindows/2022/20348.825.220713.txt).

## Release 2022-07-10

This release is rolling out to all regions - estimated time for completed roll out is 2022-07-22 for public cloud and 2022-07-25 for sovereign clouds.
Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
  * metrics-server vertical pod autoscaler will be enabled.
* Kubernetes 1.21 version deprecation will start taking effect from July 31st, 2022.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Konnectivity rollout is finished in global and started in Sovereign (China, USGov).

### Release notes

* Features
  * [Microsoft Defender cloud-native security agent for AKS clusters](https://docs.microsoft.com/azure/defender-for-cloud/defender-for-containers-enable?tabs=aks-deploy-portal%2Ck8s-deploy-asc%2Ck8s-verify-asc%2Ck8s-remove-arc%2Caks-removeprofile-api&pivots=defender-for-container-aks) is now generally available.
* Bug Fixes
  * The nodepools will not inherit node resource group tags in `az aks create --tags` and  `az aks update --tags` scenarios. Because nodepools have  `az aks nodepool add --tags` and `az aks nodepool update --tags`.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.07.04](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.07.04.txt).
  * omsagent update [ciprod06272022](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#06272022--).

## Release 2022-07-03

This release is rolling out to all regions - estimated time for completed roll out is 2022-07-15 for public cloud and 2022-07-18 for sovereign clouds.
Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with this release, the pod memory limit for Azure NPM has been increased from 300 MB to 1 GB for clusters with the uptime SLA enabled. Requests will stay at 300 MB.
* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
  * metrics-server vertical pod autoscaler will be enabled.
* Kubernetes 1.21 version deprecation will start taking effect from July 31st, 2022.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Kubernetes patch versions 1.21.14, 1.22.11, and 1.23.8 are now available; Kubernetes patch versions 1.21.7, 1.22.4, and 1.23.3 are deprecated and removed. Learn more about Kubernetes version support policy followed by AKS [here](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions?tabs=azure-cli#kubernetes-version-support-policy).
* Konnectivity rollout is done for most regions. Targeting end of this week for completion of rollout to the remaining regions - `centralus, westus, germanynorth, westeurope, australiacentral2, australiasoutheast, brazilsoutheast, canadaeast, francesouth, japanwest, jioindiacentral, koreasouth, norwaywest, southafricawest, southcentralus, southeastasia, southindia, swedensouth, switzerlandwest, uaecentral, westus3`.

### Release notes

* Features
  * [Node pool start/stop](https://docs.microsoft.com/azure/aks/start-stop-nodepools) is now generally available.
* Bug Fixes
* Fixed issue on 1.24+ clusters with Windows node pools and Calico as network policy to automatically create the service account required for installing Calico.
* Set `priorityClassName` to `system-node-critical` for Azure Key Vault Provider for Secrets Store CSI Driver addon to prevent scheduling issues arising from saturation by non-critical workloads.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.06.29](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.06.29.txt).

## Release 2022-06-26

This release is rolling out to all regions - estimated time for completed roll out is 2022-07-08 for public cloud and 2022-07-11 for sovereign clouds.
Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with the July 3rd, 2022 AKS release, Azure NPM will increase its pod memory limit from 300 MB to 1 GB for clusters with the uptime SLA enabled. Requests will stay at 300 MB.
* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
  * metrics-server vertical pod autoscaler will be enabled.
* Kubernetes 1.21 version deprecation will start taking effect from July 31st, 2022.
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of June.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Features
  * [Calico Network Policy](https://docs.microsoft.com/azure/aks/use-network-policies#create-an-aks-cluster-for-calico-network-policies) is now generally available for Windows Server 2019 and 2022. This new feature allows customers to use network policies with Windows Server on AKS. Customers can also enable and use both Linux and Windows network policies in a single cluster. This feature will be available from Kubernetes 1.20. Please take note of [common issues related to this change in our troubleshooting documentation.](https://docs.microsoft.com/azure/aks/troubleshooting#windows-containers-have-connectivity-issues-after-a-cluster-upgrade-operation)
* Preview Features
  * [API Server VNet Integration](https://docs.microsoft.com/azure/aks/api-server-vnet-integration) is available in preview.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.06.22](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.06.22.txt).
  * AKS Windows 2019 image has been updated to [17763.3046.220624](vhd-notes/AKSWindows/2019/17763.3046.220624.txt).
  * AKS Windows 2022 image has been added with version [20348.768.220624](vhd-notes/AKSWindows/2022/20348.768.220624.txt).
  * Application Gateway Ingress Controller add-on has been updated to version [1.5.2](https://github.com/Azure/application-gateway-kubernetes-ingress/releases/tag/1.5.2).
  * The Open Service Mesh addon image has been updated from version 1.0.0 to [version 1.1.1](https://github.com/openservicemesh/osm/releases/tag/v1.1.1) for AKS clusters running 1.23.5+. Please note the **breaking change** mentioned in the [version 1.1.0 release notes](https://github.com/openservicemesh/osm/releases/tag/v1.1.0).

## Release 2022-06-19

This release is rolling out to all regions - estimated time for completed roll out is 2022-07-01 for public cloud and 2022-07-04 for sovereign clouds.
Monitor the release status by regions at [AKS-Release-Tracker](http://aka.ms/aks/release-tracker).

### Announcements

* Starting with the June 26th, 2022 AKS release, Azure NPM will increase its pod memory limit from 300 MB to 1 GB for clusters with the uptime SLA enabled. Requests will stay at 300 MB.
* Starting with Kubernetes 1.24, the following changes will be made default:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled.  This will allow users to enable/disable node restriction.
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
* Kubernetes 1.21 version deprecation will start taking effect from July 31st, 2022.
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of June.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Preview Features
  * Disable [CSI Storage Drivers](https://docs.microsoft.com/azure/aks/azure-disk-csi) available in preview.
* Behavioral Changes
  * PersistentVolumeClaim mounts will now work in clouds with custom root CAs.
  * Nodepool snapshots will only allow taking snapshots from Nodepools with provisioning status as Succeeded.
* Bug Fixes
  * Fixed issue that prevented KEDA from scaling workloads. This could be observed previously as following status condition when describing the HorizontalPodAutoscaler for the KEDA scaled object: `Cannot list resource "<external-metric-name>" in API group "external.metrics.k8s.io " in the namespace "<namespace-name>": RBAC: clusterrole.rbac.authorization.k8s.io  "keda-operator-external-metrics-reader" not found`.
  * Update cloud-controller-manager versions to v1.24.2, v1.23.14, v1.1.17, v1.0.21 for Kubernetes 1.24, 1.23, 1.22, and 1.21 -
    * A new annotation is added in order to specify the PublicIP Prefix for creating IP of LB-service.beta. kubernetes.io/azure-pip-prefix-id: "/subscriptions/8ecadfc9-ffff-4ea4-ffff-0d9f87e4d7c8/resourceGroups/lodrem/providers/Microsoft.Network/publicIPPrefixes/bb" [#1848](https://github.com/kubernetes-sigs/cloud-provider-azure/pull/1848).
    * Fix unexpected managed PLS deletion issue when ILB subnet is specified. [#1835](https://github.com/kubernetes-sigs/cloud-provider-azure/pull/1835)
    * Fix: avoid unnecessary NSG updating on service reconciling [#1850](https://github.com/kubernetes-sigs/cloud-provider-azure/pull/1850)
    * Fix: panic when create private endpoint using azurefile NFS [#1816](https://github.com/kubernetes-sigs/cloud-provider-azure/pull/1816)
    * Remove redundant restriction on pls autoApproval and visibility.User can specify a list of  subscriptions for visibility (e.g. "sub1 sub2") and a subset of this list for autoApproval (e.g. "sub1"). [#1867](https://github.com/kubernetes-sigs/cloud-provider-azure/pull/1867)
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.06.13](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.06.13.txt).
  * AKS Windows 2019 image has been updated to [17763.2928.220615](vhd-notes/AKSWindows/2019/17763.3046.220615.txt).
  * AKS Windows 2022 image has been added with version [20348.707.220525](vhd-notes/AKSWindows/2022/20348.768.220615.txt).
  * Updated Windows containerd package to v1.6.6
  
## Release 2022-06-12

This release is rolling out to all regions - estimated time for completed roll out is 2022-06-24 for public cloud and 2022-06-27 for sovereign clouds.

### Announcements

* Starting with the June 26th, 2022 AKS release, Azure NPM will increase its pod memory limit from 300 MB to 1 GB for clusters with the uptime SLA enabled. Requests will stay at 300 MB.
* Starting with Kubernetes 1.24, the following changes will be made:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of June.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Behavioral Changes
  * [Upgrades spot node pools](https://docs.microsoft.com/azure/aks/spot-node-pool#upgrade-a-spot-node-pool) is now available starting this week: When upgrading a spot node pool, AKS will issue a cordon and an eviction notice, but no drain is applied. There are no surge nodes available for spot node pool upgrades.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.06.08](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.06.08.txt).
  * Upgrade Azure File CSI driver to [v1.19.0](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.19.0)
  * Upgrade Azure Disk CSI driver to [v1.19.0](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.19.0)
  * Cloud-controller-manager, Azure SDK, & API version has been updated for v1.21.7 and v1.21.9 (see the [version matrix](https://github.com/kubernetes-sigs/cloud-provider-azure#version-matrix) to see which CCM version maps to which AKS version.  

## Release 2022-06-05

This release is rolling out to all regions - estimated time for completed roll out is 2022-06-17 for public cloud and 2022-06-20 for sovereign clouds.

### Announcements

* Starting with the June 26th, 2022 AKS release, Azure NPM will increase its pod memory limit from 300 MB to 1 GB for clusters with the uptime SLA enabled. Requests will stay at 300 MB.
* Starting with Kubernetes 1.24, the following changes will be made:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled
  * CoreDNS version 1.9.2 will be default version. With this new version of CoreDNS wildcard queries are no longer allowed.
  * metrics-server version 0.6.1 will be the default version.
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of June.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Features
  * [AKS Release Tracker](https://docs.microsoft.com/azure/aks/release-tracker) is now generally available.
* Behavioral Changes
  * Set agentPoolProfile default maxPods for new agentpools to align with the expected default maxPods based on the cluster's network configuration.
  * Reverted the changes of request values to api server to reduce churn on Uptime SLA enabled AKS clusters.
  * Konnectivity agent now uses a new Service Account konnectivity-agent, instead of the default Service Account.
* Bug fixes
  * CSI Secret Store removed limit of node-driver-registrar to address[AKS Issue](https://github.com/Azure/AKS/issues/2972)
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.05.31](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.05.31.txt).
  
## Release 2022-05-29

This release is rolling out to all regions - estimated time for completed roll out is 2022-06-10 for public cloud and 2022-06-13 for sovereign clouds.

### Announcements

* Starting with Kubernetes 1.24, the following changes will be made:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of June.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Features
  * Azure Key Vault with Private Link with KMS is now [supported](https://docs.microsoft.com/azure/aks/use-kms-etcd-encryption)
  * Preview of Kubernetes 1.24
* Bug fixes
  * Add extra information in error messages when a subnet is full or drain issues are found
* Component Updates
  * Upgrade Azure File CSI driver to [v1.18.0](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.18.0)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.05.24](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.05.24.txt).
  * AKS Windows 2019 image has been updated to [17763.2928.220525](vhd-notes/AKSWindows/2019/17763.2928.220525.txt).
  * AKS Windows 2022 image has been added with version [20348.707.220525](vhd-notes/AKSWindows/2022/20348.707.220525.txt).

## Release 2022-05-22

This release is rolling out to all regions - estimated time for completed roll out is 2022-06-03 for public cloud and 2022-06-06 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerd will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with Kubernetes 1.24, the following changes will be made:
  * The default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
  * The [NodeRestriction Admission Controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction) will be enabled
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of May.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Features
  * [AKS Cluster Extensions](https://azure.microsoft.com/updates/generally-available-aks-cluster-extensions/) is now generally available.
  * [Azure CNI dynamic IP allocation and enhanced subnet support](https://azure.microsoft.com/updates/generally-available-dynamic-ip-allocation-and-enhanced-subnet-support-in-aks/) is now generally available.
  * [Alias minor version](https://azure.microsoft.com/updates/generally-available-alias-minor-version-support-in-aks/) is now generally available.
  * [Custom node configuration](https://azure.microsoft.com/updates/generally-available-custom-node-configuration-on-aks/) is now generally available.
  * [Subnet per node pool](https://azure.microsoft.com/updates/generally-available-subnet-per-node-pool/) is now generally available.
* Preview features
  * [ARM64 agent pools](https://azure.microsoft.com/updates/public-preview-arm64-agent-node-support-in-aks/) is now in public preview.
  * [Azure Disk CSI driver v2](https://azure.microsoft.com/updates/public-preview-azure-disk-csi-driver-v2-in-aks/) is now in public preview.
  * [Draft extension for Azure Kubernetes Service (AKS)](https://azure.microsoft.com/updates/public-preview-draft-extension-for-azure-kubernetes-service-aks/) is now in public preview.
  * [KEDA add-on](https://azure.microsoft.com/updates/public-preview-keda-addon-for-aks/) is now in public preview.
  * [Web application routing add-on](https://azure.microsoft.com/updates/public-preview-web-application-routing-addon-for-azure-kubernetes-service-aks/) is now in public preview.
  * [Windows Server 2022 host support](https://azure.microsoft.com/updates/public-preview-windows-server-2022-host-support-in-aks/) is now in public preview.
* Bug fixes
  * BYOCNI nodes will no longer be provisioned with additional secondary IPs
  * Calls to admission webhooks in Konnectivity clusters will properly use the Konnectivity tunnel to reach the webhook URL
* Component Updates
  * Azure Disk CSI driver has been updated to [v1.18.0](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.18.0)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.05.10](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.05.04.txt).
  * AKS Windows 2019 image has been updated to [17763.2928.220511](vhd-notes/AKSWindows/2019/17763.2928.220511.txt).
  * AKS Windows 2022 image has been added with version [20348.707.220511](vhd-notes/AKSWindows/2022/20348.707.220511.txt).
  * Cloud controller manager has been updated to versions [v1.23.12](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.23.12)/[v1.1.15](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.1.15)/[v1.0.19](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.0.19) (see the [version matrix](https://github.com/kubernetes-sigs/cloud-provider-azure#version-matrix) to see which CCM version maps to which AKS version)
  * CoreDNS has been updated to [v1.8.7](https://github.com/coredns/coredns/releases/tag/v1.8.7) for AKS clusters >=1.20.0. Clusters before 1.20.0 remain on 1.6.6.
  * external-dns has been updated to [v0.10.2](https://github.com/kubernetes-sigs/external-dns/releases/tag/v0.10.2)

## Release 2022-05-08

This release is rolling out to all regions - estimated time for completed roll out is 2022-05-21 for public cloud and 2022-05-24 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerd will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of May.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Public preview
  * [AKS now supports updating kubelet on node pools to use a new or changed user-assigned managed identity.](https://docs.microsoft.com/azure/aks/use-managed-identity#update-an-existing-cluster-using-kubelet-identity-preview)
* Bug Fixes
  * Fixes a bug with the AKS-EnableDualStack preview feature that would delete managed outbound IPv6 IPs if updating the cluster with a version of the API before the dual-stack parameters were added.
  * A validation to prevent adding clusters to a subnet with a NAT Gateway without setting the appropriate outboundType was applied to updates as well as creates, preventing changes to clusters in this situation. The validation has been removed from update calls.
* Component Updates
  * Azure File CSI driver has been updated to [v1.6](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.16.0)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.05.04](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.05.04.txt).

## Release 2022-05-01

This release is rolling out to all regions - estimated time for completed roll out is 2022-05-13 for public cloud and 2022-05-16 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerd will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of May.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Public preview
  * The `aks-preview` Azure CLI extension (version 0.5.66+) now supports running `az aks update -g <resourceGroup> -n <clusterName>` without any optional arguments. This will perform an update operation without performing any changes, which can recover a cluster stuck in a failed provisioning state.
  * [AKS now supports updating kubelet on node pools to use a new or changed user-assigned managed identity.](https://docs.microsoft.com/azure/aks/use-managed-identity#update-an-existing-cluster-using-kubelet-identity-preview)
* Behavioral changes
  * Kube-proxy now detects local traffic using the local interface subnet instead of cluster CIDR when using Azure CNI. For clusters that have agent pools in separate subnets, this ensures that kube-proxy NAT rules do not interfere with network policies enforced by Azure NPM. The configuration change applies to clusters running Azure CNI and Kubernetes version 1.23.3 or later.
  * Clusters deployed with outboundType loadBalancer but deployed in a subnet with an attached NAT gateway will be updatable. Deployment of clusters into a bring-your-own-vnet subnet with a NAT Gateway already attached will be blocked unless `outboundType userAssignedNATGateway` is passed. See [NAT Gateway](https://docs.microsoft.com/azure/aks/nat-gateway) in the AKS Documentation for more details.
* Component Updates
  * Azure CNI has been updated to [v1.4.22](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.22).
  * [Cloud Provider Azure](https://kubernetes-sigs.github.io/cloud-provider-azure/) is being upgraded to [v1.23.11](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.23.11)/[v1.1.14](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.1.14)/[v1.0.18](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v1.0.18)/[v0.7.21](https://github.com/kubernetes-sigs/cloud-provider-azure/releases/tag/v0.7.21) (depending on AKS cluster version).
  
## Release 2022-04-24

This release is rolling out to all regions - estimated time for completed roll out is 2022-05-06 for public cloud and 2022-05-09 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerd will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Konnectivity rollout will continue in May 2022 and is expected to complete by end of May.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Preview features
  * AKS now supports enabling [encryption at rest for data in etcd](https://docs.microsoft.com/azure/aks/use-kms-etcd-encryption) using Azure Key Vault with Key Management Service (KMS) plugin.
* Bug Fixes
  * Fixed CSI driver version display issue in Azure disk and file CSI Driver objects.
  * Fixed bug where cloud-controller-manager was not deleting Node Object after deletion of VMSS instance.
* Behavioral changes
  * Taints and labels applied using the AKS nodepool API are not modifiable from the Kubernetes API and vice versa.
* Component Updates
  * Azure Disk CSI driver has been updated to [1.16](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.16.0).
  * Azure File CSI driver has been rolled back to [1.12](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.12.0) to avoid storage account creation every time when a new Azure file share volume is created.
  * On AKS clusters of versions >= 1.22, nginx-ingress-controller images are updated from 1.0.5 to 1.2.0 to address [CVE-2021-25745](https://github.com/Azure/AKS/issues/2906) and [CVE-2021-25746](https://github.com/Azure/AKS/issues/2909) vulnerabilities.  
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.04.27](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.04.27.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2803.220413](vhd-notes/AKSWindows/2019/17763.2803.220413.txt).

## Release 2022-04-03

This release is rolling out to all regions - estimated time for completed roll out is 2022-04-15 for public cloud and 2022-04-18 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerd will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.
* Kubernetes version 1.20 will be deprecated and removed from AKS on April 7th 2022.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.

### Release notes

* Preview Features
  * AKS now supports [Host Process Containers](https://kubernetes.io/docs/tasks/configure-pod-container/create-hostprocess-pod/) as a preview feature on versions 1.23+.
* Features
  * [Custom node configuration](https://docs.microsoft.com/azure/aks/custom-node-configuration) for AKS is now generally available.
  * [gMSAv2 security policy support on Windows](https://docs.microsoft.com/azure/aks/use-group-managed-service-accounts) is now generally available.
* Bug Fixes
  * Fixed a bug where deployments done via the AKS run command would incorrectly display a server error when pods in a deployment did not become ready in 30s. This is now correctly flagged as a client error and will ask the user to retry or take action to ensure the pods of the deployment become ready within the allocated time.
* Component Updates
  * Azure Keyvault Secrets Provider has been updated to [v1.1.0](https://github.com/Azure/secrets-store-csi-driver-provider-azure/releases/tag/v1.1.0).
  * Azure Disk CSI driver has been updated to [1.14](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.14.0).
  * Azure File CSI driver has been updated to [1.13](https://github.com/kubernetes-sigs/azurefile-csi-driver/releases/tag/v1.13.0).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.03.29](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.03.29.txt).

## Release 2022-03-27

This release is rolling out to all regions - estimated time for completed roll out is 2022-04-08 for public cloud and 2022-04-11 for sovereign clouds.

### Announcements

* Upgrade your AKS Ubuntu 18.04 worker nodes to VHD version [2022.03.20](https://github.com/Azure/AgentBaker/blob/master/vhdbuilder/release-notes/AKSUbuntu/gen1/1804/2022.03.20.txt) or newer to address [CVE-2022-0492](https://github.com/Azure/AKS/issues/2834) and [CVE-2022-23648](https://github.com/Azure/AKS/issues/2821).
* From Kubernetes 1.23, containerd will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.
* Kubernetes version 1.20 will be deprecated and removed from AKS on April 7th 2022.
* Update your AKS labels to the recommended substitutions before deprecation after the Kubernetes v1.24 release. See more information on label deprecations and how to update your labels in the [Use labels in an AKS cluster](https://docs.microsoft.com/azure/aks/use-labels) documentation.
* Node Pool Snapshot CLI experience is changing by April 6,  2022. The current nodepool snapshot commands i.e az `aks snapshot` will now be `az aks nodepool snapshot`.

### Release notes

* Preview Features
  * You can now [Bring your Own CNI plugin to AKS](https://docs.microsoft.com/azure/aks/use-byo-cni)
* Features
  * Node pool [Scale-down Mode](https://docs.microsoft.com/azure/aks/scale-down-mode) is not Generally available and supports Spot Node Pools.
* Bug Fixes
  * Fixed <https://github.com/kubernetes-sigs/cloud-provider-azure/pull/1317> in kubernetes v1.22+.
  * Fixed <https://github.com/kubernetes-sigs/cloud-provider-azure/pull/1346> in kubernetes v1.22+.
  * Fixed bug with auto-scaling from zero with pods that utilize an `agentpool=` label selector.
  * Fixed bug for IPv6-enabled clusters using OpenVPN and BYO VNET that checked the incorrect IPv6 CIDR.
* Behavioral changes
  * An AKS API call on the cluster after a control plane upgrade was incorrectly causing many nodepool upgrades. We have amended the behavior such that if you dont specify nodepools or specify some nodepools in the call, then the nodepools are not upgraded to the control plane version implicitly. In order to upgrade the nodepools following the control plane upgrade, an explicit kubernetes version upgrade in the respective nodepool(s) should be added in the request.
* Component Updates
  * Azure CNI for Windows updated to [v1.4.22](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.22).
  * Azure Disk CSI driver to [v1.13.0](https://github.com/kubernetes-sigs/azuredisk-csi-driver/releases/tag/v1.13.0).
  * Azure Monitor for Containers addon updated to [ciprod03172022](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#3172022--).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.03.23](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.03.23.txt).

## Release 2022-03-20

This release is rolling out to all regions - estimated time for completed roll out is 2022-04-03 for public cloud and 2022-04-06 for sovereign clouds. Please note that the AKS release cadence has shifted; new releases will now be cut on Sunday.

### Announcements

* Upgrade your AKS Ubuntu 18.04 worker nodes to VHD version [2022.03.20](https://github.com/Azure/AgentBaker/blob/master/vhdbuilder/release-notes/AKSUbuntu/gen1/1804/2022.03.20.txt) or newer to address [CVE-2022-0492](https://github.com/Azure/AKS/issues/2834) and [CVE-2022-23648](https://github.com/Azure/AKS/issues/2821).
* From Kubernetes 1.23, containerd will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.
* Kubernetes version 1.20 will be deprecated and removed from AKS on April 7th 2022.

### Release notes

* Behavioral changes
  * Accelerated networking will now be enabled by default for newly-created Windows nodepools.
  * The [single placement group VMSS flag](https://docs.microsoft.com/azure/virtual-machines/sizes-hpc#cluster-configuration-options) will now be enabled for newly-created node pools using InfiniBand/RDMA-capable VM sizes. InfiniBand/RDMA-capable SKUs, like most [H-series](https://docs.microsoft.com/azure/virtual-machines/sizes-hpc) and some [N-series](https://docs.microsoft.com/azure/virtual-machines/sizes-gpu) sizes, can be identified by the "r" in the additional features section of the size name (e.g. Standard_HB120**r**s_v3, Standard_ND96as**r**_v4). Note that the InfiniBand drivers are not currently loaded to nodes. Loading these via a DaemonSet will come in the near future.
* Bug fixes
  * The 2022.03.20+ AKS Ubuntu 18.04 images fix an issue (present since 2022.02.19) in which an unneeded Azure security agent was installed, leading to higher than expected memory consumption on nodes.
  * Improved error handling to resolve a bug where a cluster stop operation may show an inconsistent state, leading to a cluster that is stuck in the "Stopping" state or moves to the "Failed" state. If a cluster is stuck in this state currently, running `az resource update --ids <cluster resource ID>` should resolve the issue.
* Features
  * [Node pool snapshot](https://docs.microsoft.com/azure/aks/node-pool-snapshot) is now GA.
* Component updates
  * Containerd updated to 1.6 for AKS Windows nodes on AKS v1.23+
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.03.20](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.03.20.txt)
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2686.220317](vhd-notes/AKSWindows/2019/17763.2686.220317.txt).

## Release 2022-03-10

This release is rolling out to all regions - estimated time for completed roll out is 2022-03-23 for public cloud and 2022-03-26 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.
* Kubernetes version 1.20 will be deprecated and removed from AKS on April 7th 2022.

### Release notes

* Component updates
  * AKS clusters >= 1.19 will now have Application Gateway Ingress Controller (AGIC) version 1.5.1 which adds support for ingress class and path prefix
  * Upgrade Azure disk CSI driver to 1.12.0 on 1.21+ clusters
  * Upgrade Azure Defender pod-collector image to 0.3.19 from 0.3.18
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.03.07](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.03.07.txt)
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2686.220309](vhd-notes/AKSWindows/2019/17763.2686.220309.txt).

## Release 2022-03-03

This release is rolling out to all regions - estimated time for completed roll out is 2022-03-16 for public cloud and 2022-03-19 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.
* Kubernetes version 1.20 will be deprecated and removed from AKS on April 7th 2022.
* AKS x OSS Integration Blog Series: This month’s article highlights how to deploy a highly available Redis Cluster to AKS. [Run scalable and resilient Redis with Kubernetes and Azure Kubernetes Service - Microsoft Tech Community](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/run-scalable-and-resilient-redis-with-kubernetes-and-azure/ba-p/3247956). Previous two articles explore storing [Prometheus metrics with Thanos/AKS](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/store-prometheus-metrics-with-thanos-azure-storage-and-azure/ba-p/3067849) and [Cluster monitoring with Prometheus/Grafana/AKS](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/using-azure-kubernetes-service-with-grafana-and-prometheus/ba-p/3020459).

### Release notes

* Preview features
  * Associate capacity reservation to node pools is now previewed in all regions. Documentation available [here](https://docs.microsoft.com/azure/aks/use-multiple-node-pools#associate-capacity-reservation-groups-to-node-pools-preview).
* Component updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.03.03](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.03.03.txt) contains hotfix for [containerd-1602](https://github.com/Azure/AgentBaker/pull/1602).
  * Introducing Prometheus performance metrics, measuring execution time of handling pod/namespace/network policy CRUD events. The pre-existing npm_add_policy_exec_time metric now has an "error" label.

## Release 2022-02-24

This release is rolling out to all regions - estimated time for completed roll out is 2022-03-09 for public cloud and 2022-03-12 for sovereign clouds.

### Announcements

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Starting with 1.24 the default format of clusterUser credential for AAD enabled clusters will be ‘exec’, which requires [kubelogin](https://github.com/Azure/kubelogin) binary in the execution PATH. If you are using Azure CLI, it will prompt users to download kubelogin. There will be no behavior change for non-AAD clusters, or AAD clusters whose version is older than 1.24. Existing downloaded kubeconfig will still work. We provide an optional query parameter ‘format’ when getting clusterUser credential to overwrite the default behavior change, you can explicitly specify format to ‘azure’ to get old format kubeconfig.
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.

### Release notes

* Behavioral changes
  * The default VNET address for managed VNETs will change from 10.0.0.0/8 to 10.240.0.0/16 and the default node subnet address will change from 10.224.0.0/12 to 10.224.0.0/16. New clusters will be required to have service and pod CIDR ranges that do not overlap with these new VNET ranges.
* Bug fixes
  * Fix azure disk resize timeout issue on aks 1.21+
* Preview features
  * Associate capacity reservation to node pools. Documentation available [here](https://docs.microsoft.com/azure/aks/use-multiple-node-pools#associate-capacity-reservation-groups-to-node-pools-preview).
* Component updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.02.19](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.02.19.txt).
  * Azure Policy for AKS updated to [Gatekeeper 3.7.1](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.7.1)

## Release 2022-02-17

This release is rolling out to all regions - estimated time for completed roll out is 2022-03-02 for public cloud and 2022-03-05 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout will continue in Feb 2022.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.

### Release Notes

* Component Updates
  * Calico updated to v3.21.4 on Windows
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.02.15](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.02.15.txt).

## Release 2022-02-10

This release is rolling out to all regions - estimated time for completed roll out is 2022-02-23 for public cloud and 2022-02-26 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout will continue in Feb 2022.
* Kubernetes 1.19 has been removed.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.

### Release Notes

* Behavioral changes
  * We now limit the OIDC issuer preview feature to 1.20+
  * Increased liveness/readiness probe timeout to 10 seconds for metrics server
* Component Updates
  * OSM addon updated to v1.0.0
  * Calico updated to v3.21.4 on Linux w/ operator managing CRDs
  * Azure file updated to v1.10.0 on aks 1.21+
  * omsagent update [ciprod01312022 & win-ciprod01312022](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#1312022--)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.02.07](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.02.07.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2565.220211](vhd-notes/AKSWindows/2019/17763.2565.220211.txt).

## Release 2022-02-06

This release is rolling out to all regions - estimated time for completed roll out is 2022-02-16 for public cloud and 2022-02-19 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout will continue in Feb 2022.
* Kubernetes 1.19 will be removed on the next release.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.

### Release Notes

* Behavioral changes
  * Increase CPU limit on Windows OMS agent from 200mc to 500mc
  * GA AKS Tags now allows Patch tags to managedCluster which will also patch tags to child ARM resources {NetworkSecurityGroup, LoadBalancer, virtualNetwork}
* Bug Fixes
  * Fix azure file NFS mount permissions and enable azure file volume stats by default on AKS 1.21+
  * Upgraded Linux version to 5.4.0-1068.70-azure to address [CVE-2021-4034](https://github.com/Azure/AKS/issues/2756)
* Preview Features
  * Kubernetes 1.23.3
  * Enable ephemeral OS on [temp disk](https://aka.ms/aks/ephemeral-os-temp) for v5 VM instances
* Component Updates
  * Kubernetes 1.20.15, 1.21.9 and 1.22.6 released, 1.20.9, 1.21.2, and 1.22.2 removed
  * Upgraded Linux version to 5.4.0-1068.70-azure to address [CVE-2021-4034](https://github.com/Azure/AKS/issues/2756 )
  * Containerd registry configuration for Linux nodes - including adding root CAs for containerd via DS.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.02.01](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.02.01.txt).

## Release 2022-01-27

This release is rolling out to all regions - estimated time for completed roll out is 2022-02-07 for public cloud and 2022-02-10 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout will continue in Feb 2022.
* Kubernetes 1.19 will be removed on the next release.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.

### Release Notes

* Behavioral changes
  * AKS will now create pseudo-random IPv6 address ranges for the Kubernetes pod and service IPs for new dual-stack clusters when --pod-cidrs or --service-cidrs are omitted instead of a default static value. These ranges will be generated with the method suggested in RFC 4193.
  * Removed secret RBAC for azure disk csi driver.
  * Increased csi-resizer timeout from 60s to 120s.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.01.24](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.01.24.txt). Upgraded Linux version to 5.4.0-1067.70-azure to address CVE-2022-0185 (<https://github.com/Azure/AKS/issues/2749>.

## Release 2022-01-20

This release is rolling out to all regions - estimated time for completed roll out is 2022-01-31 for public cloud and 2022-02-03 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout will continue in Feb 2022.
* Client automatic cert rotation is now being enabled on the last set of regions.
* Kubernetes 1.19 will be removed on 2022-01-31.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>
* Starting in Kubernetes 1.23 AKS Metrics server deployment will start having 2 pods instead of 1 for HA, which will increase the memory requests of the system by 54Mb.

### Release Notes

* Preview Features
  * Multi Instance GPU support is available for ND A100 v4 VMs. See <https://aka.ms/AAfjra1> for more details.
* Bug Fixes
  * Fixed bug where some custom in-tree storage classes on 1.21+ were deleted by mistake.
  * Ensured Azure Defender pods have affinity for system pools.
  * App GW ingress controller was added the CriticalAddonsOnly toleration as the rest of the addons and system components.
* Behavioral changes
  * New global policy added to clusters with Calico network policies enabled to allow egress from the konnectivity system component.
  * All AKS system-created tags will have an "aks-managed" prefix and cannot be modified or deleted.
* Component Updates
  * ip-masq-agent updated to v2.5.0.9.
  * Konnectivity updated to v0.0.27.
  * Azure CNI updated to v0.9.1.
  * Azure Policy addon updated to prod_20220114.1.
  * Windows Pause Image updated to 3.6-hotfix.20220114.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.01.19](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.01.19.txt).

## Release 2022-01-13

This release is rolling out to all regions - estimated time for completed roll out is 2022-01-24 for public cloud and 2022-01-27 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout will continue in Feb 2022.
* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* Kubernetes 1.19 will be removed on 2022-01-31.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>

### Release Notes

* Bug Fixes
  * Fixed a bug where if RBAC was disabled on a cluster, the Azure file daemonset would crash on windows nodes.
* Component Updates
  * Upgrade dns-autoscaler to version 1.8.5 for 1.22+.
  * Azure disk CSI driver updated to v.1.10.
  * Azure file CSI driver updated to v.19 on AKS versions 1.21+
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2022.01.08](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.01.08.txt).

## Release 2022-01-07

This release is rolling out to all regions - estimated time for completed roll out is 2022-01-17 for public cloud and 2022-01-20 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout will continue in Feb 2022.
* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* Kubernetes 1.19 will be removed on 2021-01-31.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>

### Release Notes

* Features
  * Private DNS Subzone for Private Cluster is now GA.
  * Containerd runtime on Windows is now GA
* Preview Features
  * Kubenet IPv6 support has been enabled all public cloud regions. See <https://aka.ms/aks/ipv6>  for more details.
* Bug Fixes
  * Corrected validation that silently ignored updates to HTTP proxy settings.
  * Fixed issue that blocked creation of 0 node nodepools.
  * CSI driver probe timeout increased to 30s avoid driver crashes on small Windows VM sizes.
* Behavioral Change
  * Private Cluster now supports cross-subscription VNET for PrivateLink.
  * In 1.21+ existing and newly created clusters, all built-in storage classes will use [CSI Driver provisioners](disk.csi.azure.com) [and](file.csi.azure.com). There are no in-tree provisioners any more(kubernetes.io/azure-disk and kubernetes.io/azure-file).
  * CPU limits for CSI drivers have been removed.
  * Azure CNI - won't reserve VNet IP addresses for daemonset pods using hostNetwork: true"
* Component Updates
  * Cluster Auto Scaler updates:
    * Added support for more SKUs for scaling from zero (including Standard_E2s_v5, Standard_D8s_v5 and Standard_D4s_v5).
    * Fixed an issue with balancing node groups and scaling from zero in clusters with CSI drivers that utilize zonal affinities.
    * Fixed an issue with scaling from zero when pods have a selector on the stable instance type label node.kubernetes.io/instance-type.
    * Improve scale up performance in very large scale-up scenarios
  * Azure Policy for AKS updated to [Gatekeeper 3.7.0](https://github.com/open-policy-agent/gatekeeper/releases/tag/v3.7.0)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.01.07](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2022.01.07.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2366.211215](vhd-notes/AKSWindows/2019/17763.2366.211215.txt).

## Release 2021-12-9

This release is rolling out to all regions - estimated time for completed roll out is 2021-12-20 for public cloud and 2021-12-23 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout has been halted for the rest of the year. We will continue the rollout in the new calendar year.
* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* Kubernetes 1.19 will be removed on 2021-01-31.
* Starting with 1.23 AKS will follow upstream kubernetes and deprecate in-tree azure authentication which is marked for deprecation to be replaced with 'exec'. If you are using Azure CLI or Azure clients, AKS will download kubelogin for users automatically. If outside of Azure CLI, users need to download and install kubelogin in order to continue to use kubectl with AAD authentication. <https://github.com/Azure/kubelogin>

### Release Notes

* Features
  * Kubernetes 1.22 is now GA.
  * New Kubernetes patch versions released, 1.20.13, 1.21.7, 1.22.4.
* Preview Features
  * AKS GitOps agent extension is now in Public Preview.
  * Microsoft Defender for containers is now in Public Preview.
* Bug Fixes
  * Corrected validation that silently ignored updates to HTTP proxy settings.
  * Fixed issue that blocked creation of 0 node nodepools.
  * CSI driver probe timeout increased to 30s avoid driver crashes on small Windows VM sizes.
* Component Updates
  * Calico updated to [v3.21.0](https://projectcalico.docs.tigera.io/archive/v3.21/release-notes/#v3210) on Linux.
  * Updated Azure CNI on Windows to [v1.4.16](https://github.com/Azure/azure-container-networking/releases/tag/v1.4.16). Fixes #2608
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.12.07](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.12.07.txt)

## Release 2021-12-2

This release is rolling out to all regions - estimated time for completed roll out is 2021-12-13 for public cloud and 2021-12-16 for sovereign clouds.

### Announcement

* From Kubernetes 1.23, containerD will be the default container runtime for Windows node pools. Docker support will be deprecated in Kubernetes 1.24. You are advised to test your workloads before Docker deprecation happens by following the documentation here <https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview>.
* Konnectivity rollout has been halted for the rest of the year. We will continue the rollout in the new calendar year.
* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* AKS and Holiday Season: To ease the burden of upgrade and change during the holiday season, AKS is extending a limited scope of support for all clusters and node pools on 1.19 as a courtesy. Customers with clusters and node pools on 1.19 after the [announced deprecation date of 2021-11-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy).
 The scope of this limited extension is effective from '2021-12-01 to 2022-01-31' and is limited to the following:
  * Creation of new clusters and node pools on 1.19.
  * CRUD operations on 1.19 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.

### Release Notes

* Bug Fixes
  * Fixed a bug such that the nodes on 1.21 won't be able to start with the DelegateFSGroupToCSIDriver feature flag. This feature flag is only introduced to kubelet in 1.22.
  * A WindowsGmsaProfile certificate renewal issue during certificate rotation has been identified and fixed.
  * Added the component=tunnel label to konnectivity-agent pods so they will be matched by any label selectors that previously matched tunnelfront pods. This only applies to clusters that have received the new Konnectivity network tunnel.
* Behavioral Changes
  * Increased cpu limits of csi driver node daemonsets from 200m to 1cpu in order to prevent cpu throttling.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.11.27](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.11.27.txt) - please refer to the link for package versions in this VHD.

## Release 2021-11-18

### Announcement

* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* Konnectivity - a new version of the AKS tunnel component that will replace the aks-link and tunnel-front versions has been rolled back in all regions. The AKS team will announce when Konnectivity is re-released.
* AKS and Holiday Season: To ease the burden of upgrade and change during the holiday season, AKS is extending a limited scope of support for all clusters and node pools on 1.19 as a courtesy. Customers with clusters and node pools on 1.19 after the [announced deprecation date of 2021-11-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy).
 The scope of this limited extension is effective from '2021-12-01 to 2022-01-31' and is limited to the following:
  * Creation of new clusters and node pools on 1.19.
  * CRUD operations on 1.19 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.

### Release Notes

* Bug Fixes
  * A bug has been fixed where clusters would come up with incorrect SNAT settings that broke DNS resolution. The following GitHub issues describe the bug, [Pods cannot resolve external DNSes](https://github.com/Azure/AKS/issues/2646) and [azure-ip-masq-agent-config populated with empty nonMasqueradeCIDRs list for private cluster](https://github.com/Azure/AKS/issues/2653).

## Release 2021-11-11

This release is rolling out to all regions - estimated time for completed roll out is 2021-11-18 for public cloud and 2021-11-25 for sovereign clouds.

### Announcement

* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* Konnectivity - a new version of the AKS tunnel component that will replace the aks-link and tunnel-front versions has been rolled back in all regions. The AKS team will announce when Konnectivity is re-released.
* AKS and Holiday Season: To ease the burden of upgrade and change during the holiday season, AKS is extending a limited scope of support for all clusters and node pools on 1.19 as a courtesy. Customers with clusters and node pools on 1.19 after the [announced deprecation date of 2021-11-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy).
 The scope of this limited extension is effective from '2021-12-01 to 2022-01-31' and is limited to the following:
  * Creation of new clusters and node pools on 1.19.
  * CRUD operations on 1.19 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.

### Release Notes

* New Features
  * Auto-Upgrade for AKS is now [GA](https://docs.microsoft.com/azure/aks/upgrade-cluster#set-auto-upgrade-channel).
* Bug Fixes
  * An Authentication issue related to [pulling image secrets](https://github.com/virtual-kubelet/azure-aci/pull/171) has been fixed with a new version of the virtual-kubelet.
* Component Updates
  * Virtual-kubelet has been updated to version 1.4.1.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.11.06](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.11.06.txt) - please refer to the link for package versions in this VHD.
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2300.211110](vhd-notes/AKSWindows/2019/17763.2300.211110.txt) - please refer to the link for component versions in this VHD.

## Release 2021-11-04

This release is rolling out to all regions - estimated time for completed roll out is 2021-11-11 for public cloud and 2021-11-18 for sovereign clouds.

### Announcement

* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* Konnectivity - a new version of the AKS tunnel component will replace the aks-link and tunnel-front versions slowly over the rest of the calendar year. The following regions eastus, westcentralus, uksouth, uaenorth already have Konnectivity enabled.

### Release Notes

* Preview Features
  * Managed NAT Gateway is now in [public preview](https://docs.microsoft.com/azure/aks/nat-gateway).
  * Group Managed Service Accounts (GMSA) for your Windows Server nodes is now in [public preview](https://docs.microsoft.com/azure/aks/use-group-managed-service-accounts).
* Bug Fixes
  * A bug has been fixed in `Application Gateway Ingress Controller` that previously caused users OOM errors while running a large number of ingress objects.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.10.30](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.10.30.txt) - please refer to the link for package versions in this VHD.

## Release 2021-10-28

This release is rolling out to all regions - estimated time for completed roll out is 2021-11-04 for public cloud and 2021-11-11 for sovereign clouds.

### Announcement

* AKS is implementing auto-cert rotation slowly over the next few months. We have already enabled the following regions westcentralus, uksouth, eastus, australiacentral, and australiaest. If you have clusters in those regions please run a cluster upgrade in order to have that cluster configured for auto-cert rotation. The following regions brazilsouth, canadacentral, centralindia, and eastasia will be released in January after the holidays as the next group of regions. We will update the release notes will the upcoming schedule going forward until all regions are deployed.
* Konnectivity - a new version of the AKS tunnel component will replace the aks-link and tunnel-front versions slowly over the rest of the calendar year. The following regions westcentralus, uksouth, uaenorth already have Konnectivity enabled.

### Release Notes

* Preview Features
  * Node pool start/stop is now in [preview](https://docs.microsoft.com/azure/aks/start-stop-nodepools).
  * Node pool snapshots are now supported. Please check the AKS documentation at 5pm Pacific on 11/02/2021 to read more.
* Bug Fixes
  * added missing `managed-csi` storage class in AKS Kubernetes versions 1.21+.
  * Fixed a bug with the cluster autoscaler nodepool balancing due to a new agentpool label being added "kubernetes.azure.com/agentpool"
  * User manipulation or usage of the system-reserved label prefix "kubernetes.azure.com" is now correctly blocked.
* Component Updates
  * Updated CSI Disk Driver to v1.8. and File Driver to 1.7.
  * Updated omsagent to [ciprod10132021 and win-ciprod10132021](https://github.com/microsoft/Docker-Provider/blob/ci_dev/ReleaseNotes.md#10132021--).
  * Updated Azure CNI to v1.4.13 for Windows.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.10.23](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.10.23.txt) - please refer to the link for package versions in this VHD.

## Release 2021-10-15

This release is rolling out to all regions - estimated time for completed roll out is 2021-10-21 for public cloud and 2021-10-28 for sovereign clouds.

### Release Notes

* Behavioral Changes
  * Add aks-managed-cluster-rg and aks-managed-cluster-name tags to the node resource group
* Bug Fixes
  * Fix [issue](https://github.com/Azure/AKS/issues/2584) where Terraform is unable to set a default for the auto upgrade channel preview feature
* Component Updates
  * Update Virtual Kubelet to 1.4.0
  * Use 1.5.0-rc1 of the AGIC Addon for k8s 1.22.0 to support ingress v1 API
  * Update Azure CNI to v1.4.12 for Windows
  * Update AKS base image version for Edge zones to 2021.10.13
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.10.13](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.10.13.txt) - please refer to the link for package versions in this VHD.
  * AKS Windows image has been updated to the 10B patch version including KB5004335, KB5004424, KB5006672 & KB5005701 [2019-datacenter-core-smalldisk-17763.2237.211014](vhd-notes/AKSWindows/2019/17763.2237.211014.txt) - please refer to the link for component versions in this VHD.

## Release 2021-10-08

This release is rolling out to all regions - estimated time for completed roll out is 2021-10-14 for public cloud and 2021-10-21 for sovereign clouds.

### Release Notes

* Features
  * General Availability of [Ultra SSD support](https://docs.microsoft.com/azure/aks/use-ultra-disks)
* Preview Features
  * Public Preview of Private DNS sub zone support for Private Clusters
  * Public Preview of [HTTP Proxy](https://docs.microsoft.com/azure/aks/http-proxy)
  * Public Preview of support for [WASM/WASI based nodepools](https://docs.microsoft.com/azure/aks/use-wasi-node-pools)
* Behavioral Changes
  * Validation that DNS service IP is not on subnet boundary
  * Improve system pool taints error messages
  * Don't provision network monitor on any clusters >= 1.21 as Azure CNI moved to transparent mode
* Bug Fixes
  * Fix issue where images in China region were pulled from public cloud MCR
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.10.02](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.10.02.txt) - please refer to the link for package versions in this VHD.

## Release 2021-09-30

This release is rolling out to all regions - estimated time for completed roll out is 2021-10-07 for public cloud and 2021-10-14 for sovereign clouds.

### Release Notes

* Features
  * [Ability to tune azuredisk performance parameters](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/docs/enhancements/feat-add-ability-to-tune-azuredisk-performance-parameters.md)
* Preview Features
  * Kubernetes 1.22.1 is now in preview
  * Enable multiple service account issuers for clusters using version >= 1.22, also fixing CNCF Validation issues.
  * Cloud Controller Manager is now default for clusters 1.22+
* Behavioral Changes
  * When turning Cluster Auto Scaler off, you can now specify the requested agent pool node number.
* Bug Fixes
  * Fixed csi driver crash issue on Windows nodes.
* Component Updates
  * Containerd 1.5 is now available to clusters 1.22+, for clusters prior to 1.22 AKS will continue to use and patch containerd 1.4.
  * New patches for containerd released, 1.5.5 and 1.4.9, which address [CVE-2021-41103](https://github.com/Azure/AKS/issues/2583)
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.09.25](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.09.25.txt) - please refer to the link for package versions in this VHD.
  * AKS Windows image has been updated to the 9C patch version [2019-datacenter-core-smalldisk-17763.2213.210922](vhd-notes/AKSWindows/2019/17763.2213.210922.txt) - please refer to the link for component versions in this VHD.

## Release 2021-09-16

This release is rolling out to all regions - estimated time for completed roll out is 2021-09-23 for public cloud and 2021-09-30 for sovereign clouds.

### Announcement

* In order to preserve any deallocated VMs, you must to set Scale-down Mode to `Deallocate`. That includes VMs that have been deallocated using IaaS APIs (Virtual Machine Scale Set APIs). Setting Scale-down Mode to `Delete` will remove any deallocated VMs.

### Release Notes

* New Features
  * You can now [integrate Azure HPC Cache with Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/azure-hpc-cache).
* Preview Features
  * Cloud Controller Manager is now in Public Preview in anticipation of moving [Azure specific controllers out of tree](https://docs.microsoft.com/azure/aks/out-of-tree).
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.09.19](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.09.19.txt) - please refer to the link for package versions in this VHD.
  * Azure Disk CSI driver has been updated to v1.7.0.
  * Azure File CSI driver has been updated to v1.6.0.

## Release 2021-09-09

This release is rolling out to all regions - estimated time for completed roll out is 2021-09-16 for public cloud and 2021-09-25 for sovereign clouds.

### Announcement

* In order to preserve any deallocated VMs, you must to set Scale-down Mode to Deallocate. That includes VMs that have been deallocated using IaaS APIs (Virtual Machine Scale Set APIs). Setting Scale-down Mode to Delete will remove any deallocate VMs.

### Release Notes

* New Features
  * AKS Run Command is now Generally Available (GA), estimated to roll out the week of 2021-09-13.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.09.06](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.09.06.txt) - please refer to the link for package versions in this VHD.
* Behavioral Change
  * Azure Confidential Compute has changed the CPU resource request and limits for the device plugin and quote helper daemonset as part of the ACC addon deployments. They are now reduced as the earlier requested amounts were not necessary.
  * AKS Run Command will now be available by default, and customers can now disable when desired through the cli.

## Release 2021-09-02

This release is rolling out to all regions - estimated time for completed roll out is 2021-09-09 for public cloud and 2021-09-18 for sovereign clouds.

### Preannouncement

* AKS will be upgrading to CoreDNS v1.8.4 in September. Users who are using the rewrite plugin, should [upgrade their configuration](https://github.com/Azure/AKS/issues/2521) before 1.8.4 goes live.

### Announcement

* In order to preserve any deallocated VMs, you must to set Scale-down Mode to Deallocate. That includes VMs that have been deallocated using IaaS APIs (Virtual Machine Scale Set APIs). Setting Scale-down Mode to Delete will remove any deallocate VMs.

### Release Notes

* Features
  * Scale-down mode is now in public preview [https://docs.microsoft.com/azure/aks/scale-down-mode](https://docs.microsoft.com/azure/aks/scale-down-mode)

* Component Updates
  * Update Windows Azure CNI version to v1.4.9.  - Azure CNI start time shortened by 500ms
  * csi-snapshotter has been updated to v4.2.1 for Kubernetes 1.21
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.08.31](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.08.31.txt) - please refer to the link for package versions in this VHD.

* Bug Fixes
  * NPM updated to 1.4.9, fixing <https://github.com/Azure/azure-container-networking/issues/851>

## Release 2021-08-26

This release is rolling out to all regions - estimated time for completed roll out is 2021-09-02 for public cloud and 2021-09-09 for sovereign clouds.

### Preannouncement

* AKS will be upgrading to CoreDNS v1.8.4 in September. Users who are using the rewrite plugin, should [upgrade their configuration](https://github.com/Azure/AKS/issues/2521) before 1.8.4 goes live.

### Release Notes

* Component Updates
  * Azuredisk and Azurefile CSI drivers upgraded to v1.5.0 in 1.21.0+ clusters.
  * Open Service Mesh (OSM) addon has been updated to v0.9.2.
  * Calico has been updated to v3.20.0 on linux.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.08.21](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.08.21.txt) - please refer to the link for package versions in this VHD.
* Bug Fixes
  * In scenarios where users are creating multiple node pools simultaneously, it is now possible to have the first User node pool in the deployment to have any characteristics so long as there is at least one System pool present in the deployment.

## Release 2021-08-19

This release is rolling out to all regions - estimated time for completed roll out is 2021-08-26 for public cloud and 2021-09-02 for sovereign clouds.

### Announcements

* On the next release of the az AKS CLI we will introduce a new subcommand "aks addons" which will have the following commands: disable, enable, list, list-available, show, update.

### Release Notes

* Component Updates
  * Kubernetes patch versions: 1.19.13 and 1.20.9 have been onboarded. Versions 1.19.9 and 1.20.5 have been deprecated.
  * Bump Windows containerd to v0.0.42
  * Bump CoreDNS  to 1.8.4 for Kubernetes versions above 1.20.0
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.08.14](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.08.14.txt) - please refer to the link for package versions in this VHD.
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2114.210811](vhd-notes/AKSWindows/2019/17763.2114.210811.txt) - please refer to the link for component versions in this VHD.

* Behavioral Changes
  * Stop ability to make changes to the following system labels:
    * beta.kubernetes.io/arch
    * beta.kubernetes.io/instance-type
    * beta.kubernetes.io/os
    * failure-domain.beta.kubernetes.io/region
    * failure-domain.beta.kubernetes.io/zone
    * failure-domain.kubernetes.io/zone
    * failure-domain.kubernetes.io/region
    * kubernetes.io/arch
    * kubernetes.io/hostname
    * kubernetes.io/os
    * kubernetes.io/role
    * kubernetes.io/instance-type
    * node.kubernetes.io/instance-type
    * topology.kubernetes.io/region
    * topology.kubernetes.io/zone
    * kubernetes.azure.com/role=agent
    * node-role.kubernetes.io/agent
    * kubernetes.io/role=agent
    * agentpool
    * storageprofile
    * storagetier
    * accelerator
    * kubernetes.azure.com/fips_enabled
    * kubernetes.azure.com/os-sku
    * kubernetes.azure.com/cluster

## Release 2021-08-12

This release is rolling out to all regions - estimated time for completed roll out is 2021-08-19 for public cloud and 2021-08-24 for sovereign clouds.

### Release Notes

* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.08.07](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.08.07.txt).

## Release 2021-08-05

This release is rolling out to all regions - estimated time for completed roll out is 2021-08-12 for public cloud and 2021-08-17 for sovereign clouds.

### Release Notes

* Behavioral Changes
  * All regions now use Azure Policy V2 by default.
  * TLS 1.2 is now enabled for in AKS Windows nodes. TLS1.1, TLS1.0, SSL3.0, SSL2.0 are now disabled.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.07.31](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.07.31.txt).

## Release 2021-07-29

This release is rolling out to all regions - estimated time for completed roll out is 2021-08-05 for public cloud and 2021-08-10 for sovereign clouds.

### Announcements

* Azure Kubernetes Service (AKS) will stop publishing Ubuntu 16.04 image change moving forward.
* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Bug Fixes
  * Added missing tolerations to Pod Identity Pods. Closes #2146.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.07.25](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.07.25.txt).

## Release 2021-07-22

This release is rolling out to all regions - estimated time for completed roll out is 2021-07-29 for public cloud and 2021-08-03 for sovereign clouds.

### Announcements

* Azure Kubernetes Service (AKS) will stop publishing Ubuntu 16.04 image change moving forward.
* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Features
  * New Kubernetes patch version available, v.1.21.2.
* Preview Features
  * Upgrade your Windows nodepool from Docker to Containerd by the following two methods. Note that the Kubernetes version should be > 1.20.0 and the feature flag UseCustomizedWindowsContainerRuntime should be registered under your current Azure subscription. Reminder that Docker is removed in Kubernetes 1.24 completely, use the following commands to move your workload from Docker to Containerd now.
    * To upgrade a specific nodepool to use containerd: `az aks nodepool upgrade --cluster-name $CLUSTERNAME --name $NODEPOOLNAME --resource-group $RGNAME --kubernetes-version 1.21.1 --aks-custom-headers WindowsContainerRuntime=containerd`
    * To upgrade the cluster to use containerd for all Windows nodepools: `az aks upgrade --cluster-name $CLUSTERNAME --resource-group $RGNAME --kubernetes-version 1.21.1 --aks-custom-headers WindowsContainerRuntime=containerd`
* Behavioral Changes
  * Cluster autoscaler will now enforce the minimum count in cases where the actual count drops below that. For example, Spot eviction or changing the minimum count value from the AKS API. In the past, the autoscaler operated and respected the minimum count but never interfered to enforce it if external factors affect it.
* Component Updates
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.07.17](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.07.17.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.2061.210714](vhd-notes/AKSWindows/2019/17763.2061.210714.txt).
  
## Release 2021-07-15

This release is rolling out to all regions - estimated time for completed roll out is 2021-07-27 for public cloud and 2021-07-31 for sovereign clouds.

### Announcements

* Azure Kubernetes Service (AKS) will stop publishing Ubuntu 16.04 image change moving forward.
* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* New Features
  * Kubernetes 1.21 is now Generally Available (GA), estimated to roll out the week of 2021-07-19.
  * Container Storage Interface (CSI) drivers for Azure disks and Azure files on Azure Kubernetes Service (AKS) is now Generally Available (GA) in Kubernetes version 1.21+. Azure Disk CSI migration is turned on for 1.21.0+ clusters.
* Bug Fixes
  * Fix external-dns 0.8.0 for HTTP application routing addon for 1.21+ clusters.
* Behavioral Changes
  * Azure Kubernetes Service (AKS) will now rotate your intermediate certificates during an upgrade operation
* Preview Features
  * Windows containerd support on AKS is now available in all sovereign clouds.
* Component Updates
  * Azuredisk and Azurefile CSI drivers upgraded to v1.4.0 in 1.20.0+ clusters.
  * Windows image update for omsagent for Windows mdm by setting the NODE_IP environment variable for 'machine' as required by Windows in non-sidecar enabled mode.
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.07.10](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.07.10.txt).

## Release 2021-07-08

This release is rolling out to all regions - estimated time for completed roll out is 2021-07-15 for public cloud and 2021-07-21 for sovereign clouds.

### Announcements

* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* New Features
  * Bring your own Managed Identity is now GA. Allowing you to bring your own [control plane MI](https://docs.microsoft.com/azure/aks/use-managed-identity#bring-your-own-control-plane-mi) and [Kubelet MI](https://docs.microsoft.com/azure/aks/use-managed-identity#bring-your-own-kubelet-mi).
* Preview Features
  * Public DNS for private clusters is now in preview. Read more [here](https://docs.microsoft.com/azure/aks/private-clusters#create-a-private-aks-cluster-with-a-custom-private-dns-zone).
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.07.03](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.07.03.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.07.03](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.07.03.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1999.210609](vhd-notes/AKSWindows/2019/17763.1999.210609.txt).

## Release 2021-07-03

This release is rolling out to all regions - estimated time for completed roll out is 2021-07-08 for public cloud and 2021-07-14 for sovereign clouds.

### Announcements

* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Bug Fixes
  * Resolved "TO/FROM rule and port rule on same PodSelector in multiple policies", <https://github.com/Azure/azure-container-networking/issues/870>
* Component Updates
  * Block enabling autoupgrade for unsupported k8s versions (less than lowest minor version by one)
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.06.19](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.06.19.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.06.19](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.06.19.txt).

## Release 2021-06-17

This release is rolling out to all regions - estimated time for completed roll out is 2021-06-24 for public cloud and 2021-06-28 for sovereign clouds.

### Announcements

* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Component Updates
  * Omsagent updated to 06112021. Read more [here](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#06112021--).
  * Windows Azure CNI updated to version 1.4.0.
  * HTTP Application Routing addon has been updated to support Kubernetes version >= 1.21.
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.06.12](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.06.12.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.06.12](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.06.12.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1999.210609](vhd-notes/AKSWindows/2019/17763.1999.210609.txt).

## Release 2021-06-10

This release is rolling out to all regions - ETA for conclusion 2021-06-17 for public cloud and 2021-06-21 for sovereign clouds.

### Announcements

* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Preview Features
  * Public DNS support for Private Clusters using the Private cluster endpoint.
* Bug Fixes
  * Released runc r95 to address a [vulnerability](https://github.com/Azure/AKS/issues/2375) to symlink-exchange attack.
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.06.09](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.06.09.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.06.09](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.06.09.txt).
  
## Release 2021-06-03

This release is rolling out to all regions - ETA for conclusion 2021-06-10 for public cloud and 2021-06-14 for sovereign clouds.

### Announcements

* As a response to customer feedback and issues with previous Kubernetes version patches that left a lot of users with hard options. The AKS Team is extending a limited scope of support for all clusters and nodepools on 1.18 as a courtesy. Customers with clusters and nodepools on 1.18 [after the announced deprecation date of 2021-06-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) .The scope of this limited extension is effective from '2021-06-30 to 2021-07-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.18.
  * CRUD operations on 1.18 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Preview Features
  * Windows containerd support on AKS is now available in all regions. Read more [here](https://docs.microsoft.com/azure/aks/windows-container-cli#add-a-windows-server-node-pool-with-containerd-preview).
* Bug Fixes
  * Fix priority expander in cluster autoscaler falling back to a random choice when a higher priority exists. To read more about this bug, click [here](https://github.com/Azure/AKS/issues/2359).
  * Fix a regression where users with > 200 group memberships may fail to authenticate to AAD enabled AKS clusters in Azure public cloud.
* Component Updates
  * Updated omsagent to [ciprod05202021](https://github.com/microsoft/Docker-Provider/blob/afdf5d21dceac7d27ac56f29d86f2ccf9714fa49/ReleaseNotes.md).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.06.02](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.06.02.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.06.02](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.06.02.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1935.210513](vhd-notes/AKSWindows/2019/17763.1935.210513.txt)

## Release 2021-05-20

This release is rolling out to all regions - ETA for conclusion 2021-06-03 for public cloud and 2021-06-07 for sovereign clouds.

### Announcements

* Previous pod security policy (preview)](<https://docs.microsoft.com/azure/aks/use-pod-security-policies>) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Features
  * Use Set-TimeZone now with Windows Containers to change timezones.
  * New Kubernetes patch versions available, v1.18.19, 1.19.11, v1.20.7.
  * Encryption at Host is now GA
* Preview Features
  * Kubernetes 1.21.1
  * Disable local accounts in now in preview [here](https://docs.microsoft.com/azure/aks/managed-aad#disable-local-accounts-preview).
  * Windows containerd support on AKS are available in 3 regions (eastus, uksouth, and westcentralus) today. If you registered the containerd public preview feature flag and add node pool on a cluster below k8s 1.20 version in other regions than mentioned above, the windows nodepool creation will fail. If you are using k8s version 1.20 and register the containerd feature flag in the available regions, this will only add containerd node pool instead of docker. You can unregister the feature flag to use docker node pool. Please note that we are working towards releasing the fix in other regions in few days.
* Bug Fixes
  * Reverting Container Insights agent to March release [ciprod03262021] in response to failing liveness probes.
* Component Updates
  * Upgraded calico to v3.19. The newest Calico update includes this fix for customers that were experiencing upgrade problems.
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.05.19](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.05.19txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.05.19](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.05.19.txt).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1911.210513](vhd-notes/AKSWindows/2019/17763.1911.210513.txt).

## Release 2021-05-13

This release is rolling out to all regions - ETA for conclusion 2021-05-20 for public cloud and 2021-05-24 for sovereign clouds.

### Announcements

* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Preview Features
  * The CSI Secret Store AKS Addon is now in Public Preview. See more [here](https://docs.microsoft.com/azure/aks/csi-secrets-store-driver).
* Component Updates
  * Upgrade azuredisk/azurefile CSI Driver to v1.2.0 (currently in preview).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.05.08](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.05.08.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.05.08](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.05.08.txt).

## Release 2021-05-06

This release is rolling out to all regions - ETA for conclusion 2021-05-13 for public cloud and 2021-05-17 for sovereign clouds.

### Announcements

* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Preview Features
  * FIPS compliant nodes
* Bug Fixes
  * Fix a bug that different users could not reset service principal using same Azure Active Directory Client ID.
* Component Updates
  * AGIC has been updated to 1.4.0. Read more [here](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/master/CHANGELOG/CHANGELOG-1.4.md).
  * Azure NPM has been updated to 1.3.2
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.05.01](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.05.01.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.05.01](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.05.01.txt).

## Release 2021-04-29

This release is rolling out to all regions - ETA for conclusion 2021-05-03 for public cloud and 2021-05-10 for sovereign clouds.

### Announcements

* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.

### Release Notes

* Preview Features
  * Autoupgrade will now respect customer's default maintenance configuration settings.
* Bug Fixes
  * Customers trying to use the `RunCommand` on clusters with both PrivateLink and AAD enabled will now see a `NotSupportedSetup` message.
* Component Updates
  * Azure Monitor for Containers image tag has been updated to ciprod04222021. Read more [here](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1911.210423](vhd-notes/AKSWindows/2019/17763.1911.210423.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.04.27](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.04.27.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.04.27](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.04.27.txt).

## Release 2021-04-22

This release is rolling out to all regions - ETA for conclusion 2021-04-26 for public cloud and 2021-05-03 for sovereign clouds.

### Announcements

* Kubernetes version 1.17 has now been deprecated since March 31st.
* CSI Drivers will become default for [Kubernetes versions 1.21+](https://docs.microsoft.com/azure/aks/csi-storage-drivers).
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.
* For all AKS clusters using Kubernetes v1.20+, CoreDNS will be upgraded to version 1.8.3. This will remove `resyncperiod` and `upstream`from the Kubernetes plugin.

### Release Notes

* New Features
  * You can now update Windows passwords via Azure cli.
* Bug Fixes
  * Fixed a bug with Cert Rotation trying to call windows agentpools, this is a linux only function.
  * Fixed a bug that if a customer uses "[]" as "AvailabilityZones" for both create and update, their update will be blocked incorrectly.
* Behavioral Changes
  * Node pool limit has increased from 10 to 100.
* Component Updates
  * Linux Pause container image has been updated to [3.5] from 1.3.1
  * Dns-autoscaler image has been updated to [mcr.microsoft.com/oss/kubernetes/autoscaler/cluster-proportional-autoscaler:1.8.3]  for 1.18 and above cluster.  1.8.3 uses non-root user.
  * Pod Identity nmi image has been updated to [1.7.5] and set critical addon torelations.
  * OSM has been updated to [v0.8.3]
  * The OSM Envoy image has been updated to [1.17.2]
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1879.210414](vhd-notes/AKSWindows/2019/17763.1879.210414.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.04.20](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.04.20.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.04.20](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.04.20.txt).

## Release 2021-04-05

This release is rolling out to all regions - ETA for conclusion 2021-04-14 for public cloud.

### Announcements

* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Kubernetes version 1.17 has now been deprecated since March 31st.
* Before k8s 1.20 a bug would allow exec probes to run indefinitely, ignoring any timeoutSeconds configuration value. The previous buggy behavior has been fixed, and timeouts are now enforced. Additionally, this change introduces a new default timeout of 1 second. Please audit all your existing exec probes to make sure that it is appropriate to enforce a 1 second timeout. If not, please provide an explicit timeoutSeconds value that is appropriate for each [exec probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes).
* CSI Drivers will become default for [Kubernetes versions 1.21+](https://docs.microsoft.com/azure/aks/csi-storage-drivers).
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.
* For all AKS clusters using Kubernetes v1.20+, CoreDNS will be upgraded to version 1.8.3. This will remove `resyncperiod` and `upstream`from the Kubernetes plugin.

### Release Notes

* Bug Fixes
  * Fixed a bug in runc that caused pods to be stuck in container creation in containerd 1.4.3 and 1.4.4.
  * Fixed a bug in VMAS that accidentally enabled VMAS to be scaled down to 0.
* Behavioral Changes
  * Increased nslookup/nc timeout to 10s for Provisioning CSE in nodes.
* Component Updates
  * Removed Cross-namespace owner references in Azure Policy on AKS v1.20+.
  * Updated omsagent to [ciprod03262021](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#03262021--).
  * Updated Azure Confidential Compute Image to 1.16 with updated webhook and plugin version, to include a liveness probe.
  * Calico will upgrade to 3.18.1 to correct the policy for Tigera operator which requires hostPath. For the base Calico on linux, we will automatically upgrade cluster with Calico 3.17.2. For the Windows node pools, calico will be upgraded to v3.18.1 in any agent pool update/upgrade operations, for example, upgrade the cluster, update the node image, or upgrade the node pool. For detailed updates on Calico, please read more [here](https://docs.projectcalico.org/archive/v3.18/release-notes/).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1817.210330](vhd-notes/AKSWindows/2019/17763.1817.210330.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.03.31](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.03.31.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.03.31](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.03.31.txt).

## Release 2021-03-29

This release is rolling out to all regions - ETA for conclusion 2021-04-07 for public cloud.

### Announcements

* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Kubernetes version 1.17 has now been deprecated since March 31st.
* Before k8s 1.20 a bug would allow exec probes to run indefinitely, ignoring any timeoutSeconds configuration value. The previous buggy behavior has been fixed, and timeouts are now enforced. Additionally, this change introduces a new default timeout of 1 second. Please audit all your existing exec probes to make sure that it is appropriate to enforce a 1 second timeout. If not, please provide an explicit timeoutSeconds value that is appropriate for each [exec probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes).
* CSI Drivers will become default for [Kubernetes versions 1.21+](https://docs.microsoft.com/azure/aks/csi-storage-drivers).
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.
* For all AKS clusters using Kubernetes v1.20+, CoreDNS will be upgraded to version 1.8.3. This will remove `resyncperiod` and `upstream`from the Kubernetes plugin.

### Release Notes

* New Features
  * `brazilsouth`, `centralindia`, `eastasia` and `francecentral` are all new supported regions for Virtual Node. The `southindia` region has been removed from the supported region list.
* Preview Features
  * Open Service Mesh (OSM), as a managed AKS add-on, is now in public preview.
* Component Updates
  * Calico will upgrade to 3.18.1 to correct the policy for Tigera operator which requires hostPath. For the base Calico on linux, we will automatically upgrade cluster with Calico 3.17.2. For the Windows node pools, calico will be upgraded to v3.18.1 in any agent pool update/upgrade operations, for example, upgrade the cluster, update the node image, or upgrade the node pool. For detailed updates on Calico, please read more [here](https://docs.projectcalico.org/archive/v3.18/release-notes/).

## Release 2021-03-22

This release is rolling out to all regions - ETA for conclusion 2021-03-31 for public cloud.

### Announcements

* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Next week, Kubernetes version 1.17 will be deprecated on March 31st.
* Before k8s 1.20 a bug would allow exec probes to run indefinitely, ignoring any timeoutSeconds configuration value. The previous buggy behavior has been fixed, and timeouts are now enforced. Additionally, this change introduces a new default timeout of 1 second. Please audit all your existing exec probes to make sure that it is appropriate to enforce a 1 second timeout. If not, please provide an explicit timeoutSeconds value that is appropriate for each [exec probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes).
* CSI Drivers will become default for [Kubernetes versions 1.21+](https://docs.microsoft.com/azure/aks/csi-storage-drivers).
* Previous [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) deprecation was June 30th 2021. To better align with Kubernetes Upstream pod security policy (preview) deprecation will begin with Kubernetes version 1.21, with its removal in version 1.25. As Kubernetes Upstream approaches that milestone, the Kubernetes community will be working to document viable alternatives.
* For all AKS clusters using Kubernetes v1.20+, CoreDNS will be upgraded to version 1.8.3. This will remove `resyncperiod` and `upstream`from the Kubernetes plugin.

### Release Notes

* Bug Fixes
  * Fixed an issue regarding indecisiveness in Kubernetes versions and the auto-upgrade feature in ARM templates. Read more [here](https://github.com/Azure/AKS/issues/2138).
* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1817.210310](vhd-notes/AKSWindows/2019/17763.1817.210310.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.03.17](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.03.17.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.03.17](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.03.17.txt).

## Release 2021-03-15

This release is rolling out to all regions - ETA for conclusion 2021-03-24 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on June 30th, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Kubernetes version 1.17 will be deprecated in the last week of March 2021.
* Before k8s 1.20 a bug would allow exec probes to run indefinitely, ignoring any timeoutSeconds configuration value. The previous buggy behavior has been fixed, and timeouts are now enforced. Additionally, this change introduces a new default timeout of 1 second. Please audit all your existing exec probes to make sure that it is appropriate to enforce a 1 second timeout. If not, please provide an explicit timeoutSeconds value that is appropriate for each [exec probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes).
* CSI Drivers will become default for [Kubernetes versions 1.21+](https://docs.microsoft.com/azure/aks/csi-storage-drivers).

### Release Notes

* Bug Fixes
  * Fixed an issue with using a managed identity created in a different subscription from the cluster while using pod identity [github](https://github.com/Azure/aad-pod-identity/issues/1003#issuecomment-802574090).
* Behavioral Changes
  * Made improvements to Cluster AutoScaler for ignoring pods that are stuck in Terminating state to be considered for scale down after exhausting their grace period.
  * WinDSR is enabled by default for [Kubernetes versions 1.20+]
* Component Updates
  * Updated image tunnel-front to v1.9.2-v3.0.22
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1817.210310](vhd-notes/AKSWindows/2019/17763.1817.210310.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.03.10](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.03.10.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.03.10](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.03.10.txt).

## Release 2021-03-08

This release is rolling out to all regions - ETA for conclusion 2021-03-17 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on June 30th, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Kubernetes version 1.17 will be deprecated in the last week of March 2021.
* Before k8s 1.20 a bug would allow exec probes to run indefinitely, ignoring any timeoutSeconds configuration value. The previous buggy behavior has been fixed, and timeouts are now enforced. Additionally, this change introduces a new default timeout of 1 second. Please audit all your existing exec probes to make sure that it is appropriate to enforce a 1 second timeout. If not, please provide an explicit timeoutSeconds value that is appropriate for each [exec probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes).

### Release Notes

* Features
  * Azure monitor for containers now supports Pods & Replica set live logs in AKS resource view. Read more [here](https://azure.microsoft.com/updates/azmon-livelogs-pods/)
  * Confidential computing addon for confidential computing nodes (DCSv2) on AKS is updated to align with Intel SGX's future initiatives.
* Bug Fixes
  * The latest Windows image fixes a bug where Windows could break nodes at the CNI level and cause all pods scheduled on that node to be permanently stuck, or blocked during deployment. If you have questions about this fix, please contact the [Windows Container Team](https://github.com/microsoft/Windows-Containers).
  * Fixed an issue where duplicate packets were sent for kubenet on clusters with k8s 1.19+ and containerd-based clusters. This was cased when the traffic is sent to another pod on the same node over cluster service IP."
  * Fixed bug in the addon profile API that caused crashes on build using Terraform in [sov clouds](https://github.com/terraform-providers/terraform-provider-azurerm/issues/6462).
* Behavioral Change
  * The maximum number of managed identities for the Pod Identity addon was increased from 50 to 200.
  * Systemd-resolved will no longer be used in AKS Ubuntu 18.04 images. This weeks image, [AKSUbuntu-1804-2021.03.09](https://github.com/Azure/AgentBaker/blob/master/vhdbuilder/release-notes/AKSUbuntu/gen1/1804/2021.03.03.txt) resolves past issues regarding private DNS with .local entries not working with [Kubernetes 1.18 and Ubuntu 18.04](https://github.com/Azure/AKS/issues/2052).
* Preview Features
  * Kubenet support for Pod Identity.
* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1790.210302](vhd-notes/AKSWindows/2019/17763.1790.210302.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.03.03](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.03.03.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.03.03](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.03.03.txt).

## Release 2021-03-01

This release is rolling out to all regions - ETA for conclusion 2021-03-10 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on June 30th, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Starting last week, the week of Feb 22nd (Azure China Cloud and Azure Government Cloud users will get this update in the following weeks), we will upgrade AKS clusters Calico network policy from Calico version v3.8.9 to v3.17.2 for cluster 1.20.2 and above. This upgrade will cause a breaking change to the default behavior of all-interfaces Host Endpoints. For customers that use Host Endpoints, and only these, this version brings a change. Please follow our [guidance](https://github.com/Azure/AKS/issues/2089) to apply the appropriate label and Global Network Policy if you want to keep the v3.8.9 default behavior of all-interfaces Host Endpoints.
* Systemd-resolved will no longer be used in AKS Ubuntu 18.04 images starting on next week's release. This resolves past issues regarding private DNS with .local entries not working with [Kubernetes 1.18 and Ubuntu 18.04](https://github.com/Azure/AKS/issues/2052).

### Release Notes

* Features
  * AKS Managed AAD now supports Just-in-Time Access is now Generally Available [GA](https://docs.microsoft.com/azure/aks/managed-aad#configure-just-in-time-cluster-access-with-azure-ad-and-aks).
  * Application Gateway Ingress Controller (AGIC) AKS Add-On is now Generally Available [GA].
  * Confidential Computing Nodes (DCSv2) AKS Add-on is now Generally Available [GA].
  * HTTP Application Routing addon now Generally Available in Gov Cloud.
  * Encrypted customer managed keys policy for AKS is now Generally Available [GA].
  * Public IP per node capability in AKS is now Generally Available [GA].
  * Deploy WebLogic on Azure Kubernetes Service (AKS) using custom Docker images is now Generally Available [GA](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/weblogic-aks).
  * Persistent Volume monitoring & Reports tab in Container Insights is now Generally Available [GA]. Read more here:
    * <https://docs.microsoft.com/azure/azure-monitor/containers/container-insights-persistent-volumes>.
    * <https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-reports>.
* Preview Features
  * Calico Windows support in AKS 1.20 for new clusters.
  * Planned Maintenance Windows in AKS.
  * Dynamic IP allocation & enhanced subnet support in AKS.
  * Containerize and migrate apps to Azure Kubernetes Service with Azure Migrate: App Containerization. [Read More Here](https://docs.microsoft.com/azure/migrate/tutorial-containerize-java-kubernetes).
* Behavioral Change
  * Windows Containers may fail to resolve DNS names in ~1 seconds after it is created successfully and the status is showing running. This may not affect all customers but only those with applications that requires FQDN resolution when starting up the container. The workaround is retry or sleep ~1 seconds. For feedback, please go to [Windows Container GitHub](https://github.com/microsoft/Windows-Containers).
* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1757.210220.](vhd-notes/AKSWindows/2019/17763.1757.210220.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.02.24](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.02.24.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.02.24](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.02.24.txt).

## Release 2021-02-22

This release is rolling out to all regions - ETA for conclusion 2021-03-03 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on June 30th, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Starting this week (Azure China Cloud and Azure Government Cloud users will get this update in the following weeks), we will upgrade AKS clusters Calico network policy from Calico version v3.8.9 to v3.17.2 for cluster 1.20.2 and above. This upgrade will cause a breaking change to the default behavior of all-interfaces Host Endpoints. For customers that use Host Endpoints, and only these, this version brings a change. Please follow our [guidance](https://github.com/Azure/AKS/issues/2089) to apply the appropriate label and Global Network Policy if you want to keep the v3.8.9 default behavior of all-interfaces Host Endpoints.
* Systemd-resolved will no longer be used in AKS Ubuntu 18.04 images starting on next week's release. This resolves past issues regarding private DNS with .local entries not working with [Kubernetes 1.18 and Ubuntu 18.04](https://github.com/Azure/AKS/issues/2052).
* CSI Drivers will become default for [Kubernetes versions 1.21+](https://docs.microsoft.com/azure/aks/csi-storage-drivers).

### Release Notes

* Component Updates
  * Calico updated to v3.17.2 for Kubernetes versions 1.20+.
  * NMI image updated to 1.7.4.
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.02.17](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.02.17.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.02.17](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.02.17.txt).

## Release 2021-02-15

This release is rolling out to all regions - ETA for conclusion 2021-02-24 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on June 30th, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Starting this week on 22 February 2021 (Azure China Cloud and Azure Government Cloud users will get this update in the following weeks), we will upgrade AKS clusters Calico network policy from Calico version v3.8.9 to v3.17.1 for cluster 1.20.2 and above. This upgrade will cause a breaking change to the default behavior of all-interfaces Host Endpoints. For customers that use Host Endpoints, and only these, this version brings a change. Please follow our [guidance](https://github.com/Azure/AKS/issues/2089) to apply the appropriate label and Global Network Policy if you want to keep the v3.8.9 default behavior of all-interfaces Host Endpoints.

### Release Notes

* Behavioral Change
  * Date/Time removed from tunnel-front log entries. Timestamps can still be viewed by adding --timestamps to your kubectl logs command.
* Bug Fixes
  * Fixed Auto Scaling issues with 1.19 Preview Clusters where no image is found for a distro to scale from.
  * A previous release defaulted to Gen2 VHDs for Kubernetes versions below 1.18.0. This implicitly changed the Ubuntu version from 16.04 to 18.04 for users still below 1.18.0. This has been fixed and users will only receive Gen2 VHDs for Kubernetes versions greater than or equal to 1.18.0.
  * Fixed AuthorizationFailed errors on cluster deletion operations to better expose to users.
  * Fixed case sensitivity problem when specifying "--os-type"
  * Fixed an Error Handling issue when provisioning node pools with Ephemeral OS and a VM size with no cache disk.
  * Fixed an issue with Azure Policy pods not getting scheduled with CriticalAddonsOnly taint [GithubIssue](https://github.com/Azure/AKS/issues/1963)

* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1697.210210](vhd-notes/AKSWindows/2019/17763.1697.210210.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.02.10](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.02.10.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.02.10](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.02.10.txt).

## Release 2021-02-08

This release is rolling out to all regions - ETA for conclusion 2021-02-17 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Starting on the week of 22 February 2021 (Azure China Cloud and Azure Government Cloud users will get this update in the following weeks), we will upgrade AKS clusters Calico network policy from Calico version v3.8.9 to v3.17.1 for any cluster 1.20.0 or above. This upgrade will cause a breaking change to the default behavior of all-interfaces Host Endpoints. For customers that use Host Endpoints, and only these, this version brings a change. Please follow our [guidance](https://github.com/Azure/AKS/issues/2089) to apply the appropriate label and Global Network Policy if you want to keep the v3.8.9 default behavior of all-interfaces Host Endpoints.

### Release Notes

* Features
  * Cluster Start/Stop is now [GA](https://docs.microsoft.com/azure/aks/start-stop-cluster).
* Preview Features
  * AKS now supports Private Clusters created with a custom DNS zone (BYO DNS zone). Read more [here](https://docs.microsoft.com/azure/aks/private-clusters#configure-private-dns-zone).
  * AKS now allows you to reuse your standard LoadBalancer outbound IP (created by AKS) as Inbound IP to your services (and vice-versa) from Kubernetes v1.20+.
  * AKS now supports reusing the same Load Balancer IP across multiple services from Kubernetes v1.20+.
* Behavioral Change
  * The AKS default storage class behavior now will be to delay the creation of a Persistent Volume until a pod is created. Allowing the Persistent Volume to be created in the same zone as the pod. Read more [here](https://docs.microsoft.com/azure/aks/azure-disk-csi#create-a-custom-storage-class).
* Component Updates
  * Update default Windows Azure CNI to v1.2.2.
  * Calico updated to v3.8.9.2.
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1697.210127](vhd-notes/AKSWindows/2019/17763.1697.210127.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.02.03](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.02.03.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.02.03](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.02.03.txt).

## Release 2021-02-01

This release is rolling out to all regions - ETA for conclusion 2021-02-12 for public cloud.

### Announcements

* Kubernetes 1.16 is officially deprecated in AKS
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* Starting on the week of 15 February 2021 (Azure China Cloud and Azure Government Cloud users will get this update in the following weeks), we will upgrade AKS clusters Calico network policy from Calico version v3.8.9 to v3.17.1. This upgrade will cause a breaking change to the default behavior of all-interfaces Host Endpoints. For customers that use Host Endpoints, and only these, this version brings a change. Please follow our [guidance](https://github.com/Azure/AKS/issues/2089) to apply the appropriate label and Global Network Policy if you want to keep the v3.8.9 default behavior of all-interfaces Host Endpoints.

### Release Notes

* Features
  * Generation 2 Virtual Machines are now [GA on AKS](https://docs.microsoft.com/azure/aks/cluster-configuration#generation-2-virtual-machines-preview).
  * New Kubernetes patch version available, 1.19.7
* Preview Features
  * Kubernetes 1.20.2 is now in preview
* Bug Fixes
  * Fixed ContainerD + Kubenet - Pod IP SNAT/Masquerade Behavior [GitHubIssue](https://github.com/Azure/AKS/issues/2031).
* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1697.210127](vhd-notes/AKSWindows/2019/17763.1697.210127.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.01.28](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.01.28.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.01.28](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.01.28.txt).

## Release 2021-01-25

This release is rolling out to all regions - ETA for conclusion 2021-02-03 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* As previously announced, with the Holiday Season ending AKS will deprecate Kubernetes v1.16, completing the extension given after the GA of v1.19 for the holiday season and returning to the regular 3 supported versions window. After the week of January 31st, 2021 you will no longer be able to create v1.16.x based clusters or node pools.

### Release Notes

* Features
  * AKS now supports NCasT4_v3 SKUs.
  * New Kubernetes patch versions available, v1.17.16, 1.18.14, v1.19.6.
  * AKS Managed AAD now supports Azure AD Conditional Access. See more: <https://docs.microsoft.com/azure/aks/managed-aad#use-conditional-access-with-azure-ad-and-aks>
* Preview Features
  * AKS now supports WinDSR in AKS Windows nodes in preview by registering the `Microsoft.ContainerService/EnableAKSWindowsDSR` feature flag.
  * New options for Custom node Configuration: `ContainerLogMaxSizeMB`, `ContainerLogMaxFiles`, `PodMaxPids`.
  * AKS now supports Auto-Upgrade channels. <https://aka.ms/aks/autoupgrade>
* Bug Fixes
  * When clusters that are using bring your own subnet and route table with kubenet are deleted, they will now clean up any routes set by Kubernetes/AKS.
  * Added new IP availability validation for cluster upgrade of kubenet clusters.
  * Fixed bug where `Standard_DC2s_v2`, `Standard_DC4s_v2`, `Standard_DC8_v2` were incorrectly listed as supporting Accelerated Networking resulting in creation failures.
* Behavioral Change
  * The [Reset Service Principal](https://docs.microsoft.com/azure/aks/update-credentials) operation will now perform a node image upgrade in-order to update the configuration of each agent node.
* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1697.210113](vhd-notes/AKSWindows/2019/17763.1697.210113.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2021.01.13](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2021.01.13.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2021.01.13](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2021.01.13.txt).
  * Virtual Node updated to Virtual Kubelet 1.3.2.

## Release 2021-01-04

This release is rolling out to all regions - ETA for conclusion 2021-01-13 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* AKS has defaulted Azure CNI to transparent mode (from its previous default of bridge mode). This should bring no impact and carries several benefits, read more about it [here](https://aka.ms/aks/transparent-mode)
* As previously announced, with the Holiday Season ending AKS will deprecate Kubernetes v1.16, completing the extension given after the GA of v1.19 for the holiday season and returning to the regular 3 supported versions window. After the week of January 31st, 2021 you will no longer be able to create v1.16.x based clusters or nodepools.

### Release Notes

* Features
  * AKS now supports every CPU-based SKU dynamically. This means that every new CPU-based SKU is automatically supported by AKS so long they're not on the [restrictions list](https://docs.microsoft.com/azure/aks/quotas-skus-regions#restricted-vm-sizes). GPU-based SKUs and other specialty SKUs still require additional validation before being enabled
  * AKS Cluster Autoscaler now exposes the `max-node-provision-time` and `priority` properties as part of the [Cluster Autoscaler profile](https://docs.microsoft.com/azure/aks/cluster-autoscaler#using-the-autoscaler-profile).
* Bug Fixes
  * Fixed edge case on Bring your own subnet + kubenet network plugin scenarios where the route table was not correctly associated before the nodes started being created.
  * Better handling of race condition with liveness probes of the `aks-link` component.
  * Cluster autoscaler bug fix for incorrectly reading the value of `new-pod-scale-up` and improvements to CA liveness probe.
  * Case insensitivity fix for `networkPlugin``, networkPolicy` and `loadbalancerSku`.
  * Bug fixed on BYO Route Table kubenet scenarios where the cluster deletion didn't correctly clean up the route table rules created by kubernetes.
* Preview Features
  * Cluster Start/Stop now works in clusters with Cluster Autoscaler enabled and Private clusters.
* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1579.201208](vhd-notes/AKSWindows/2019/17763.1637.201215.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.12.15](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.12.15.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2020.12.15](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.12.15.txt).
  * Updated Azure CNI plugin version for Linux and Windows to 1.2 - <https://github.com/Azure/azure-container-networking/releases>.

## Release 2020-11-30

This release is rolling out to all regions - ETA for conclusion 2020-12-09 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.
* AKS will be defaulting Azure CNI to transparent mode (from its current default of bridge mode) on the next release. This should bring no impact and carries several benefits, read more about it [here](https://aka.ms/aks/transparent-mode)

### Release Notes

* Features
  * Bring your Own (BYO) Control Plane Managed Identity is Now Generally Available.
  * You may now update your Uptime SLA clusters to Free.
* Behavioral Changes
  * AKS Clusters will from now on choose to fail the upgrade if the drain/evict operation doesn't succeed instead of timing out. This means that users must ensure their PodDisruptionBudgets (PDBs) allow their pods to be successfully moved. To see if you have any incorrect PDB check [AKS Diagnostics](https://docs.microsoft.com/azure/aks/concepts-diagnostics) and search for PDBs and Node Drain Failures to see if you have any problematic PDBs in your cluster.
* Preview Features
  * AKS now supports [Custom Node Configuration](https://aka.ms/aks/custom-node-config) in Public Preview.
  * AKS now supports Private Clusters created with no Private DNS zone, deferring all DNS to an enterprise-managed DNS server.
    * You can create a cluster like this by using `--private-dns-zone none`, and making sure your custom DNS server is on the cluster subnet and contains all necessary entries including the API server endpoint IP (you can add after the cluster is created).
  * [Azure AD Pod Identity Add-on](https://aka.ms/aks/pod-identity) is now in public preview.

## Release 2020-11-16

This release is rolling out to all regions - ETA for conclusion 2020-11-25 for public cloud.

### Announcements

* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * Ephemeral OS is now Generally Available (GA). From now on ephemeral OS will be the default OS disk type for all SKUs and disk sizes that support it. See more [here](https://aka.ms/aks/ephemeral-os).
  * Kubernetes v1.19 is now Generally Available (GA).
  * ContainerD is now Generally Available (GA) and the default container runtime for cluster created or upgraded to kubernetes v1.19+. See more [here](https://aka.ms/aks/containerd).
  * Max Surge upgrades are now generally available. See more [here](https://aka.ms/aks/maxsurge).
* Behavioral changes
  * The command `az aks browse` will now open the [Azure Portal Kubernetes resource view](https://docs.microsoft.com/azure/aks/kubernetes-portal) after the Azure CLI v2.15.0.
  * A new property `subnetCIDR` was added for the Application Gateway Ingress Controller (AGIC) addon. This property will eventually replace `subnetPrefix`, and is used by AGIC to create a new subnet for Application Gateway. Application Gateway is deployed in this subnet and is then configured by AGIC to provide ingress capability to AKS.
  * Added additional username and password validations for windows. The minimal password length in AKS is 14 characters. See more [here](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile).
  * AKS Base images now come from [Shared Image Gallery](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries) and no longer from the Azure Marketplace.
* Bug Fixes
  * Fixed issued caused by Chrony on recent AKSUbuntu-1604-2020.10.28 images.
* Component Updates
  * Azure Monitor for Containers updated to [version 11092020](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#11092020--).
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1577.201111](vhd-notes/AKSWindows/2019/17763.1577.201111.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.11.11](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.11.11.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2020.11.11](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.11.11.txt).

## Release 2020-11-09

This release is rolling out to all regions - ETA for conclusion 2020-11-19 for public cloud

### Announcements

* AKS will default to containerd as the default runtime on kubernetes v1.19+ after this feature GAs. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19. If you are doing container builds in cluster please use the recommended [docker buildx](https://github.com/docker/buildx).
* After the GA of Ephemeral OS and release of the 2020-11-01 AKS API version. Clusters and nodepools will be created by default with Ephemeral OS. You can still select managed disks explicitly if you prefer that option. See more at <https://aka.ms/aks/ephemeral-os>.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * Migration from Service Principal-based clusters to Managed Identity Managed clusters is now supported. See how [here](https://docs.microsoft.com/azure/aks/use-managed-identity#update-an-existing-service-principal-based-aks-cluster-to-managed-identities).
  * Added Standard_Dxds_v4 VM SKUs.
  * Added Standard_exds_v4.
* Behavioral changes
  * From kubernetes clusters v1.19+ `az aks browse` CLI command will open the Azure Portal resource view instead of the kubernetes dashboard that no longer is supported on these clusters.
* Bug Fixes
  * The AKS control plane will always send RST for idle connections after 4min. Closes #1052, #1755, #1877.
  * Fixed issue with etcd replica management.
* Component updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.10.28](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.10.28.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2020.10.28](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.10.28.txt).
  * Azure Monitor for Containers updated to [version 10272020](https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#10272020--)
  * Updated CNI network monitor addon to version v1.1.18.
  * The new AKS API version 2020-11-11 [has been published](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/containerservice/resource-manager/Microsoft.ContainerService/stable/2020-11-01).

## Release 2020-10-26

This release is rolling out to all regions - ETA for conclusion 2020-11-11

### Announcements

* AKS and Holiday Season: To ease the burden of upgrade and change during the holiday season, AKS is extending a limited scope of support for all clusters and nodepools on 1.16 as a courtesy. Customers with clusters and nodepools on 1.16 after the [announced deprecation date of 2020-11-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy).
 The scope of this limited extension is effective from '2020-11-30 to 2021-01-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.16.
  * CRUD operations on 1.16 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* AKS will default to containerd as the default runtime on kubernetes v1.19+ after this feature GAs. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19. If you are doing container builds in cluster please use the recommended [docker buildx](https://github.com/docker/buildx).
* After the GA of Ephemeral OS and release of the 2020-11-01 AKS API version. Clusters and nodepools will be created by default with Ephemeral OS. You can still select managed disks explicitly if you prefer that option. See more at <https://aka.ms/aks/ephemeral-os>.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st, 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * Outbound type UDR now allows for dynamic route exchange over BGP for Private Clusters.
  * Proximity Placement Group support is now Generally Available. Releases for #1351
  * Spot Node pools are now Generally Available. Releases for #982
  * Support for `CriticalAddonsOnly` taint as an exception for system pools. Releases for #1833
  * Support for soft Taints. Resolves #1484
  * New Kubernetes patch versions available, v1.17.13, 1.18.10.
* Preview Features
  * New Preview Kubernetes patch versions available, 1.19.3.
* Bug Fixes
  * Fixed misalignment of taint validations with upstream kubernetes validations. Fixes #1412
* Component updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.10.21](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.10.21.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2020.10.21](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.10.21.txt).

## Release 2020-10-19

This release is rolling out to all regions - ETA for conclusion 2020-10-28.

### Announcements

* AKS and Holiday Season: To ease the burden of upgrade and change during the holiday season, AKS is extending a limited scope of support for all clusters and nodepools on 1.16 as a courtesy. Customers with clusters and nodepools on 1.16 after the [announced deprecation date of 2020-11-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy).
 The scope of this limited extension is effective from '2020-11-30 to 2021-01-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.16.
  * CRUD operations on 1.16 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* AKS will default to containerd as the default runtime on kubernetes v1.19+ after this feature GAs. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19. If you are doing container builds in cluster please use the recommended [docker buildx](https://github.com/docker/buildx).
* After the GA of Ephemeral OS and release of the 2020-11-01 AKS API version. Clusters and nodepools will be created by default with Ephemeral OS. You can still select managed disks explicitly if you prefer that option. See more at <https://aka.ms/aks/ephemeral-os>.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * New cluster autoscaler parameters available for the [cluster autoscaler profile](https://docs.microsoft.com/azure/aks/cluster-autoscaler#using-the-autoscaler-profile). New parameters supported: `skip-nodes-with-local-storage`, `skip-nodes-with-system-pods`, `max-empty-bulk-delete`, `expander`, `new-pod-scale-up-delay`, `max-total-unready-percentage`, `ok-total-unready-count`
  * New Admission controllers supported from kubernetes v1.19: `PodNodeSelector`, `PodTolerationRestriction`, `ExtendedResourceToleration`. See how to use them [here](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#extendedresourcetoleration) Releases for #1143, #1719 and #1449.
* Bug fixes
  * Fixed a few CPU throttling and health probe issues on the AKS control plane.
  * Updated signed PowerShell package to v0.0.3. Fixes #1772
  * Fixed issue with Azure policy addon and kubernetes v1.19 preview. Fixes #1869
* Component updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1397.201014](vhd-notes/AKSWindows/2019/17763.1397.201014.txt).
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.10.15](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.10.15.txt).
  * AKS Ubuntu 18.04 image updated to [AKSUbuntu-1804-2020.10.15](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.10.15.txt).

## Release 2020-10-12

**This release is rolling out to all regions - ETA for conclusion 2020-10-22**

### Announcements

* AKS and Holiday Season: To ease the burden of upgrade and change during the holiday season, AKS is extending a limited scope of support for all clusters and nodepools on 1.16 as a courtesy. Customers with clusters and nodepools on 1.16 after the [announced deprecation date of 2020-11-30](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) will be granted an extension of capabilities outside the [usual scope of support for deprecated versions](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy).
 The scope of this limited extension is effective from '2020-11-30 to 2021-01-31' and is limited to the following:
  * Creation of new clusters and nodepools on 1.16.
  * CRUD operations on 1.16 clusters.
  * Azure Support of non-Kubernetes related, platform issues. Platform issues include trouble with networking, storage, or compute running on Azure. Any support requests for K8s patching and troubleshooting will be requested to upgrade into a supported version.
* AKS will default to containerd as the default runtime on kubernetes v1.19+ after this feature GAs. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19. If you are doing container builds in cluster please use the recommended [docker buildx](https://github.com/docker/buildx).
* After the GA of Ephemeral OS and release of the 2020-11-01 AKS API version. Clusters and nodepools will be created by default with Ephemeral OS. You can still select managed disks explicitly if you prefer that option. See more at <https://aka.ms/aks/ephemeral-os>.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * You can now mutate the AKS default storage class. See how here <https://docs.microsoft.com/azure/aks/azure-disks-dynamic-pv>
  * AKS now supports Dv4 and DSv4 VM SKU.
* Bug Fixes
  * Fixed bug that allow setting unsupported labels. Unsupported labels are now blocked as per: <https://github.com/kubernetes/enhancements/blob/master/keps/sig-auth/0000-20170814-bounding-self-labeling-kubelets.md#proposal>
  * Fixed an issue with Managed-AAD and kubernetes v1.19 preview. Closes #1891
  * Fixed an issue where topology labels where not correctly preserved after upgrade.
* Behavior change
  * The `calico-typha` deployment is now called `calico-typha-deployment`.
  * The revisionHistoryLimit is now set to 2 for managed components and addon deployments. Closes #1502
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.10.08](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.10.08.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.10.08](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.10.08.txt).

## Release 2020-09-21

**This release is rolling out to all regions - ETA for conclusion 2020-09-30**

* AKS will default to containerd as the default runtime in kubernetes v1.19. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19. If you are doing container builds in cluster please use the recommended [docker buildx](https://github.com/docker/buildx).
* [New Date] We heard your feedback and as such, the Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * Azure Policy Addon is now Generally Available, see <https://azure.microsoft.com/updates/ga-policy-addon-for-azure-kubernetes-service>
  * New Kubernetes patches available. v1.16.15 and v1.17.11
  * New detectors for [AKS Diagnostics](https://docs.microsoft.com/azure/aks/concepts-diagnostics):
    * Node drain failure detector that calls out node drain failures that might impact the cluster workloads.
    * Managed-AAD integration detector that looks for common issues with AADv2 integration like the kubectl minimum version.
* Preview features
  * AKS now supports the ability to completely stop and start clusters on demand. A great option for times your cluster might be idle. Start here: <https://aka.ms/aks/stop-cluster>
  * Ephemeral OS is now part of the 2020-09-01 AKS API and can be enabled through ARM. This will also allow you to in the future keep using managed network-attached disk if you want. <https://github.com/Azure/azure-rest-api-specs/blob/master/specification/containerservice/resource-manager/Microsoft.ContainerService/stable/2020-09-01/examples/AgentPoolsCreate_Ephemeral.json>
  * Azure RBAC for Kubernetes Authorization is now in Public Preview and open to anyone to test (no form required): <https://docs.microsoft.com/azure/aks/manage-azure-rbac>
  * Kubernetes v1.19 is now in public preview
  * Azure Confidential Compute addon for AKS is not in public preview providing you with confidential nodes: <https://docs.microsoft.com/azure/confidential-computing/confidential-nodes-aks-get-started>
  * AKS now integrates with Azure Files NFS shares on its CSI storage drivers. See more here: <https://docs.microsoft.com/azure/aks/azure-files-csi#nfs-file-shares>
* Bug fix
  * Issue where addon names where not accepted with any casing fixed.
* Component updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1397.200904](vhd-notes/AKSWindows/2019/17763.1397.200904.txt)
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.09.17](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.09.17.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.09.17](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.09.17.txt).
* Behavior Change
  * AKS is now validating/blocking labels that are [disallowed upstream](https://github.com/kubernetes/enhancements/blob/master/keps/sig-auth/0000-20170814-bounding-self-labeling-kubelets.md#proposal).

## Release 2020-08-31

**This release is rolling out to all regions - ETA for conclusion 2020-09-18**

* AKS will default to containerd as the default runtime in kubernetes v1.19. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19. If you are doing container builds in cluster please use the recommended [docker buildx](https://github.com/docker/buildx).
* [New Date] We heard your feedback and as such, the Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2021.
* Once GA AKS will default to its new [GPU specialized image](https://aka.ms/aks/specialized-gpu-image) as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * Kubernetes version 1.18 is now Generally Available (GA) on AKS. (1.15 is being retired as this release progressively reaches all regions, as previously communicated). Check the [release calendar](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#aks-kubernetes-release-calendar) for future version release and GA date.
  * New Kubernetes patch versions available, v1.18.8.
  * The AKS Kubernetes Audit logs are now split in 2 categories to allow you granularly subscribe and save costs.
    * `kube-audit-admin`: This category contains only audit events that include write verbs (`create`,`update`,`delete`,`patch`,`post`)
    * `kube-audit`: This category contains all remaining audit events.
  * AKS Ubuntu 18.04 is now Generally Available and will be the default agent node base image on k8s v1.18 and onward.
* Preview Features
  * AKS now supports [Azure disk and Azure files CSI storage drivers](https://docs.microsoft.com/azure/aks/csi-storage-drivers) in Public preview.
* Bug Fix
  * Fixed an issue where non-AKS managed identities (eg. from Pod Identity) would be lost after an AKS upgrade.
  * Fixed bug where the VMSS backend pool was removed after a Service Principal reset operation.
* Behavior Changes
  * Ensure all components use only strong ciphers (matching the AKS API server). Metrics server now only allows the following cipher suites:
    * TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    * TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
    * TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    * TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* Component Updates
  * Azure Policy Addon updated to Gatekeeper beta12 and Policy 0804 versions.
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1397.200820](https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.1397.200820.txt)
  * Azure Monitor for Containers versions updated: <https://github.com/microsoft/Docker-Provider/blob/ci_prod/ReleaseNotes.md#08072020-->
    * Linux version mcr.microsoft.com/azuremonitor/containerinsights/ciprod:ciprod08072020
    * Windows version mcr.microsoft.com/azuremonitor/containerinsights/ciprod:win-ciprod08072020
  * Add LivenessProbe and ReadinessProbe for Metrics Server.
  * Updated AKS Moby version to 19.03.12 (from now on AKS Moby versions will follow docker versioning to assist scanning tools false positives).
  * Updated NVIDIA GPU drivers to v450.51.06.
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.08.28](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.08.28.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.08.28](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.08.28.txt).

## Release 2020-08-17

**This release is rolling out to all regions - ETA for conclusion 2020-08-26**

### Important Service Updates

* AKS will default to AKS ubuntu 18.04 in upcoming GA of kubernetes v1.18 which marks the GA of AKS Ubuntu 18.04 as well. We recommend testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. See how here: <https://aka.ms/aks/Ubuntu1804>
* AKS will default to containerd as the default runtime in kubernetes v1.19. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19. If you are doing container builds in cluster please use the recommended [docker buildx](https://github.com/docker/buildx).
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2020.
* Kubernetes version 1.18 will GA on the week of August 31st and you will no longer be able to create 1.15.x based clusters or nodepools.
* Once GA AKS will default to the GPU specialized image as the supported option for GPU-capable agent nodes.

### Release Notes

* Features
  * You can now see the [AKS Authentication Webhook Server](https://docs.microsoft.com/azure/aks/concepts-identity#webhook-and-api-server) logs for the Azure AD Integrated clusters, as part of the [AKS Control Plane logs](https://docs.microsoft.com/azure/aks/view-master-logs).
* Preview Features
  * AKS now has a specialized GPU Node Image that already includes not only the docker drivers but also the NVIDIA device plugin, so ready to use. See more at: <https://aka.ms/aks/specialized-gpu-image>
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.08.13](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.08.13.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.08.13](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.08.13.txt).

## Release 2020-08-10

**This release is rolling out to all regions - ETA for conclusion 2020-08-21**

### Important Service Updates

* AKS will default to AKS ubuntu 18.04 in upcoming GA of kubernetes v1.18 which marks the GA of AKS Ubuntu 18.04 as well. We recommend testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. See how here: <https://aka.ms/aks/Ubuntu1804>
* AKS will default to containerd as the default runtime in kubernetes v1.19. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2020.
* Kubernetes version 1.18 will GA on the week of August 31st and you will no longer be able to create 1.15.x based clusters or nodepools.

### Release Notes

* Features
  * AKS now supports autoscaling to 0. (latest CLI extension and following core CLI version)
* Bug fixes
  * Fixed a bug when scaling windows node pools and the windows profile parameters were missing.
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.08.06](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.08.06.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.08.06](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.08.06.txt).

## Release 2020-08-03

**This release is rolling out to all regions - ETA for conclusion 2020-08-14**

### Important Service Updates

* AKS will default to AKS ubuntu 18.04 in upcoming GA of kubernetes v1.18 which marks the GA of AKS Ubuntu 18.04 as well. We recommend testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. See how here: <https://aka.ms/aks/Ubuntu1804>
* AKS will default to containerd as the default runtime in kubernetes v1.19. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2020.
* Kubernetes version 1.18 will GA on the week of August 31st and you will no longer be able to create 1.15.x based clusters or nodepools.

### Release Notes

* Preview Features
  * AKS now supports Ephemeral OS disks in Public Preview. Read more here: <https://aka.ms/aks/ephemeral-os>
  * AKS has announced the AKS Portal Resource view: <https://azure.microsoft.com/updates/kubernetes-resource-view-is-in-public-preview>
* Bug fixes
  * Fixed but where VNET CIDR was not set correctly on Windows nodes which could result in incorrect NATing behavior.

## Release 2020-07-27

**This release is rolling out to all regions - ETA for conclusion 2020-08-07**

### Important Service Updates

* AKS will default to AKS ubuntu 18.04 in upcoming GA of kubernetes v1.18 which marks the GA of AKS Ubuntu 18.04 as well. We recommend testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. See how here: <https://aka.ms/aks/Ubuntu1804>
* AKS will default to containerd as the default runtime in kubernetes v1.19. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA of kubernetes v1.19, containerd will be served by default for all new clusters or cluster that upgrade to v1.19.
* AKS has removed the custom "high-priority" and "addon-priority" Priority Classes which are no longer used by the service.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2020.
* Kubernetes version 1.18 will GA on the week of August 31st and you will no longer be able to create 1.15.x based clusters or nodepools.

### Release Notes

* Features
  * AKS-managed Azure AD integration (v2) is now generally available: <https://docs.microsoft.com/azure/aks/managed-aad>
    * You can now upgrade non-AzureAD clusters to AD-integrated clusters.
    * You can now upgrade clusters with the previous iteration of the AzureAD integration (v1) into AKS-managed AzureAD clusters (v2)
    * Closes: <https://github.com/Azure/AKS/issues/1489>
  * AKS is now supported on the following regions:
    * UAE Central - Closes <https://github.com/Azure/AKS/issues/1693>
    * West Central US - Closes <https://github.com/Azure/AKS/issues/998>
  * New Kubernetes patch versions available v1.16.13, v1.17.9
    * This patch versions respond the following CVEs:
      * <https://github.com/Azure/AKS/issues/1724>
      * <https://github.com/Azure/AKS/issues/1731>
      * <https://github.com/Azure/AKS/issues/1732>
    * We've heard your feedback and we will not be removing all the previous versions due to the vulnerabilities. Instead we've patched all the latest GA ones (v1.15.11, v1.15.12, v1.16.10, v1.17.7) and you can still mitigate the vulnerabilities in any GA version by leveraging <https://aka.ms/aks/node-image-upgrade>.
* Bug fixes
  * AKS has added the ready and health plugins to coredns. Closes <https://github.com/Azure/AKS/issues/1676>
  * We've added additional validations to prevent the creation of multiple node pool clusters with Basic Load Balancer which isn't supported.
* Preview features
  * AKS now supports Bring Your Own (BYO) control plane managed identity: <https://aka.ms/aks/byo-mi>
  * New Kubernetes patch versions are available for preview, v1.18.6.
* Behavior changes
  * After API version 2020-07-01 the node image upgrade operation will only allow POST and not PUT. CLI versions won't be affected.
  * A default load balancer is not longer created in UDR OutboundType clusters. The LB can be automatically created later if a public service of type LoadBalancer is created.
* Component Updates
  * Calico updated to v3.8.0
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1339.200716](https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.1339.200716.txt)
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.07.16](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.07.16.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.07.16](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.07.16.txt).

## Release 2020-07-06

**This release is rolling out to all regions**

### Important Service Updates

* AKS will default to AKS ubuntu 18.04 in upcoming GA of kubernetes 1.18 and after AKS Ubuntu 18.04 is GA as well. We recommend testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. See how here: <https://aka.ms/aks/Ubuntu1804>
* AKS will default to containerd as the default runtime in the upcoming months. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA containerd will be served for all new clusters on the latest kubernetes version clusters that upgrade to it.
* On the *next* release, AKS will be removing the custom "high-priority" and "addon-priority" Priority Classes which are no longer used by the service.
* The Azure Kubernetes Service [pod security policy (preview)](https://docs.microsoft.com/azure/aks/use-pod-security-policies) feature will be retired on May 31st 2020.

### Release Notes

* Features
  * Users can now reuse inbound and outbound IPs on the Load Balancer. After this change, users can assign an outbound IP that is same as an inbound IP in the AKS SLB.
* Preview Features
  * AKS is announcing the release of Azure RBAC integration for Kubernetes Authorization in Preview. Which allows you to control the RBAC of your cluster directly from the Azure Portal. See more at: <https://aka.ms/aks/azure-rbac>
  * AKS integration with Azure Policy and Gatekeeper now supports securing your pods with Azure Policy (with the equivalent controls that were made available previously in Pod Security Policies). Read more at <https://aka.ms/aks/azpodpolicy>
  * AKS now supports Azure Ultra disks in preview.
  * AKS now supports confidential workloads through DCSv2 SKUs (private preview). Read more [here](https://azure.microsoft.com/en-in/updates/azure-kubernetes-service-aks-now-supports-confidential-workloads-through-dcsv2-skus-preview/).
* Component Updates
  * New AKS base images - Upgrade to these using <https://aka.ms/aks/node-image-upgrade>
    * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.06.30](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.06.30.txt).
    * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.06.30](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.06.30.txt).

## Release 2020-06-29

**This release is rolling out to all regions**

### Important Service Updates

* AKS is preparing to GA the AKS Ubuntu 18.04 node image and recommends testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. **After AKS Ubuntu 18.04 is GA**, at the time of upgrading to a pre-announced kubernetes version, clusters running AKS Ubuntu 16.04 will receive this new image. See how here: <https://aka.ms/aks/Ubuntu1804>
* AKS will default to containerd as the default runtime in the upcoming months. During preview we encourage to create nodepools with the new container runtime to validate workloads still work as expected. And do check the [containerd differences and limitations](https://docs.microsoft.com/azure/aks/cluster-configuration#containerd-limitationsdifferences). After GA containerd will be served for all new clusters on the latest kubernetes version clusters that upgrade to it.

### Release Notes

* Features
  * Kubernetes version 1.17 is now Generally Available (GA) on AKS. (1.14 is being retired as this release progressively reaches all regions, as previously communicated).
  * New Kubernetes patch versions available, v1.15.12, v1.16.10, v1.17.7. Note that as per [policy](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions#supported-versions-policy-exceptions), versions <1.16.10 and <1.17.7 were removed due to severe bugs and security issues flagged upstream.
* Preview Features
  * New Kubernetes patch versions are available for preview, v1.18.4.
  * AKS now supports `containerd` as container runtime in preview. This runtime will be the default on AKS on the upcoming months. Read about it at <https://aka.ms/aks/containerd> and try it out!
  * AKS now supports Proximity Placement Groups in preview to provide collocation capabilities for low latency workloads. Read more about it at <https://aka.ms/aks/ppg> and try it out!
* Bug fixes
  * Fix issues on clusters using managed identities, where a cluster upgrade or scale operation would remove unknown identities.
  * Fixed bug where users where being shown more than the minor version above as available versions to upgrade to.
* Behavior changes
  * Kubernetes API server Log Level changed from 4 to 2. This will be reduce the log volume while keeping pair with k8s Prod recommendations.
  * Azure Policy for AKS is removing the built-in policies offered for the private preview version 1.0 of the add-on. The built-in policies with category name of "Kubernetes Service" will no longer function starting July 21st, 2020. To continue service, update the add-on to version 2.0 by following [these steps](https://docs.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes#install-azure-policy-add-on-for-aks) and use policies of category name "Kubernetes".
* Component Updates
  * Metrics Server has been updated to v0.3.6.
  * New AKS base images - Upgrade to these using <https://aka.ms/aks/node-image-upgrade>
    * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1282.200625](https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.1282.200625.txt)
    * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.06.25](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.06.25.txt).
    * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.06.25](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.06.25.txt).

## Release 2020-06-15

**This release is rolling out to all regions**

### Important Service Updates

* AKS is preparing to GA the AKS Ubuntu 18.04 node image and recommends testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. **After AKS Ubuntu 18.04 is GA**, at the time of upgrading to a pre-announced kubernetes version, clusters running AKS Ubuntu 16.04 will receive this new image. See how here: <https://aka.ms/aks/Ubuntu1804>
* Kubernetes version 1.17 will GA on the week of July 1st and you will no longer be able to create 1.14.x based clusters or nodepools.
  * Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>

### Release Notes

* Behavior changes
  * In advance of the GA of kubernetes v1.17 AKS is now defaulting to kubernetes v1.16 as the default version. If you have a dependency on the AKS default version, make sure your kubernetes APIs are up to date: <https://github.com/Azure/AKS/issues/1205>
* Component updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.06.10](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.06.10.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.06.10](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.06.10.txt).
  * Azure Monitor for Containers monitoring addon image was updated to ciprod05222020 and win-ciprod05222020-2 (for Windows). Notable changes:
    * Windows Logs - Starting from this release, users will see the agent automatically start collecting windows container STDOUT/STDERR logs and sending them to same log analytics workspace.
    * Metrics available for Alerting - Users will see the below metrics on the AKS 'Metrics' blade in the Azure portal, under the "Container Insights" Namespace.
      * Metrics:
        * diskUsagePercentage
        * completedJobsCount
        * oomKilledContainerCount
        * podReadyPercentage
        * restartingContainerCount
        * cpuExceededPercentage
        * memoryRssExceededPercentage
        * memoryWorkingSetExceededPercentage

## Release 2020-06-08

**This release is rolling out to all regions**

### Important Service Updates

* AKS is preparing to GA the AKS Ubuntu 18.04 node image and recommends testing existing workloads on AKS Ubuntu 18.04 nodepools prior to GA. **After AKS Ubuntu 18.04 is GA**, at the time of upgrading to a pre-announced kubernetes version, clusters running AKS Ubuntu 16.04 will receive this new image. See how here: <https://aka.ms/aks/Ubuntu1804>
* Kubernetes version 1.17 will GA on the week of July 1st and you will no longer be able to create 1.14.x based clusters or nodepools.
  * Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>

### Release Notes

* Features
  * It is now supported to upgrade clusters from Free to Paid on all regions that support Uptime SLA. This can be done after this weeks release finishes via ARM and on the next CLI version.
  * Windows Server container support is now Generally Available on Azure China regions.
* Preview Features
  * AKS has released [Node Image Upgrade](http://aka.ms/aks/nodeimageupgrade), to allow users to upgrade the node image of all their cluster nodes, or a specific nodepool, without requiring a full kubernetes upgrade. See more at: <http://aka.ms/aks/nodeimageupgrade>
  * AKS has released the Application Ingress Controller (AGIC) Addon in public preview. With it you can now easily install and leverage AGIC as a fully managed addon on AKS. More here: <https://aka.ms/aks/agic>
  * AKS now supports NDr_v2 with the gen2 preview.
* Bug fixes
  * Fixed issue where upgrading older clusters would fail due to incompatibility with the podpriority kubelet feature gate.
  * Fixed issue in Upgrade and Update operations where there was a mismatch between the Cluster Autoscaler node count and current node count.
  * AKS has cherry picked the following bug fixes into v1.15.11:
    * <https://github.com/kubernetes/kubernetes/pull/89337>
    * <https://github.com/kubernetes/kubernetes/pull/90749>
    * <https://github.com/kubernetes/kubernetes/pull/89794>
* Behavior changes
  * You are now allowed deploy AKS into dual-stack subnets on dual-stack vnets. The AKS cluster will only leverage the IPv4 stack currently.
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.05.31](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.05.31.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.05.31](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.05.31.txt).

## Release 2020-06-01

**This release is rolling out to all regions**

### Important Service Updates

* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* For any cluster created on K8s 1.18 or above, AKS will default the kube-dashboard add-on to disabled moving forward.
* Kubernetes version 1.17 will GA on the week of July 1st and you will no longer be able to create 1.14.x based clusters or nodepools.
  * Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>

### Release Notes

* Features
  * Outbound Type is now Generally Available and supports Kubenet based clusters. Read more at <https://aka.ms/aks/outboundtype>
  * AKS now supports custom Route Tables for Kubenet-based clusters, by enabling use of existing Route Tables so Kubernetes can add required routes for node communication. Read more at <https://aka.ms/aks/customrt>
* Preview Features
  * Support for Confidential Workloads on AKS and [DC-series](https://docs.microsoft.com/azure/virtual-machines/dcv2-series) nodepools are now in Private Preview. Read more at <https://aka.ms/accakspreview>.
  * AKS now supports Max Surge Upgrades in preview. Read more at <https://aka.ms/aks/maxsurge>
* Behavior changes
  * AKS default OS disk size is now 128GB, a [P10](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types#premium-ssd).
  * Set calico `IptablesFilterAllowAction` to return from its default value, so that requests not dropped by the policy can be compared against system chains. In this way, the requests to services without endpoints can be rejected following the intention of kube-proxy.
  * AKS has released [API version 2020-04-01](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/containerservice/resource-manager/Microsoft.ContainerService/stable/2020-04-01) which as previously announced defaults to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* Component Updates
  * Updated Kube-Dashboard images for 1.16, 1.17 and 1.18
    * 1.16 clusters will use dashboard:v2.0.0-rc3, 1.17 will use dashboard:v2.0.0-rc7, 1.18 will use dashboard:v2.0.1
    * Read more about the User Experience here: <https://docs.microsoft.com/azure/aks/kubernetes-dashboard>
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.05.27](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.05.27.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.05.27](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.05.27.txt).
  
## Release 2020-05-25

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 defaults to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>
* For any cluster created on K8s 1.18 or above, AKS will default the kube-dashboard add-on to disabled moving forward.

### Release Notes

* Features
  * Uptime SLA is now available in all Public Cloud regions.
* Bug Fixes
  * Fixed bug with Windows nodes and Managed Identity, where nodes were unable to pull images from ACR.
* Component Updates
  * Azure Policy image updated to version `prod_20200519.1`
  * Azure Network policy image updated to v1.1.2, <https://github.com/Azure/azure-container-networking/releases/tag/v1.1.2>
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1217.200513](https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.1217.200513.txt)

## Release 2020-05-18

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 (to be published) will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>
* Only Spot and Regular will be accepted as parameters for nodepool scaleSetPriority, low priority (now Spot) will no longer be accepted.
* For any cluster created on K8s 1.18 or above, AKS will default the kube-dashboard add-on to disabled moving forward.

### Release Notes

* Features
  * Windows Server container support is now Generally Available on Azure Government Cloud.
  * AKS has introduced new kubernetes patch versions v1.15.11, v1.16.8, v1.16.9.
* Preview Features
  * AKS has introduced new public preview patch version v1.17.4, v1.17.5.
  * AKS now supports Gen2 VMs in Public Preview.

    ```bash
    az feature register --name "Gen2VMPreview" --namespace "Microsoft.ContainerService"
    # wait for the feature to register
    az feature show --name Gen2VMPreview --namespace "Microsoft.ContainerService"
    # Re-register the AKS namespace by performing the below
    az provider register --namespace 'Microsoft.ContainerService'
    # Finally create the cluster
    az aks create -n aks -g aks -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
    ```

* Bug Fixes
  * Fixed an issue where if a nodepool operation was performed in a locked resource group it would return error 500 instead of correctly returning a ResourceGroupLocked error.
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.05.13](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.05.13.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.05.13](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.05.13.txt).

## Release 2020-05-11

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 (to be published) will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>
* Only Spot and Regular will be accepted as parameters for nodepool scaleSetPriority, low priority (now Spot) will no longer be accepted.

### Release Notes

* Features
  * AKS now offers an optional Paid Uptime SLA. Read more about it: <https://techcommunity.microsoft.com/t5/azure-kubernetes-service/aks-introduces-uptime-sla/ba-p/1350832>
* Preview Features
  * AKS now supports in preview kubernetes versions 1.18.1 and 1.18.2
  * AKS now supports creating nodepools leveraging AKS Ubuntu 18.04 images in any existing cluster
  eg. `az aks nodepool add -n ubuntu1804 --cluster-name aks -g aks --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804`
  * The Azure Policy Add-on for AKS has released a new version to integrate with OPA Gatekeeper v3. Read more here <https://github.com/Azure/AKS/issues/1606>
* Bug Fixes
  * Fixed bug where newly added agent pool did not inherit VnetCidrs from existing agent pools resulting in wrong nonMasqueradeCIDRs
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.05.06](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.05.06.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.05.06](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.05.06.txt).

## Release 2020-05-04

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 (to be published) will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>
* Only Spot and Regular will be accepted as parameters for nodepool scaleSetPriority, low priority (now Spot) will no longer be accepted.

### Release Notes

* Features
  * AKS has released an Admissions Enforcer to protect the system from Admission Controller Webhooks that might impact the kube-system components. Ready more about it [here](https://docs.microsoft.com/azure/aks/faq#can-admission-controller-webhooks-impact-kube-system-and-internal-aks-namespaces)
  * AKS now allows users to create windows nodepools with the latest windows image without requiring a cluster upgrade.
  * AKS now supports [HB series](https://docs.microsoft.com/azure/virtual-machines/hb-series) and [HBv2 series](https://docs.microsoft.com/azure/virtual-machines/hbv2-series) SKU families.

## Release 2020-04-27

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 (to be published) will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* Kubernetes 1.17 introduces API deprecations, please make sure your manifests are up to date before upgrading, and check Azure Advisor to confirm you are not using deprecated APIs. More information on 1.17 API deprecations here: <https://v1-17.docs.kubernetes.io/docs/setup/release/#deprecations-and-removals>
* Only Spot and Regular will be accepted as parameters for nodepool scaleSetPriority, low priority (now Spot) will no longer be accepted.

### Release Notes

* Features
  * Windows Server container support is now Generally Available on AKS.
* Preview Features
  * [The Public IP Per Node Feature](https://aka.ms/aks/pip-per-node) can now be used with Standard Load Balancer SKU.
* Bug Fixes
  * Added validation to prevent a subnetID update which would result on a failed nodepool.
  * Fixed issue when multiple delete operations where attempted simultaneously.
  * Fixed issue when performing cluster updates with APIs older than 2020-03-01 failed in clusters created using API 2020-03-01.
  * Fixed issue with agent count mismatch on upgrade.
  * Fixed an issue with a dependency on github:443 on Windows provisioning.
* Behavior Changes
  * Metrics-server now enforces burstable QoS class.
* Component Updates
  * Azure Network Policy (NPM) was updated from v1.0.33 to v1.1.0 - <https://github.com/Azure/azure-container-networking/releases/tag/v1.1.0>
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1158.200421](https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.1158.200421.txt).
  * Azure CNI was updated to 1.0.33 on Windows
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.04.16](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.04.16.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.04.16](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.04.16.txt).

## Release 2020-04-13

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 (to be published) will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.

### Release Notes

* Bug Fixes
  * Added CriticalAddonsOnly toleration for calico-typha-horizontal-autoscaler
  * Fixed a bug where the ILB backend bool would be removed after a manual VMSS update-instances command was issued.
* Features
  * AKS has now introduced a new **Mode** property for nodepools. This will allow you to set nodepools as *System* or *User* nodepools. System nodepools will have additional validations and will be preferred by system pods, while User pool will have more lax validations and can perform additional operations like scale to 0 nodes or be removed from the cluster. Each cluster needs at least one system pool. All details here: <https://aka.ms/aks/nodepool/mode>
    * System/User nodepools are available from core CLI version 2.3.1 or greater (or latest preview extension 0.4.43)
    * Nodepool mode requires API 2020-03-01 or greater
  * AKS now allows User nodepools to scale to 0.
  * AKS Diagnostics - Added networking and connectivity checks through our new Cluster Network Configuration detector. This allows you to check DNS and subnet related issues that may have impacted your cluster. It also highlights your network configuration to give you all this information at your fingertips.
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.04.06](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.04.06.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.04.06](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.04.06.txt).

## Release 2020-03-30

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.

### Release Notes

* Features
  * New VM GPU SKUs are now supported: Standard_NV12_Promo; Standard_NV12s_v3; Standard_NV24_Promo; Standard_NV24s_v3; Standard_NV48s_v3.
* Bug fixes
  * Added validation to block cluster creation if user specifies a subnet that is delegated
  * Fixed bug caused by apmz package being installed from <https://upstreamartifacts.blob.core.windows.net>, which is not in the AKS required endpoint egress list.
  * CoreDNS memory *limit* increased to 170Mb and assigned *Guaranteed* QoS class.
  * Fixed a bug with Cluster Proportional Autoscaler (CPA) version on 1.16. This bug is solved on version 1.7.1 which is now the version being used in AKS.
  * Fixed bug passing the correct nodepool at validation time on UDR OutboundType preview feature.
  * Patched bug where nodepool was not correctly added to internal SLB backend address pool: <https://github.com/kubernetes/kubernetes/issues/89336>
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.03.24](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.03.24.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.03.24](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.03.24.txt).

## Release 2020-03-23

**This release is rolling out to all regions**

### Important Service Updates

* AKS API version 2020-04-01 will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* **Two security issues were discovered in Kubernetes that could lead to a recoverable denial of service.**
  * CVE-2020-8551 affects the kubelet, and has been rated Medium ([CVSS:3.0/AV:A/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L](https://www.first.org/cvss/calculator/3.0#CVSS:3.0/AV:A/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L)).
  * CVE-2020-8552 affects the API server, and has also been rated Medium ([CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L](https://www.first.org/cvss/calculator/3.0#CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L)).
  * **Am I vulnerable?**
    * Only in cases where the attacker can make authorized resource requests to un-patched API server or kubelets.
    * Also AKS auto restarts apiserver and kubelet in the event of an OOM error which further limits exposure.
  * **How can I get the latest patched API and kubelet and fix this vulnerability?**
    * Upgrade to kubernetes versions v1.16.7 or v1.15.10. Or AKS preview versions v1.17.3

### Release Notes

* Bug fixes
  * Fixed bug that caused an error while updating existing AAD cluster with the new 2020-03-01 API
* Preview Features
  * Updated Azure Policy addon preview to use Gatekeeper v3 on new and updated addons.
    See more at <https://docs.microsoft.com/azure/governance/policy/concepts/rego-for-aks>
* Behavioral changes
  * All AKS Standard LBs will now have TCP Reset flag set to true.
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu-1604-2020.03.11](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.03.11.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu-1804-2020.03.11](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.03.11.txt).

## Release 2020-03-16

**This release is rolling out to all regions**

### Important Service Updates

* As previously announced we have retired support for Kubernetes 1.13 releases.
* AKS API version 2020-04-01 will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.

### Release Notes

* Features
  * An update to AAD integration (AADv2) is in public preview. Code has been rolled out; documents and cli extension to be published in the week of 23rd March.
  * AKS now exposes the balance-similar-node-groups setting on cluster autoscaler, which enables evenly balanced numbers of auto-scaled nodes across nodepools.
  * AKS has added 2 new built-in storage classes for Azure Files Standard (azurefile) and Azure Files Premium (azurefile-premium).
  * AKS Clusters using Managed Identity are now Generally Available (GA) and will no longer need a service principal.
* Behavioral Changes
  * The default azure disk storage class configuration has been changed from `Standard_LRS` to `StandardSSD_LRS`, and `allowVolumeExpansion` has been set to true.
  * Event deletion from the cluster will be audited to increase threat detection.
* Bug Fixes
  * A change in how swap nodes (used during upgrade of VMSS) are deleted from the cluster to increase reliability.
* Component Updates
  * AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.1075.200227](https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.1075.200227.txt).

## Release 2020-03-09

**This release is rolling out to all regions**

### Important Service Updates

* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). If you plan to upgrade to this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16.
* AKS API version 2020-04-01 will default to VMSS (Virtual Machine Scale Sets), SLB (Standard Load Balancer) and RBAC enabled.
* AKS has introduced AKS Ubuntu 18.04 in preview. During this time we will provide both OS versions side by side. **After AKS Ubuntu 18.04 is GA**, on the next cluster upgrade, clusters running AKS Ubuntu 16.04 will receive this new image.
* [Egress Breaking Change] Azure MCR has Updated its CDN endpoints - Read about it here: #1476

### Release Notes

* Features
  * Kubernetes version 1.16 is now Generally Available (GA) on AKS. (1.13 is being retired as previously communicated).
  * New Kubernetes patch versions available, v1.15.10, v1.16.7.
* Preview features
  * New Kubernetes patch versions (v1.17.3) are available for v1.17 preview.
  * AKS will now generate a default Windows username and password when creating a cluster (similarly as with ssh keys for Linux nodes). Customers can then add Windows pools to any newly created cluster without the need to have explicitly specified this parameters at create time. Customers can also reset this username and password at any time if they need it.
    * Note that, as before, You can only add Windows nodepools to clusters using VMSS and AzureCNI.
  * AKS now supports a new AKS base image based of Ubuntu 18.04 LTS.
    * You can test it by following:

        ```bash
        # Install or update the extension
        az extension add --name aks-preview
        # Register the preview feature flag
        az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
        # Create 18.04 based cluster
        az aks create -g <CLUSTER RG> -n <CLUSTER NAME> --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
        ```

    * If you want to continue to create 16.04 GA clusters, just omit the -aks-custom-headers.
* Behavioral Changes
  * To ensure user is correctly configuring OutboundType: UDR feature AKS now validates not only if a Route Table is present but also if it contains a default route from 0.0.0.0/0 to allow egress through an appliance, FW, on-prem GW, etc. More details how to correctly configure this feature can be found here: <https://docs.microsoft.com/azure/aks/egress-outboundtype>.
  * AKS enforces password expiration as part of CIS compliance but excludes the linux admin account that is using public key auth only. All accounts created using password will be subject to this enforcement.
    * As usual, with the GA of 1.16 the AKS default version follows n-1 and is now 1.15
    * As per <https://github.com/Azure/AKS/issues/1304> AKS will now upgrade the rest of the fleet to CoreDNS 1.6.6 after upgrading only non-Proxy users on [Release 2020-01-27](#release-2020-01-27).
* Component Updates
  * AKS Ubuntu 16.04 image updated to [AKSUbuntu:1604:2020.03.05](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.03.05.txt).
  * AKS Ubuntu 18.04 image release notes: [AKSUbuntu:1804:2020.03.05](vhd-notes/aks-ubuntu/AKSUbuntu-1804/2020.03.05.txt).
  * Updated to Moby 3.0.10 - <https://github.com/Azure/moby/releases/tag/3.0.10>.
  * Updated Azure CNI plugin version for Linux to 1.0.33 and Azure CNI plugin version for Windows 1.0.30 - <https://github.com/Azure/azure-container-networking/releases>.
  * External DNS image was updated to v0.6.0.
  * (Added 03/16/2020) AKS Windows image has been updated to [2019-datacenter-core-smalldisk-17763.973.200213](https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.973.200213.txt)

## Release 2020-03-02

**This release is rolling out to all regions**

### Important Service Updates

* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). When AKS supports this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16 when it is available in AKS.
* 1.16 will GA on the week of March 9th and you will no longer be able to create 1.13.x based clusters or nodepools.

### Release Notes

* Features
  * Added balance-similar-node-groups as an additional parameter users can configure for AKS Managed Cluster Autoscaler (CA)
* Behavioral Changes
  * For enhanced security AKS has removed CHACHA from API server accepted tls cipher suites.

## Release 2020-02-24

**This release is rolling out to all regions**

### Important Service Updates

* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). When AKS supports this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16 when it is available in AKS.
* With the introduction of Kubernetes v1.16 on the last release that marked the start of the deprecation for v1.13 in AKS. 1.13 is scheduled to be retired on February 28th.

### Release Notes

* Features
  * AKS now supports [Service Account Token Volume Projection](https://github.com/Azure/AKS/issues/1208)
* Preview Features
  * AKS now supports [Azure Spot NodePools](https://docs.microsoft.com/azure/aks/spot-node-pool)
* Bug Fixes
  * Fixed bug on Windows Nodepools preview where vnetCidrs were sometimes not set correctly on Windows nodepools resulting in wrong NAT exceptions on Windows nodes.

## Release 2020-02-17

**This release is rolling out to all regions**

### Important Service Updates

* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). When AKS supports this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16 when it is available in AKS.
* With the introduction of Kubernetes v1.16 on the last release that marked the start of the deprecation for v1.13 in AKS. 1.13 is scheduled to be retired on February 28th.

### Release Notes

* New Features
  * AKS Cluster AutoScaler now supports configuring the autoscaler profile parameters. <https://docs.microsoft.com/azure/aks/cluster-autoscaler#using-the-autoscaler-profile>
* Bug Fixes
  * Fixed bug when upgrading Virtual Machine Availability Set (VMAS) clusters that would trigger a PutNicOperation cancelled
  * Fixed bug causing throttling when using Internal Load Balancer
* Preview Features
  * AKS now supports adding tags and labels to nodepools
    * <https://github.com/Azure/AKS/issues/1088>
    * <https://github.com/Azure/AKS/issues/1344>
* Component Updates
  * AKS VHD image updated to [aks-ubuntu-1604-202002_202002.12](vhd-notes/aks-ubuntu/AKSUbuntu-1604/2020.02.12.txt)

## Release 2020-02-10

**This release is rolling out to all regions**

### Important Service Updates

* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). When AKS supports this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16 when it is available in AKS.
* With the introduction of Kubernetes v1.16 on the last release that marked the start of the deprecation for v1.13 in AKS. 1.13 is scheduled to be retired on February 28th.

### Release Notes

* New Features
  * Virtual Nodes are now supported in Canada Central
  * AKS now supports [Service Account Token Volume Projection](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection)
* Preview Features
  * Windows nodepools will change to use a vhd image provided by aks-engine. This release updates the Windows base image to version: 17763.864.191211 --> Rel Notes: <https://github.com/Azure/aks-engine/blob/master/vhd/release-notes/aks-windows/2019-datacenter-core-smalldisk-17763.864.191211.txt>
    * **Important** With this change, the Image  Publisher to "microsoft-aks" also changes, as such existing node pools cannot upgrade to this new image. To get the newest OS image, you'll have to create a new node pool.
* Bug Fixes
  * Improved error message when attempting to skip minor versions when performing an upgrade operation.
  * Fixed a bug where the dashboard would not work when RBAC was set to false for kubernetes v1.16/v1.17
* Behavioral Changes
  * AKS has released a new API [version](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/containerservice/resource-manager/Microsoft.ContainerService/stable/2020-02-01)

## Release 2020-02-03

**This release is rolling out to all regions**

### Important Service Updates

* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). When AKS supports this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16 when it is available in AKS.
* CoreDNS has been updated to v1.6.6. This change can affect users using the deprecated Proxy plugin which is no longer supported. Users should replace that with the Forward Plugin.
<https://github.com/Azure/AKS/issues/1304>
* With the introduction of Kubernetes v1.16 on the last release that marked the start of the deprecation for v1.13 in AKS. 1.13 is scheduled to be retired on February 28th.

### Release Notes

* New Features
  * AKS now supports specifying the Outbound Port and Idle Timeout properties on the Azure SLB. <https://aka.ms/aks/slb-ports>
* Bug Fixes
  * Fixed a bug that caused a billing extension error.
* Preview features
  * AKS now supports specifying Outbound type to define if the cluster should egress through the Standard Load Balancer (SLB) or a custom UDR (that sends egress traffic through a custom FW, on-prem gateway, etc.) Egress requirements are still the same, wherever the traffic egresses from. <https://aka.ms/aks/egress>
* Behavioral Changes
  * The private cluster FQDN format has changed from *guid.<region>.azmk8s.io to*guid.privatelink.<region>.azmk8s.io

## Release 2020-01-27

**This release is rolling out to all regions**

### Important Service Updates

* AKS has updated supported versions as announced in this [service update](https://azure.microsoft.com/updates/azure-kubernetes-service-will-be-retiring-support-for-kubernetes-versions-1-11-and-1-12/) and [AKS issue](https://github.com/Azure/AKS/issues/1235) to move from the "N-3" to "N-2" window. Starting December 9th, 2019 AKS has removed support for anything older than K8s 1.13 and scoped the active support window to K8s 1.13, 1.14, and 1.15.
* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). When AKS supports this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16 when it is available in AKS.
* CoreDNS will be updated to v1.6.6. This change can affect users using the deprecated Proxy plugin which is no longer supported. Users should replace that with the Forward Plugin.
<https://github.com/Azure/AKS/issues/1304>
* With the introduction of Kubernetes v1.16 this marks the start of the deprecation for v1.13 in AKS. 1.13 is scheduled to be retired on February 28th.

### Release Notes

* New Features
  * AKS now supports Customer-Managed keys (BYOK) to be used for encryption of both OS and Data Disks of AKS clusters.
  <https://docs.microsoft.com/azure/aks/azure-disk-customer-managed-keys>
  * New Supported SKUs: Standard_ND40s_v3, Standard_ND40rs_v2, Standard_D_v4, Standard_E_v4 and Standard_NP families
  * New supported patch version for kubernetes v1.15 (v1.15.7)
  * AKS now supports up to 10 nodepools.
  * Virtual Nodes is now supported in Korea Central
  * AKS now supports setting tags per nodepool. Which will propagate automatically to all nodes in the nodepool.
* Preview Features
  * AKS now supports Kubernetes versions 1.16 (1.16.1, 1.16.2) and 1.17 (1.17.0) in preview.
* Bug Fixes
  * Fixed bug with calico-typha health check in cases where localhost doesn't resolve 127.0.0.1
  * Fixed validation bug where users could not deploy AKS at the same time of their SLB Public IP resource
  * For clusters using Managed Identities and addons a bug was fixed where the addons' identity information was not displayed correctly.
  * Fixed bug where Accelerated Networking would be disabled after an upgrade.
  * Fixed issue while retrying to create the SLB default egress IP.
  * Fixed bug where DS3_v2 would be Network Accelerated despite supporting it.
  * Fixed several issues where under specific conditions users could see Azure API throttling on their subscriptions. - <https://github.com/Azure/AKS/issues/1413>
  * Fixed bug with `az aks reset-credentials --reset-aad` that would require manual intervention to complete.
* Component Updates
  * Updated to Moby 3.0.8 - <https://github.com/Azure/moby/releases/tag/3.0.8>
  * Updated AKS-Engine to 0.45.0 - <https://github.com/Azure/aks-engine/releases/tag/v0.45.0>
  * Azure Monitor for Containers Agent updated to [01072020 release](https://github.com/microsoft/Docker-Provider/releases/tag/v8.0.0.2)
    * **Important** Node cpu, node memory, container cpu and container memory metrics were obtained earlier by querying kubelet readonly port(<http://$NODE_IP:10255>). Agent now supports getting these metrics from kubelet port(<https://$NODE_IP:10250>) as well. During the agent startup, it checks for connectivity to kubelet port(<https://$NODE_IP:10250>), and if it fails the metrics source is defaulted to readonly port(<http://$NODE_IP:10255>).
  * AKS VHD image updated to [aks-ubuntu-1604-202001_2020.01.10](https://github.com/jackfrancis/aks-engine/blob/1de1ad55f86e863952081f0a6fbf85910d02e9d7/releases/vhd-notes/aks-ubuntu-1604/aks-ubuntu-1604-202001_2020.01.10.txt)

## Release 2019-12-02

**This release is rolling out to all regions**

### Important Service Updates

* AKS is updating supported versions as announced in this [service update](https://azure.microsoft.com/updates/azure-kubernetes-service-will-be-retiring-support-for-kubernetes-versions-1-11-and-1-12/) and [AKS issue](https://github.com/Azure/AKS/issues/1235) to move from the "N-3" to "N-2" window. Starting December 9th, 2019 AKS will remove support for anything older than K8s 1.13 and scope the active support window to K8s 1.13, 1.14, and 1.15.
* K8s 1.16 introduces API deprecations which will impact user workloads as described in this [AKS issue](https://github.com/Azure/AKS/issues/1205). When AKS supports this version user action is required to remove dependencies on the deprecated APIs to avoid disruption to workloads. Ensure you have taken this action prior to upgrading to K8s 1.16 when it is available in AKS.

### Release Notes

* Bug Fixes
  * Fixed cases of failed cluster creations due to an "Unregistering" or "NotRegistered" state for a subscription's access to NRP or CRP.
  * Added AKS validation that service principal secrets may not exceed 190 bytes.
* Behavior Changes
  * Fixed a bug where outbound IP creation for Standard Load Balancer did not retry when receiving internal server error from Network Resource Provider.
  * Improved validation of agent pool operations to only validate agent pool count when cluster autoscaler is turned off. When cluster autoscaler is turned on the minCount and maxCount set are used for count validations.

## Release 2019-11-18

**This release is rolling out to all regions**

### Release Notes

* New Features
  * Announcing AKS Diagnostics in Public Preview
    * Hopefully, most of the time your AKS clusters are running happily and healthily. However, when things go wrong, we want to make sure that our AKS customers are empowered to easily and quickly figure out what's wrong and the next steps for mitigation or deeper investigation.
    * AKS Diagnostics is a guided and interactive experience in the Azure Portal that helps you diagnose and solve potential issues with your AKS cluster, such as identity and security management, node issues, CRUD operations and more. Detectors in AKS Diagnostics intelligently find issues and observations as well as recommend next steps. This feature comes configured completely out-of-the-box and is free for all our AKS customers.
    * Get started and learn more here: <https://aka.ms/aks/diagnostics>
  * Support for new regions:
    * Norway East
    * Norway West
* Bug Fixes
  * Fixed enforcement that node pool versions can never be greater than the control plane `major.minor.patch` version.
  * Fixed error messages incorrectly stating a version was not supported to return proper errors detailing what validation was failed.
  * Added retries to retrieve a managed resource group. Errors can be returned with `ResourceGroupNotFound` due to slow Azure Resource Manager (ARM) data replication when AKS tries to place new managed resources into the managed resource group.
* Behavior Changes
  * Added a label `control-plane=true` to the `kube-system` namespace
* Component updates
  * AKS-Engine has been updated to [v0.43.0](https://github.com/Azure/aks-engine/releases/tag/v0.43.0)

## Release 2019-11-11

**This release is rolling out to all regions**

### Important Service Updates

* With the official 2019-11-04 Azure CLI release (v2.0.76), AKS has defaulted new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB). Users can still explicitly
  choose VMAS and BLB.
* From 2019-10-14 AKS Portal has defaulted new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB).
* From 2019-11-04 the CLI extension has a new parameter --zones to replace --node-zones, which specifies the zones to be used by the cluster nodes.

### Release Notes

* New Features
  * AKS has created a new default role clusterMonitoringUser to simplify the Azure Monitor Live metrics onboard experience so that moving forward users don't need to explicitly grant those permissions.
  This user will have 'GET' and 'LIST' permissions to  'POD/LOGS', 'EVENTS', 'DEPLOYMENTS', 'PODS', 'REPLICASETS' and 'NODES'.
  * Support for new regions:
    * Germany North
    * Germany West Central
    * UAE North
    * Switzerland North
    * Switzerland West
  * On-Demand Certificate Rotation is now Generally Available: <https://docs.microsoft.com/azure/aks/certificate-rotation>
* Bug Fixes
  * Fixed bug with MC_ infra resource group not being created/propagated quickly enough and triggering ResourceGroupNotFound errors.
  * Fixed missing cloud provider role binding: <https://github.com/Azure/AKS/issues/1104>
  * Fixed nodepool bug where a PUT would be accepted while the pool was being deleted.
  * Correctly assign the cluster-admin clusterrolebinding to the clusterAdmin user in all cases.
  * Fixed several upstream bugs with attach/detach in VMSS:
    * <https://github.com/kubernetes/kubernetes/pull/85158>
    * <https://github.com/kubernetes/kubernetes/pull/83685>
    * <https://github.com/kubernetes/kubernetes/pull/84917>
    * AKS is rolling this changes in automatically and users do not need to upgrade.
  * Fixed a bug upgrading Basic LB clusters that were using the preview of API Authorized Ranges feature, only supported in GA with Standard LB.
* Behavior Changes
  * Add priorityClass for calico-node and ensure calico-node tolerates all NoSchedule taints. This ensures calico-node will still be scheduled to all nodes even when users have added other node taints.
* Component updates
  * Metrics server has been updated to v0.3.5

## Release 2019-10-28

**This release is rolling out to all regions**

### Service Updates

* With the official 2019-11-04 Azure CLI release, AKS will default new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB). Users can still explicitly
  choose VMAS and BLB.
* From 2019-10-14 AKS Portal will default new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB).
* From 2019-11-04 the CLI extension will have a new parameter --zones to replace --node-zones, which specifies the zones to be used by the cluster nodes.

### Release Notes

* New Features
  * Multiple Nodepools backed AKS clusters are now Generally Available (GA)
    * <https://docs.microsoft.com/azure/aks/use-multiple-node-pools>
  * Cluster Autoscaler is now Generally Available (GA)
    * <https://docs.microsoft.com/azure/aks/cluster-autoscaler>
  * Availability Zones are now Generally Available (GA)
    * <https://docs.microsoft.com/azure/aks/availability-zones>
  * AKS API server Authorized IP Ranges is now Generally Available (GA)
    * <https://docs.microsoft.com/azure/aks/api-server-authorized-ip-ranges>
  * Kubernetes versions 1.15.5, 1.14.8 and 1.13.12 have been added.
    * These versions have new API call logic that helps users with many AKS clusters in the same subscription to incur is less throttling.
    * These versions have security fixes for [CVE-2019-11253](https://github.com/Azure/AKS/issues/1262)
  * The minimum `--max-pods` value has been altered from **30 per node to 30 per Nodepool**. Each node will have a hard **minimum of 10 pods** the user can specify, but this value can only be used if the total pods across all nodes on the nodepool accrue to 30+.
* Bug Fixes
  * Added additional validation to nodepool operations to check for enough address space. If there is no address space left for a scale/upgrade operation,
  the operation will not start and give a descriptive error message.
  * Fixed bug with Nodepool operations and `az aks update-credentials` to reflect on the agentpool state.
  * Fixed bug on Nodepool operations when using incorrect SKUs to have more descriptive error.
  * Added validation to block `az aks update-credentials` if nodepool is not ready to avoid conflicts.
  * Node count on the Nodepool is ignored when user has autoscaling enabled. (Manual scale with autoscaler enabled is not allowed)
  * Fixed bug where some clusters would still receive an older Moby version (3.0.6). Current version is 3.0.7
* Preview Features
  * Windows docker runtime updated to 19.03.2
* Component updates
  * Moby has been updated to v3.0.7
  * AKS-Engine has been updated to v0.41.5

## Release 2019-10-14

**This release is rolling out to all regions**

### Service Updates

* With the official 2019-11-04 Azure CLI release, AKS will default new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB).
* From 2019-10-14 AKS Portal will default new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB). Users can still explicitly
  choose VMAS and BLB.
* From 2019-11-04 the CLI extension will have a new parameter --zones to replace --node-zones, which specifies the zones to be used by the cluster nodes.

### Release Notes

* Bug Fixes
  * Fixed a bug where nodepool API would not accept and handle empty fields correctly, "", "{}", "{"properties":{}}".
  * Fixed a bug with http application routing addon where portal would lowercase all addon names and the input was not accepted.
  * Upgrade operation will not fail when manual changes have been applied to the SinglePlacementGroup property on underlying VMSS.
  * Fixed bug where customers trying to enable pod security policy without providing k8s version in the request would encounter failure (500 internal error).
  * Fixed bug where NPM pods would consume an excessive amount of memory.
* Preview Features
  * Updated windows image to the latest version.
* Component Updates
  * Updated Azure Network Policy (NPM) version to v1.0.28
  * Azure Monitor for Containers Agent updated to 2019-10-11 release: <https://github.com/microsoft/Docker-Provider/releases>

## Release 2019-10-07

**This release is rolling out to all regions**

### Service Updates

* With the official 2019-11-04 Azure CLI release, AKS will default new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB).
* From 2019-10-14 AKS Portal will default new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB). Users can still explicitly
  choose VMAS and BLB.

### Release Notes

* Behavioral Changes
  * Improved process and speed of upgrade to reduce impact to pods during the process
* Bug Fixes
  * Fixed a bug where kubelet reserved values where applied only to primary node pool. Now correctly applied to all nodepools if using multiple nodepools.
  * Added additional service principal validation on Upgrade.
  * Prevented multiple concurrent provisioning operations.
* New Features
  * Kubernetes versions 1.15.4, 1.14.7 and 1.13.11 have been added.
* Component Updates
  * AKS-Engine has been updated to v0.41.4

## Release 2019-09-30

**This release is rolling out to all regions**

### Service Updates

* With the official 2019-11-04 Azure CLI release, AKS will default new cluster
  creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM
  Availability Sets and Basic Load Balancers (VMAS/BLB).
* Support for node pool taints and public ip assignment per node with AKS will
  be available in Azure CLI extension v0.4.17
* AKS Availability Zone support has been expanded to the following regions:
  * Japan East
  * UK South
  * France Central
  * East US
  * Central US
  * Australia East

### Release Notes

* New Features
  * Customer may use NetworkPolicies with Azure CNI and Kubenet based clusters:
    * <https://docs.microsoft.com/azure/aks/use-network-policies>
  * Managed Identity (MSI) support is now in *public preview*.
    * <https://docs.microsoft.com/azure/aks/use-managed-identity>
* Bug Fixes
  * Fix a bug where the removal of an outbound rule from standard load balancer
    in the AKS node resource group could cause the failure of subsequent
    cluster operations.
  * Fixed the issue impacting GPU enabled clusters being unable to install the
    required NVidia drivers.
  * Fixed an issue where customers could encounter a CSE (custom script
    extension) error 99 during operations.
  * Fixed an issue with the Azure Portal cluster metrics multiplying the metric
    count based on the viewed window of time. Moving forward the default for
    these metrics will be correctly set to average() as opposed to sum().
    * For customers with metrics already enabled and in-use in portal, the sum()
      type will continue to be supported.
* Component Updates
  * AKS-Engine has been updated to v0.40.1
* Preview Features
  * Fixed an issue where nodes provisioned by cluster autoscaler would be
    de-provisioned when resetting or updating AAD credentials.

## Release 2019-09-23

### Service Updates

* Azure CLI 2.0.74 released with key AKS changes
  * <https://github.com/Azure/azure-cli/releases/tag/azure-cli-2.0.74>
  * Added `--load-balancer-sku` parameter to aks create command, which allows for
    creating AKS cluster with SLB
  * Added `--load-balancer-managed-outbound-ip-count`,
    `--load-balancer-outbound-ips` and `--load-balancer-outbound-ip-prefixes`
    parameters to aks `[create|update]` commands, which allow for updating load
    balancer profile of an AKS cluster with SLB
  * Added `--vm-set-type` parameter to aks create command, which allows to
    specify vm types of an AKS Cluster (vmas or vmss)

### Release Notes

* Bug Fixes
  * Fixed an issue where the node pool count rendered in the portal would be incorrect when not using the multiple node pools feature.
  * Fixed an issue to ensure a cluster upgrade will upgrade both the control plane and agent pools for clusters using VMSS, but not multiple agent pools.
  * Resolved an issue with cluster upgrades that could remove existing
    diagnostics settings and data erroneously.
  * Fixed an issue where AKS was not validating user defined taint formats per agent pool resulting in
    failures at cluster creation time.
* Behavioral Changes
  * Increased the reserved CPU cores for kubelets to scale proportionally to cores available on the kubelet's host node. Read more about [AKS resource reservation here](https://docs.microsoft.com/azure/aks/concepts-clusters-workloads#resource-reservations).
* Preview Features
  * Fixed an issue where AKS was not enforcing the minimum Kubernetes version
    required at additional agent pool creation time when using the multiple node pools feature.
  * Fixed an issue where creating new agent pools will overwrite the route
    table and customers would lose their route table rules. Fixes issue [#1212](https://github.com/Azure/AKS/issues/1212).

## Release 2019-09-16

**This release is rolling out to all regions**

### Service Updates

* The announced updates to default new clusters to VMSS/SLB configurations is
  under way, if you are using the `aks-preview` Azure CLI extension,
  all clusters created are now defaulted to VMSS & SLB.
* AKS Kubernetes 1.10 support will end-of-lifed on Oct 25, 2019
* AKS Kubernetes 1.11 & 1.12 support will end-of-lifed on Dec 9, 2019
* New Documentation additions:
  * [Authenticate with Azure Container Registry from AKS](https://docs.microsoft.com/azure/aks/cluster-container-registry-integration)
  * [Security hardening in AKS virtual machine hosts](https://docs.microsoft.com/azure/aks/security-hardened-vm-host-image)
* The AKS team is pleased to announce the new `aks-periscope` tool.
  * AKS Periscope will allow AKS customers to run initial diagnostics and
    collect logs into an Azure Blob storage account to help them analyze and
    identify potential problems.
  * For more information please see: <https://aka.ms/AKSPeriscope>

### Release Notes

* New Features
  * AKS now GA in the Azure US Gov Virginia region.
    * <https://azure.microsoft.com/updates/azure-kubernetes-service-is-now-available-in-azure-government/>
  * Control of egress traffic for cluster nodes in AKS is now GA
    * This feature allows you to restrict outbound network communication for
      you cluster as required for compliance or other secure use-cases.
    * <https://docs.microsoft.com/azure/aks/limit-egress-traffic>
* Known Issues:
  * Clusters that do not have PSPs enabled upgrading to Kubernetes 1.15 will fail
    * <https://github.com/Azure/AKS/issues/1220>
* Bug Fixes
  * An issue where excessively logs (eg node/status patch events) were being
    emitted to the audit logs stream and stored. Customer should now see
    greatly reduced audit log volume
* Preview Features
  * The `--control-plane-only` flag has been added to the `aks-preview` extension - this command
    will force the upgrade of the customers control plane without simultaneously
    upgrading the other nodepools. This functionality is only supported for
    multi-pool clusters.
    * See: <https://github.com/Azure/azure-cli-extensions/blob/master/src/aks-preview/HISTORY.md>

## Release 2019-09-09

**Service Updates**

* AKS Kubernetes 1.10 support will end-of-lifed on Oct 25, 2019
  * Please see: <https://azure.microsoft.com/updates/kubernetes-1-10-x-end-of-life-upgrade-by-oct-25-2019/>
* AKS Kubernetes 1.11 & 1.12 support will end-of-lifed on Dec 9, 2019
  * Note that AKS Kuberrnetes 1.15 support is in public preview, on Dec 9, 2019
    the supported *minor* Kubernetes versions will be 1.13, 1.14, 1.15
  * Azure Updates blog post with additional details will be published this week

* New Features
  * VMSS backed AKS clusters are now GA
    * VMSS is the underlying compute resource type which enables features such
      as cluster autoscaler, but is a separate feature.
    * See <https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview>
      and <https://docs.microsoft.com/azure/aks/cluster-autoscaler>
      for more information.
    * **NOTE**: Official support in the Azure CLI for AKS+VMSS will be released
      on 2019-09-24 (version 2.0.74 <https://github.com/Azure/azure-cli/milestone/73>)
  * Standard Load Balancer support (SLB) is now GA
    * See <https://docs.microsoft.com/azure/aks/load-balancer-standard>
      for documentation.
    * **NOTE**: Official support in the Azure CLI for AKS+SLB will be released
      on 2019-09-24 (version 2.0.74 <https://github.com/Azure/azure-cli/milestone/73>)
  * Support for the following VM SKUs is now released: Standard_D48_v3,
    Standard_D48s_v3, Standard_E48_v3, Standard_E48s_v3, Standard_F48s_v2,
    Standard_L48s_v2, Standard_M208ms_v2, Standard_M208s_v2
* Bug Fixes
  * CCP Fall back is not working as expected. This is because we updated CCP
    to turn on useCCPPool flag based on the toggle. But we did not refresh the useCCPPool flag after the change. So the flag is still false even though toggle changed it to true.
  * Fixed an issue where cluster upgrade could be blocked when the "managedBy"
    property is missing from the node resource group.
  * Fixed an issue where ingress controller network policy would block all
    egress traffic when assigned to pods (using label selectors).
* Behavioral Changes
  * Review the planned changes for new cluster creation defaults referenced in
    [Release 2019-08-26](https://github.com/Azure/AKS/releases/tag/2019-08-26)
* Preview Features
  * Fixed an issue where multiple nodepool clusters would use the incorrect
    version(s) and block upgrades.
  * Fixed an issue where AKS would incorrectly allow customers to specify
    different versions for multiple nodepools.
  * Fixed an issue where the incorrect node count would be returned or fail to
    update when using multiple node pools

## Release 2019-09-02

* Preview Features
  * Kubernetes 1.15 is now in Preview (1.15.3)

* Bug Fixes
  * A bug where kube-svc-redirect would crash due to an invalid bash line has been fixed.
  * A recent Kubernetes dashboard change to enable self-signed certs has been reverted due to browser issues.
  * A bug where the OMSAgent pod would fail scheduling on a user tainted node has been fixed with proper toleration on the OMSAgent pod.
  * A preview bug allowing more than 8 node pools to be created has been fixed to enforce a max of 8 node pools per cluster.
  * A preview bug that would change the primary node pool when adding a new node pool has been fixed.

* Behavioral Changes
  * Review the planned changes for new cluster creation defaults referenced in [Release 2019-08-26](https://github.com/Azure/AKS/releases/tag/2019-08-26)

* Component Updates
  * aks-engine has been updated to v0.40.0
    * <https://github.com/Azure/aks-engine/releases/tag/v0.40.0>

## Release 2019-08-26

**This release is rolling out to all regions**

* Features
  * Added prometheus annotation to coredns to facilitate metric port discovery
    * For more info please check: <https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-agent-config#overview-of-configurable-prometheus-scraping-settings>
* Bug Fixes
  * Fixed bug with older 1.8 clusters that was preventing clusters from upgrading.
    * **Important: this was a best effort fix since these cluster versions are out of support. Please upgrade to a currently supported version**
    * For information on how AKS handles Kubernetes version support see:
      [Supported Kubernetes versions in Azure](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions)
  * Removed the default restricted Pod Security Policy to solve race condition with containers not seeing the user in their config. This policy can be applied by customers.
  * Fixed a bug with kube-proxy, ip-masq-agent and kube-svc-redirect where in certain scenarios they could try to access iptables at the same time.
* Preview Features
  * CLI extension updated for new Standard Load Balancer (SLB) and VM Scale Set (VMSS) Parameters:
    * `--vm-set-type` Agent pool vm set type. VirtualMachineScaleSets or AvailabilitySet.
    * `--load-balancer-sku` - Azure Load Balancer SKU selection for your cluster. Basic or Standard.
    * `--load-balancer-outbound-ip-prefixes` - Comma-separated public IP prefix resource IDs for load balancer outbound connection. Valid for Standard SKU load balancer cluster only.
    * `--load-balancer-outbound-ips` - Comma-separated public IP resource IDs for load balancer outbound connection. Valid for Standard SKU load balancer cluster only.
    * `--load-balancer-managed-outbound-ip-count` - Desired number of automatically created and managed outbound IPs for load balancer outbound connection. Valid for Standard SKU load balancer cluster only.
* Behavioral Changes
  * Starting from 2019-09-10, the **preview CLI extension** will default new cluster creates to VM Scale-Sets and Standard Load Balancers (VMSS/SLB) instead of VM Availability Sets and Basic Load Balancers (VMAS/BLB).
  * Starting from 2019-10-22 the official CLI and Azure Portal will default new cluster creates to VMSS/SLB instead of VMAS/BLB.
  * These client defaults changes are important to be aware of due to:
    * SLB will automatically assign a public IP to enable egress. This is a requirement placed by Azure Standard Load Balancers, to learn more about Standard vs. Basic, read [here](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).
    * SLB enables bringing your own IP address to be used, you will be able to define these with new parameters.
    * The capability to use an SLB without any public IP assigned is on the roadmap plan.
    * You may still provision a basic load balancer by specifying "basic" for the "loadbalancersku" property at cluster create time.
    * Read more at <https://aka.ms/aks/slb>
* Component Updates
  * aks-engine has been updated to v0.39.2
    * <https://github.com/Azure/aks-engine/releases/tag/v0.39.2>
  * Azure Monitor for Containers Agent updated to 2019-08-22 release: <https://github.com/microsoft/Docker-Provider/releases>

## Release 2019-08-19 (Hotfix)

**This release is rolling out to all regions**

**Please Note**: This release includes new Kubernetes versions 1.13.10 &
1.14.6 these include the fixes for CVEs CVE-2019-9512 and
CVE-2019-9514. Please see our [customer guidance](https://github.com/Azure/AKS/issues/1159)

* Bug Fixes
  * New kubernetes versions released to fix CVE-2019-9512 and CVE-2019-9514
    * Kubernetes 1.14.6
    * Kubernetes 1.13.10
  * Fixed Azure Network Policy bug with multiple labels under a matchLabels selector.
  * Fix for CNI lock timeout issue caused due to race condition in starting telemetry process.
  * Fixed issue creating AKS clusters using supported Promo SKUs
* Component Updates
  * aks-engine has been updated to v0.38.8
    * <https://github.com/Azure/aks-engine/releases/tag/v0.38.8>
  * Azure CNI has been updated to v1.0.25

## Release 2019-08-12

**This release is rolling out to all regions**

* Bug Fixes
  * Several bug fixes for AKS NodePool creation and other CRUD operations.
  * Fixed audit log bug on older < 1.9.0 clusters.
    * **Important: this was a best effort fix since these cluster versions are out of support. Please upgrade to a currently supported version**
    * For information on how AKS handles Kubernetes version support see:
      [Supported Kubernetes versions in Azure](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions)
  * Improved error messaging for VM size errors, including actions to take.
  * Fixed for PUT request bug that caused an unexpected API restart.

* Behavioral Changes
  * AKS has released an API update, documentation available here: [https://docs.microsoft.com/rest/api/aks/managedclusters](https://docs.microsoft.com/rest/api/aks/managedclusters)
    * **Important:** With this API update there are changes to the API whitelisting API. This is now under [ManagedClusterAPIServerAccessProfile](https://docs.microsoft.com/rest/api/aks/managedclusters/createorupdate#managedclusterapiserveraccessprofile), where previously it was a top level property.

## Release 2019-08-05

**This release is rolling out to all regions**

**Please Note**: This release includes new Kubernetes versions 1.13.9 &
1.14.5 (GA today) these include the fixes for CVEs CVE-2019-11247 and
CVE-2019-11249. Please see our [customer guidance](https://github.com/Azure/AKS/issues/1145)

* New Features
  * Kubernetes 1.14 is now GA (1.14.5)
    * As of Monday August 12th (2019-08-12) customers running Kubernetes 1.10.x
      have 60 days (2019-10-14) to upgrade to a supported release. Please see
      [AKS supported versions document][1] for more information.
  * [Kubernetes Audit log](https://docs.microsoft.com/azure/aks/view-master-logs)
    support is now GA.
* Bug Fixes
  * Fixed an issue where creating a cluster with a custom subnet would return an
    HTTP error 500 vs 400 when the subnet could not be found.
* Behavioral Changes
* Preview Features
  * Fixed an issue where customers could not create a new node pool with AZs
    even if they were already using SLBs.
  * Fixed an issue where VMSS cluster commands could return the incorrect node
    count.
* Component Updates
  * aks-engine has been updated to v0.38.7
    * <https://github.com/Azure/aks-engine/releases/tag/v0.38.7>

## Release 2019-07-29

* New Features
  * Customers may now create multiple AKS clusters using ARM templates
    regardless of what region the clusters are located in.
* Bug Fixes
  * AKS has resolved the issue(s) with missing metrics in the default
    metrics blade.
  * An issue where the `--pod-max-pids` was set to 100 (maximum) for clusters
    and re-applied during upgrade causing `pthread_create() failed (11: Resource
    temporarily unavailable)` pod start failures was fixed.
    * See <https://github.com/Azure/aks-engine/pull/1623> for more information

* Preview Features
  * AKS is now in **Public Preview** in the Azure Government (Fairfax, VA)
    region. Please note the following:
    * Azure Portal support for AKS is in progress, for now customers must use the
      Azure CLI for all cluster operations currently.
    * AKS preview features are not supported in Azure Government currently and will
      be supported when those features are GA.
  * Fixed an issue where a delete request for a locked VMSS node would get an
    incorrect and unclear `InternalError` failure - the error message and error
    code have both been fixed.
  * Fixed an issue with egress filtering where managed AKS pods
    would incorrectly use the IP address to connect instead of the FQDN.
  * Fixed an issue with the SLB preview where AKS allowed the customer to
    provide an IP address already in use by another SLB.
  * An issue that prevented customers from using normal cluster operations
    on multiple node pool clusters with a single VMSS pool has been fixed.

* Component Updates
  * AKS-Engine has been updated to v0.38.4
    * <https://github.com/Azure/aks-engine/releases/tag/v0.38.4>

## Release 2019-07-22

* Preview Features
  * An issue where New Windows node pools in existing cluster would not get
    updated Windows versions has been fixed.
  * TCP reset has been set for all new clusters using the SLB preview.
  * An issue where AKS would trigger a scale operation requested on a previously
    deleted VMSS cluster has been fixed.
* Component Updates
  * AKS-Engine has been updated to v0.38.3

## Release 2019-07-15

**Important behavioral change**: All AKS clusters are being updated to pull all
needed container images for cluster operations from Azure Container Registry,
this means if you have custom allow/deny lists, port filtering, etc you will
need to update your network configuration to allow ACR.

[Please see the documentation](https://docs.microsoft.com/azure/aks/limit-egress-traffic#required-ports-and-addresses-for-aks-clusters) for more
information including all required AKS cluster ports and URLs

* New Features
  * Support for the M, NC_promo and DS_v3 Azure Compute VM SKUs has been added.
* Bug Fixes
  * Fixed an issue with clusters created in Canada and Australia regions between
    2019-07-09 and 2019-07-10 as well as US region clusters created on 2019-07-10
    where customers would receive `error: Changing property
    'platformFaultDomainCount' is not allowed` errors.

* Behavioral Changes
  * The error message returned to users when attempting to create clusters with
    an unsupported Kubernetes version in that region has been fixed.
  * Noted above, AKS has moved all container images required by AKS clusters for
    cluster CRUD operations have been moved to Azure Container Registry. This
    means that customers must update allow/deny rules and ports. See:
    [Required ports and addresses for AKS clusters](https://docs.microsoft.com/azure/aks/limit-egress-traffic#required-ports-and-addresses-for-aks-clusters)
* Preview Features
  * Fixed a VMSS cluster upgrade failure that would return: `Changing property
    'type' is not allowed.`
  * An issue where `az aks nodepool list` would return the incorrect node count
    has been resolved.
* Component Updates
  * The Azure Monitor for Container agent has been updated to the 2019-07-09 release
    * Please see the [release notes](https://github.com/Microsoft/docker-provider/tree/ci_feature_prod#07092019--).

## Release 2019-07-08

* New Features
  * Kubernetes versions 1.11.10 and 1.13.7 have been added. Customers
    are encouraged to upgrade.
    * For information on how AKS handles Kubernetes version support see:
      [Supported Kubernetes versions in Azure](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions))
  * The `az aks update-credentials` command now supports Azure tenant migration of your
AKS cluster. Follow the instructions in [Choose to update or create a service principal][8] and then execute the `Update AKS cluster with new credentials` command passing in the `--tenant-id` argument.
* Behavioral Changes
  * All new clusters now have --protect-kernel-defaults enabled.
    * See: <https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/>
* Preview Features
  * Kubernetes 1.14.3 is now available for preview users.
  * Azure availability zone support is now in public preview.
    * This feature enables customers to distribute their AKS clusters across
      availability zones providing a higher level of availability.
    * Please see [AKS previews][previews] for additional information.
  * For all previews, please see the [previews][previews] document for opt-in
    instructions and documentation links.
* Component Updates
  * aks-engine has been updated to version 0.37.5
    * <https://github.com/Azure/aks-engine/releases/tag/v0.37.5>
  * Azure CNI has been updated to version 1.0.22
  * Moby has been updated to 3.0.5 from 3.0.4
    * Note that this version number is Azure specific, the Moby project does not
      have official releases / release numbers.

## Release 2019-07-01

* Bug Fixes
  * Fixed an issue with `az aks update-credentials` where the command would
    not take special characters and nodes would get incorrect values.
    Note that double quote `"` , backslash `\`, ampersand `&`, and angle quotations `<>`
    are still NOT allowed to be used as password characters.
  * Fixed an issue with update-credentials where the command would not work for VMSS clusters
    with more than 10 instances.
  * AKS now has validation to check for Resource Locks when performing Scale and Upgrade operations.
  * Fixed an issue where GPU nodes could fail to install the GPU driver due to ongoing
    background apt operations.
  * Adjusted the timeout value for Service Principal update based on the number of nodes in the
    cluster, to accommodate larger clusters.
* New Features
  * AKS now supports OS disk sizes of up to 2048GiB.
  * Persistent Tags
    * Custom tags can now be passed to AKS and will persisted onto the MC infrastructure Resource Group.
      Note: They will NOT be applied to all child resources in that RG, aka VMs, VNets, disks, etc.
* Preview Features
  * Windows Node Pools
    * AKS updated Windows default image to latest windows patch release.
  * API server authorized IP ranges
    * The max number of API server authorized IP ranges has now increased to 100.
* Component Updates
  * [AKS-Engine has been updated to v0.35.6](https://github.com/Azure/aks-engine/releases/tag/v0.35.6)
    * This change includes a new AKS VHD with the Linux Kernel CVE fixes. See more:
      <https://github.com/Azure/AKS/issues/>
    * This new VHD also fixes broken IPv6 support for the host.

## Release 2019-06-24

* Bug Fixes
  * Fixed an issue that could result in a failed service principal update and
    AKS cluster creation.
  * Fixed an issue where deploying AKS clusters using ARM templates without a
    defined Service Principal would incorrectly pass validation.
* Preview Features
  * Azure Standard load balancer support is now in public preview.
    * This has been a long awaited feature which enables selection of the SKU
      type offered by Azure Load Balancer to be used with your AKS cluster. Please see
      [AKS previews][previews] for additional information.
  * For all previews, please see the [previews][previews] document for opt-in
    instructions and documentation links.
* Component Updates
  * The Azure Monitor for Container agent has been updated to the 2019-06-14 release
    * Please see the [release notes](https://github.com/Microsoft/docker-provider/tree/ci_feature_prod#06142019--).

## Release 2019-06-18

* Behavioral Changes
  * **Important: Change in UDR and subnet behavior**
    * When using Kubenet with a custom subnet, AKS now checks if there is an
      existing associated route table.
    * If that is the case AKS will NOT attach the kubenet RT/Routes automatically
      and they should be added manually to the existing RT.
    * If no Route Table exists AKS will automatically attach the kubenet RT/Routes.

* Preview Features
  * A bug where users could not scale VMSS based clusters after disabling the
   cluster autoscaler has been fixed.
  * A missing CRD for calico-enabled clusters (#1042) has been fixed.

## Release 2019-06-10

* Bug Fixes
  * Kubernetes taints and tolerations are now supported in all AKS regions.
    * Taints & Tolerations are preserved for current cluster nodes and
      through upgrades, however they are *not* preserved through scale (up,
      down) operations.

* Preview Features
  * A bug that prevented cluster agent pool deletions due to VMSS creation
    failures has been fixed.
  * A bug preventing the cluster autoscaler from working with nodepool enabled
    clusters (one or more nodepools) has been fixed.
  * A bug where the NSG would not be reset as needed during a nodepool create
    request has been fixed.

* Behavioral Changes
  * AKS removed all weak CBC suite ciphers for API server. More info: <https://blog.qualys.com/technology/2019/04/22/zombie-poodle-and-goldendoodle-vulnerabilities>

* Component Updates
  * AKS-Engine has been updated to v0.35.4

## Release 2019-05-28

* New Features
  * AKS is now available in both China East 2 / China North 2 Azure Regions.
  * AKS is now available in South Africa North
  * The L and M series Virtual Machines are now supported

* Component Updates
  * AKS-Engine has been updated to version 0.35.3
  * CoreDNS has been upgraded from 1.2.2 to version 1.2.6

* Preview Features
  * A bug where users could not deleted an agent pool containing VMSS nodes if
    the VMSS node creation fails has been fixed.

## Release 2019-05-20

* Behavioral Changes
  * The 192.0.2.0/24 IP block is now reserved for AKS use. Clusters created in
    a VNet that overlaps with this block will fail pre-flight validation.
* Bug Fixes
  * An issue where users running old AKS clusters attempting to upgrade would
    get a failed upgrade with an Internal Server Error has been fixed.
  * An issue where Kubernetes 1.14.0 would not show in the Azure Portal or AKS
    Preview CLI with the 'Preview' or 'isPreview' tag has been resolved.
  * An issue where customers would get excessive log entries due to missing
    Heapster rbac permissions has been fixed.
    * <https://github.com/Azure/AKS/issues/520>
  * An issue where AKS clusters could end up with missing DNS entries resulting
    in DNS resolution errors or crashes within CoreDNS has been resolved.

* Preview Features
  * A bug where the AKS node count could be out of sync with the VMSS node count
    has been resolved.
  * There is a known issue with the cluster autoscaler preview and multiple
    agent pools. The current autoscaler in preview is not compatible with
    multiple agent pools, and could not be disabled. We have fixed the issue
    that blocked disabling the autoscaler. A fix for multiple agent pools and
    the cluster autoscaler is in development.

## Release 2019-05-17 (Announcement)

* Window node support for AKS is now in Public Preview
  * Blog post: <https://aka.ms/aks/windows>
  * Support and documentation:
    * Documentation: <https://aka.ms/aks/windowsdocs>
    * Issues may be filed on this Github repository (<https://github.com/Azure/AKS>)
      or raised as a Sev C support request. Support requests and issues for
      preview features do not have an SLA / SLO and are best-effort only.
  * **Do not enable preview featured on production subscriptions or clusters.**
  * For all previews, please see the [previews][previews] document for opt-in
    instructions and documentation links.

* Bug fixes
  * An issue impacting Java workloads where pods running Java workloads would
    consume all available node resources instead of the defined pod resource
    limits defined by the user has been resolved.
    * <https://bugs.openjdk.java.net/browse/JDK-8217766>
    * AKS-Engine PR for fix: <https://github.com/Azure/aks-engine/pull/1095>

* Component Updates
  * AKS-Engine has been updates to v0.35.1

## Release 2019-05-13

* New Features
  * Shared Subnets are now supported with Azure CNI.
    * Users may bring / provide their own subnets to AKS clusters
    * Subnets are no longer restricted to a single subnet per AKS cluster, users
      may now have multiple AKS clusters on a subnet.
    * If the subnet provided to AKS has NSGs, those NSGs will be preserved and
      used.
      * **Warning**: NSGs must respect: <https://aka.ms/aksegress> or the
      cluster might not come up or work properly.
    * Note: Shared subnet support is not supported with VMSS (in preview)
* Bug Fixes
  * A bug that blocked Azure CNI users from setting maxPods above 110 (maximum
    of 250) and that blocked existing clusters from scaling up when the value
    was over 110 for CNI has been fixed.
  * A validation bug blocking long DNS names used by customers has been fixed.
    For restrictions on DNS/Cluster names, please see
    <https://aka.ms/aks-naming-rules>

## Release 2019-05-06

* New Features
  * Kubernetes Network Policies are GA
    * See <https://docs.microsoft.com/azure/aks/use-network-policies>
      for documentation.

* Bug Fixes
  * An issues customers reported with CoreDNS entering CrashLoopBackoff has
    been fixed. This was related to the upstream move to `klog`
    * <https://github.com/coredns/coredns/pull/2529>
  * An issue where AKS managed pods (within kube-system) did not have the correct
    tolerations preventing them from being scheduled when customers use
    taints/tolerations has been fixed.
  * An issue with kube-dns crashing on specific config map override scenarios
    as seen in <https://github.com/Azure/acs-engine/issues/3534> has been
    resolved by updating to the latest upstream kube-dns release.
  * An issue where customers could experience longer than normal create times
    for clusters tied to a blocking wait on heapster pods has been resolved.
* Preview Features
  * New features in public preview:
    * Secure access to the API server using authorized IP address ranges
    * Locked down egress traffic
      * This feature allows users to limit / whitelist the hosts used by AKS
        clusters.
    * Multiple Node Pools
    * For all previews, please see the [previews][previews] document for opt-in
      instructions and documentation links.

## Release 2019-04-22

* Kubernetes 1.14 is now in Preview
  * Do not use this for production clusters. This version is for early adopters
    and advanced users to test and validate.
  * Accessing the Kubernetes 1.14 release requires the `aks-preview` CLI
    extension to be installed.

* New Features
  * Users are no longer forced to create / pre-provision subnets when using
    Advanced networking. Instead, if you choose advanced networking and do not
    supply a subnet, AKS will create one on your behalf.

* Bug fixes
  * An issue where AKS / the Azure CLI would ignore the `--network-plugin=azure`
    option silently and create clusters with Kubenet has been resolved.
    * Specifically, there was a bug in the cluster creation workflow where users
      would specific `--network-plugin=azure` with Azure CNI / Advanced Networking
      but miss passing in the additional options (eg '--pod-cidr, --service-cidr,
      etc). If this occurred, the service would fall-back and create the cluster
      with Kubenet instead.

* Preview Features
  * Kubernetes 1.14 is now in Preview
  * An issue with Network Policy and Calico where cluster creation could
    fail/time out and pods would enter a crashloop has been fixed.
    * <https://github.com/Azure/AKS/issues/905>
    * Note, in order to get the fix properly applied, you should create a new
      cluster based on this release, or upgrade your existing cluster and then
      run the following clean up command after the upgrade is complete:

```
kubectl delete -f https://github.com/Azure/aks-engine/raw/master/docs/topics/calico-3.3.1-cleanup-after-upgrade.yaml
```

* Component Updates
  * Azure Monitoring for Containers has been updated to the 2019-04-23 release
    * For more information, please see: <https://github.com/Microsoft/docker-provider/tree/ci_feature_prod#04232019>--

## Release 2019-04-15

* Kubernetes 1.13 is GA
* **The Kubernetes 1.9.x releases are now deprecated.** All clusters
  on version 1.9 must be upgraded to a later release (1.10, 1.11, 1.12, 1.13)
  within **30 days**. Clusters still on 1.9.x after 30 days (2019-05-25)
  will no longer be supported.
  * During the deprecation period, 1.9.x will continue to appear in the available
    versions list. Once deprecation is completed 1.9 will be removed.

* (Region) North Central US is now available
* (Region) Japan West is now available

* New Features
  * Customers may now provide custom Resource Group names.
    * This means that users are no longer locked into the MC_* resource name
      group. On cluster creation you may pass in a custom RG and AKS will
      inherit that RG, permissions and attach AKS resources to the customer
      provided resource group.
    * Currently, you must pass in a new RG (resource group) must be new, and
      can not be a pre-existing RG. We are working on support for pre-existing
      RGs.
    * This change requires newly provisioned clusters, existing clusters can
      not be migrated to support this new capability. Cluster migration across
      subscriptions and RGs is not currently supported.
  * AKS now properly associates existing route tables created by AKS when
      passing in custom VNET for Kubenet/Basic Networking. *This does not
      support User Defined / Custom routes (UDRs)*.

* Bug fixes
  * An issue where two delete operations could be issued against a cluster
    simultaneously resulting in an unknown and unrecoverable state has been
    resolved.
  * An issue where users could create a new AKS cluster and set the `maxPods`
    value too low has been resolved.
    * Users have reported cluster crashes, unavailability and other issues
      when changing this setting. As AKS is a managed service, we provide
      sidecars and pods we deploy and manage as part of the cluster. However
      users could define a maxPods value lower than the value required for the
      managed pods to run (eg 30), AKS now calculates the minimum number of
      pods via: `maxPods or maxPods * vm_count > managed add-on pods`

* Behavioral Changes
  *AKS cluster creation now properly pre-checks the assigned service CIDR
    range to block against possible conflicts with the dns-service CIDR.
    * As an example, a user could use 10.2.0.1/24 instead of 10.2.0.0/24 which
      would lead to IP conflicts. This is now validated/checked and if there is
      a conflict, a clear error is returned.
  * AKS now correctly blocks/validates users who accidentally attempt an
    upgrade to a previous release (eg downgrade).
  * AKS now validate all CRUD operations to confirm the requested action will
    not fail due to IP Address/subnet exhaustion. If a call is made that would
    exceed available addresses, the service correctly returns an error.
  * The amount of memory allocated to the Kubernetes Dashboard has been
    increased to 500Mi for customers with large numbers of nodes/jobs/objects.
  * Small VM SKUs (such as Standard F1, and A2) that do not have enough RAM to
    support the Kubernetes control plane components have been removed from the
    list of available VMs users can use when creating AKS clusters.

* Preview Features
  * A bug where Calico pods would not start after a 1.11 to 1.12 upgrade has
    been resolved.
  * When using network policies and Calico, AKS now properly uses Azure CNI for
    all routing vs defaulting to using Calico the routing plugin.
  * Calico has been updated to v3.5.0

* Component Updates
  * AKS-Engine has been updates to v0.33.4

## Release 2019-04-01

* Bug Fixes
  * Resolved an issue preventing some users from leveraging the Live Container
    Logs feature (due to a 401 unauthorized).
  * Resolved an issue where users could get "Failed to get list of supported
    orchestrators" during upgrade calls.
  * Resolved an issue where users using custom subnets/routes/networking with
    AKS where IP ranges match the cluster/service or node IPs could result in
    an inability to `exec`, get cluster logs (`kubectl get logs`) or otherwise
    pass required health checks.
  * An issue where a user running `az aks get-credentials` while a cluster is
    in creation resulting in an unclear error ('Could not find role name') has
    been resolved.

## Release 2019-04-08 (Hotfix)

This release fixes one AKS product regression and an issue identified with the
Azure Jenkins plugin.

* A regression when using ARM templates to issue AKS cluster update(s) (such as
  configuration changes) that also impacted the Azure Portal has been fixed.
  * Users do not need to perform any actions / upgrades for this fix.
* An issue when using the Azure Container Jenkins plugin with AKS has been
  mitigated.
  * This issue caused errors and failures when using the Jenkins plugin - the
    bug triggered by a new AKS API version but was related to a latent issue in
    the plugin's API detection behavior.
  * An updated Jenkins plugin has been published:
    <https://github.com/jenkinsci/azure-acs-plugin/issues/16>
  * <https://github.com/jenkinsci/azure-acs-plugin/releases/tag/azure-acs-0.2.4>

## Release 2019-04-04 - Hotfix (CVE mitigation)

* Bug fixes
  * New kubernetes versions released with multiple CVE mitigations
    * Kubernetes 1.12.7
      * <https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.12.md#changelog-since-v1126>
    * Kubernetes 1.11.9
      * <https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.11.md#changelog-since-v1118>
    * Customers should upgrade to the latest 1.11 and 1.12 releases.
      * Kubernetes versions prior to 1.11 must upgrade to 1.11/1.12 for the fix.

* Component updates
  * Updated included AKS-Engine version to 0.33.2
    * See: <https://github.com/Azure/aks-engine/releases/tag/v0.33.4> for details

## Release 2019-03-29 (Hotfix)

* The following regions are now GA: South Central US, Korea Central and Korea
  South

* Bug fixes
  * Fixed an issue which prevented Kubernetes addons from being disabled.

* Behavioral Changes
  * AKS will now block subsequent PUT requests (with a status code 409 -
    Conflict) while an ongoing operation is being performed.

## Release 2019-03-21

* The Central India region is now GA

* Bug fixes
  * AKS will now begin preserving node labels & annotations users apply to
    clusters during upgrades.
    * Note: labels & annotations will not be applied to new nodes added during
      a scale up operation.
  * AKS now properly validates the Service Principal / Azure Active Directory
    (AAD) credentials
    * This prevents invalid, expired or otherwise broken credentials being
      inserted and causing cluster issues.
  * Clusters that enter a failed state due to upgrade issues will now allow
    users to re-attempt to upgrade or will throw an error message with
    instructions to the user.
  * Fixed an issue with cloud-init and the walinuxagent resulting in
    `failed state` VMs/worker nodes
  * The `tenant-id` is now correctly defaulted if not passed in for AAD enabled
    clusters.

* Behavioral Changes
  * AKS is now pre-validating MC_* resource group locks before any CRUD
    operation, avoiding the cluster enter Failed state.
  * Scale up/down calls now return a correct error ('Bad Request') when users
    delete underlying virtual machines during the scale operation.
  * Performance Improvement: caching is now set to read only for data disks
  * The Nvidia driver has been updated to 410.79 for N series cluster
    configurations
  * The default worker node disk size has been increased to 100GB
    * This resolves customer reported issues with large numbers (and large
      sizes) of Docker images triggering out of disk issues and possible
      workload eviction.
  * The Kubernetes controller manager `terminated-pod-gc-threshold` has been
    lowered to 6000 (previously 12500)
    * This will help system performance for customers running large number of
      Jobs (finished pods)
  * The Azure Monitor for Container agent has been updated to the 2019-03
    release
  * The "View Kubernetes Dashboard" has been removed from the Azure Portal
    * Note that this button did not expose/add functionality, it only linked to
      the existing instructions for using the Kubernetes dashboard found here:
      <https://docs.microsoft.com/azure/aks/kubernetes-dashboard>

## Release 2019-03-07

* The Azure Monitor for containers Agent has been updated to 3.0.0-4 for newly
  built or upgraded clusters
* The Azure CLI now properly defaults to N-1 for Kubernetes versions, for
  example N is the current latest (1.12) release - the CLI will correctly pick
  1.11.x. When 1.13 is released, the default will move to 1.12.

* Bug Fixes:
  * If a user exceeds quota during a scale operation, the Azure CLI will now
    correctly display a "Quota exceeded" vs "deployment not found"
  * All AKS CRUD (put) operations now validate and confirm user subscriptions
    have the needed quota to perform the operation. If a user does not, an
    error is correctly shown and the operation will not take effect.
  * All AKS issued Kubernetes SSL certificates have had weak cipher support
    removed, all certificates should now pass security audits for BEAST and
    other vulnerabilities.
    * If you are using older clients that do not support TLS 1.2 you will need
      to upgrade those clients and associated SSL libraries to securely connect.
      * Note that only Kubernetes 1.10 and above support the new certificates,
        additionally existing certificates will not be updated as this would
        revoke all user access. To get the updated certificates you will need
        to create a new AKS cluster.
  * Clusters that are in the process of upgrading or in failed upgrade state
    will attempt to re-execute the upgrade or throw an obvious error message.
* The preview feature for Calico/Network Security Policies has been updated to
  repair a bug where ip-forwarding was not enabled by default.
* The `cachingmode: ReadOnly` flag was not always being correctly applied to
  the managed premium storage class, this has been resolved.

## Release 2019-03-01

* New kubernetes versions released for CVE-2019-1002100 mitigation
  * Kubernetes 1.12.6
  * Kubernetes 1.11.8
  * Customers should upgrade to the latest 1.11 and 1.12 releases.
  * Kubernetes versions prior to 1.11 must upgrade to 1.11/1.12 for the fix.
    * Announcement here: <https://groups.google.com/forum/#!msg/kubernetes-announce/vmUUNkYfG9g/B9rHFrqLCAAJ>
* A security bug with the Kubernetes dashboard and overly permissive service
  account access has been fixed
* The France Central region is now GA for all customers
* Bug fixes and performance improvements

## Release 2019-02-19

* Fixed a bug in cluster location/region validation has been resolved.
  * Previously, if you passed in a location/region with a trailing unicode
    non-breaking space (U+00A0) would cause failures on CRUD operations or
    cause other non-parseable characters to be displayed.
* Fixed a bug where if the dnsService IP conflicts with the apiServer IP
  address(es) creates or updates would fail after the fact.
  * Addresses are now checked to ensure no overlap or conflict at CRUD operation
    time.
* The Australia Southeast region is now GA
* Fixed a bug when using the new Service Principal rotation/update command on
  cluster nodes using the Azure CLI would fail
  * Specifically, there was a missing dependency (e.g. `jq is missing`) on the
    nodes, all new nodes should now contain the `jq` utility.

## Release 2019-02-12 - Hotfix Release (UPDATE)

At this time, all regions now have the CVE hotfix release. The simplest way to
consume it is to perform a Kubernetes version upgrade, which will cordon, drain,
and replace all nodes with a new base image that includes the patched version of
Moby. In conjunction with this release, we have enabled new patch versions for
Kubernetes 1.11 and 1.12. However, as there are no new patch versions available
for Kubernetes versions 1.9 and 1.10, customers are recommended to move forward
to a later minor release.

If that is not possible and you must remain on 1.9.x/1.10.x, you can perform
the following steps to get the patched runtime:

1. Scale *up* your existing 1.9/1.10 cluster - add an equal number of nodes to
  your existing worker count.
1. After scale-up completes, pick a single node and using the kubectl command,
  cordon the old node, drain all traffic from it, and then delete it.
1. Repeat step 2 for each worker in your cluster, until only the new nodes
  remain.

Once this is complete, all nodes should reflect the new Moby runtime version.

We apologize for the confusion, and we recognize that this process is not ideal
and we have future plans to enable an upgrade strategy that decouples system
components like the container runtime from the Kubernetes version.

Note: All newly created 1.9, 1.10, 1.11 and 1.12 clusters will have the new
Moby runtime and will not need to be upgraded to get the patch.

### Release 2019-02-12 - Hotfix Release

**Hotfix releases follow an accelerated rollout schedule - this release should
be in all regions by 12am PST 2019-02-13**

* Kubernetes 1.12.5, 1.11.7
* This release mitigates CVE-2019-5736 for Azure Kubernetes Service (see below).
  * Please note that GPU-based nodes do not support the new container runtime
    yet. We will provide another service update once a fix is available for
    those nodes.

**CVE-2019-5736 notes and mitigation**
Microsoft has built a new version of the Moby container runtime that includes
the OCI update to address this vulnerability. In order to consume the updated
container runtime release, you will need to **upgrade your Kubernetes cluster**.

Any upgrade will suffice as it will ensure that all existing nodes are removed
and replaced with new nodes that include the patched runtime. You can see the
upgrade paths/versions available to you by running the following command with
the Azure CLI:

```
az aks get-upgrades -n myClusterName -g myResourceGroup
```

To upgrade to a given version, run the following command:

```
az aks upgrade -n myClusterName -g myResourceGroup -k <new Kubernetes version>
```

You can also upgrade from the Azure portal.

When the upgrade is complete, you can verify that you are patched by running
the following command:

```
kubectl get nodes -o wide
```

If all of the nodes list **docker://3.0.4** in the Container Runtime column,
you have successfully upgraded to the new release.

## Release 2019-02-07 - Hotfix Release

This hotfix release fixes the root-cause of several bugs / regressions
introduced in the 2019-01-31 release. This release does not add new features,
functionality or other improvements.

**Hotfix releases follow an accelerated rollout schedule - this release should
be in all regions within 24-48 hours barring unforeseen issues**

* Fix for the API regression introduced by removing the Get Access Profile API
  call.
  * Note: This call is planned to be deprecated, however we will issue advance
    communications and provide the required logging/warnings on the API call to
    reflect it's deprecating status.
  * Resolves [Issue 809](https://github.com/Azure/AKS/issues/809)
* Fix for CoreDNS / kube-dns autoscaler conflict(s) leading to both running in
  the same cluster post-upgrade
  * Resolves [Issue 812](https://github.com/Azure/AKS/issues/812)
* Fix to enable the CoreDNS customization / compatibility with kube-dns config
  maps
  * Resolves [Issue 811](https://github.com/Azure/AKS/issues/811)
  * Note: customization of Kube-dns via the config map method was technically
    unsupported, however the AKS team understands the need and has created a
    compatible work around (formatting of the customizations has changed
    however). Please see the example/notes below for usage.

### Using the new CoreDNS configuration for DNS configuration

With kube-dns, there was an undocumented feature where it supported two config
maps allowing users to perform DNS overrides/stub domains, and other
customizations. With the conversion to CoreDNS, this functionality was lost -
CoreDNS only supports a single config map. With the hotfix above, AKS now has a
work around to meet the same level of customization.

You can see the pre-CoreDNS conversion customization instructions [here][7]

Here is the equivalent ConfigMap for CoreDNS:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  azurestack.server: |
    azurestack.local:53 {
        errors
        cache 30
        proxy . 172.16.0.4
    }
```

After create the config map, you will need to delete the CoreDNS deployment to
force-load the new config.

```
kubectl -n kube-system delete po -l k8s-app=kube-dns
```

## Release 2019-01-31

* [Kubernetes 1.12.4 GA Release][1]
  * With the release of 1.12.4 *Kubernetes 1.8 support has been removed*, you
    will need to upgrade to at least 1.9.x
* CoreDNS support GA release
  * Conversion from kube-dns to CoreDNS completed, CoreDNS is the default for
    all new 1.12.4+ AKS clusters.
  * If you are using configmaps or other tools for kube-dns modifications, you
    will need to be adjust them to be CoreDNS compatible.
    * The CoreDNS add-on is set to `reconcile` which means modifications to the
      deployments will be discarded.
    * We have identified two issues with this release that will be resolved in
      a hot fix beginning rollout this week:
      * <https://github.com/Azure/AKS/issues/811> (kube-dns config map not
        compatible with CoreDNS)
      * <https://github.com/Azure/AKS/issues/812> (kube-dns/coreDNS autoscaler
        conflicts)
* Kube-dns (pre 1.12) / CoreDNS (1.12+) autoscaler(s) are enabled by default,
  this should resolve the DNS timeout and other issues related to DNS queries
  overloading kube-dns.
  * In order to get the dns-autoscaler, you must perform an **AKS cluster
    upgrade** to a later supported release (clusters prior to 1.12 will
    continue to get kube-dns, with kube-dns autoscale)
* Users may now self update/rotate Security Principal credentials using the
  [Azure CLI][6]
* Additional non-user facing stability and reliability service enhancements
* **New Features in Preview**
  * **Note**: Features in preview are considered beta/non-production ready and
    unsupported. Please do not enable these features on production AKS clusters.
  * [Cluster Autoscaler / Virtual machine Scale Sets][2]
  * [Kubernetes Audit Log][3]
  * Network Policies/Network Security Policies
    * This means you can now use `calico` as a valid entry in addition to
      `azure` when creating clusters using Advanced Networking
    * There is a known issue when using Network Policies/calico that prevents
      `exec` into the cluster containers which will be fixed in the next release
  * For all product / feature previews including related projects, see
    [this document][5].

[1]: https://docs.microsoft.com/azure/aks/supported-kubernetes-versions
[2]: https://docs.microsoft.com/azure/aks/cluster-autoscaler#create-an-aks-cluster-and-enable-the-cluster-autoscaler
[3]: https://github.com/Azure/AKS/blob/master/previews.md#kubernetes-audit-log
[5]: https://github.com/Azure/AKS/blob/master/previews.md
[6]: https://docs.microsoft.com/azure/aks/update-credentials
[7]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[8]: https://docs.microsoft.com/azure/aks/update-credentials

[previews]: https://github.com/Azure/AKS/blob/master/previews.md

