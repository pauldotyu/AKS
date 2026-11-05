---
title: "Build the platform, not the plumbing, on AKS"
date: 2026-11-09
description: "Why platform engineers should build internal developer platforms on AKS: let Azure run the undifferentiated work so your team can own the developer contract."
authors: [paul-yu]
tags:
  [
    platform-engineering,
    aks-automatic,
    best-practices,
    developer,
    workload-identity,
    argo-cd,
    open-source,
    kro,
  ]
draft: true
---

Running one app on Azure Kubernetes Service (AKS) is easy. Running a platform that hosts that app, the next one, and every one after it is the hard part. Without one, every team ends up wiring the same pieces together on its own.

Platform conversations often start with a tool list. Backstage or something else? Argo CD or Flux? Tools matter, but they should follow the contract you give your developers, not define it. In this post, I'll walk through why I think AKS is a great foundation for that contract, how to pick tools against it, and the trade-offs to keep in mind.

<!-- truncate -->

![Four-layer diagram: developers use their namespace, pull requests, AKS desktop, and Copilot CLI. Platform teams choose open source tools. AKS Automatic manages Argo CD, workload identity, RBAC, Deployment Safeguards, and app routing. Azure services connect through identity.](./building-on-azure-foundation.svg)

## Start with the contract

Every app needs the same things before it can ship, no matter which team owns it:

1. **A place to run** with clear boundaries
2. **An identity** to reach other services without copying keys around
3. **A way to see what it's doing**, not just whether it's running
4. **A safe way to ship**: review first, then reconcile

Kubernetes gives you good building blocks for all of these. Making them work together, and keeping them working through upgrades and CVEs, is the real work. A platform turns them into one contract every team can depend on. That doesn't mean every app looks the same. It means the same capabilities are always there.

## Map each promise to who runs it

Think of the contract as a set of promises to your developers. You can build everything underneath them yourself, but each piece you assemble by hand is another thing to patch, debug, and staff. Here's where AKS has a managed default and where you'd bring your own:

| Promise to developers                                   | AKS default                                                                                                                                                                                                                                                                                                      | Bring your own                                                                                                                           |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Your app gets its own boundary                          | [Managed namespaces](https://learn.microsoft.com/azure/aks/concepts-managed-namespaces) with quota, network policy, and RBAC                                                                                                                                                                                     | Namespaces you template yourself                                                                                                         |
| You can request an environment                          | Not built in                                                                                                                                                                                                                                                                                                     | Crossplane with OpenTofu, or [kro](https://kro.run) with [Azure Service Operator (ASO)](https://azure.github.io/azure-service-operator/) |
| Capacity follows demand                                 | [Node autoprovisioning](https://learn.microsoft.com/azure/aks/node-auto-provisioning), HPA, and KEDA                                                                                                                                                                                                             | Cluster autoscaler or self-managed Karpenter                                                                                             |
| Your app reaches Azure services without a stored secret | Workload identity, plus the [Key Vault provider for Secrets Store CSI Driver](https://learn.microsoft.com/azure/aks/csi-secrets-store-driver) or the [App Configuration Kubernetes provider](https://learn.microsoft.com/azure/azure-app-configuration/quickstart-azure-kubernetes-service) when you do need one | External Secrets                                                                                                                         |
| Your app gets an HTTPS URL                              | [Application routing with the Gateway API](https://learn.microsoft.com/azure/aks/app-routing-gateway-api)                                                                                                                                                                                                        | Your own Gateway controller with cert-manager                                                                                            |
| Only trusted images run                                 | [Azure Container Registry](https://learn.microsoft.com/azure/container-registry/container-registry-intro) with [Defender for Containers](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-containers-introduction) scanning                                                                     | Trivy, Notation, and Ratify                                                                                                              |
| A rejected change tells you how to fix it               | [Deployment Safeguards](https://learn.microsoft.com/azure/aks/deployment-safeguards) through Azure Policy                                                                                                                                                                                                        | Kyverno                                                                                                                                  |
| Changes ship through review                             | [Argo CD](https://learn.microsoft.com/azure/azure-arc/kubernetes/tutorial-use-gitops-argocd) or [Flux](https://learn.microsoft.com/azure/azure-arc/kubernetes/tutorial-use-gitops-flux2) extension                                                                                                               | Self-managed Argo CD or Flux                                                                                                             |
| You can see what your app is doing                      | Managed Prometheus, Container Insights, and Application Insights                                                                                                                                                                                                                                                 | Prometheus, Grafana, and the OpenTelemetry Collector                                                                                     |
| Your team knows what it spends                          | [AKS cost analysis](https://learn.microsoft.com/azure/aks/cost-analysis) per namespace                                                                                                                                                                                                                           | OpenCost                                                                                                                                 |
| The environment stays patched                           | Automatic cluster and node OS upgrades in [planned maintenance windows](https://learn.microsoft.com/azure/aks/planned-maintenance)                                                                                                                                                                               | Your own upgrade pipeline                                                                                                                |
| You can find what's available                           | Not built in                                                                                                                                                                                                                                                                                                     | Backstage                                                                                                                                |

If you've seen the [BACK stack](https://github.com/crossplane-contrib/back-stack) (Backstage, Argo CD, Crossplane, and Kyverno), every piece of it shows up in the right column. Either column keeps the promise. The difference is who runs it. Most rows come preconfigured on [AKS Automatic](https://learn.microsoft.com/azure/aks/intro-aks-automatic), and the two "not built in" rows are where your platform team adds the most value.

:::note
kro + ASO is a good option for Azure but it is still pre-1.0, so expect API changes and pin your version. For background, see [Building community with CRDs: Kube Resource Orchestrator](/2025/01/30/kube-resource-orchestrator).
:::

For me, identity is the big one. My motto has always been "ditch the passwords." When apps use workload identity, the platform stores no secrets for Azure services, and access gets granted per component. Not every dependency supports Microsoft Entra authentication yet, but the more that do, the less secret rotation you own.

## Start smaller than you think

Every tool in the right column is a system you run. Backstage is a framework you build and upgrade, not a product you install, and writing and debugging Crossplane compositions takes practice.

Your first platform can be one AKS Automatic cluster, a pull request template, one Helm chart, and a GitOps extension. Add a self-service API when requests start piling up, and add a portal when discovery becomes the bottleneck.

Splitting ownership stays simple too. Developers own their app config and image. The platform team owns the contract, the templates, and delivery. AKS owns the rest.

## Split clusters when you outgrow one

Once you add a self-service API, the pattern I recommend is: **run the platform where you want control, and run tenant workloads where you want guardrails by default.**

- **Control cluster on AKS Standard** runs Crossplane or kro and your GitOps controller. It's small, predictable, and owned by the platform team, so pick exactly what you need: a fixed node count, the `patch` upgrade channel, local accounts disabled, and a private API server.
- **Workload cluster on AKS Automatic** runs developer apps. Every namespace starts from a hardened baseline, and nodes show up when workloads need them.

The control cluster holds the credentials that create infrastructure, so treat it as your most sensitive cluster. Scope its workload identity to the resource groups it manages, and keep tenant workloads off it. Managed namespaces are Azure resources, so the control cluster creates them through Azure Resource Manager instead of holding a kubeconfig for the workload cluster.

On a shared workload cluster, a few things still need your attention:

- **Namespaces are a soft boundary.** Teams still share the node kernel, the API server, CRDs, and the upgrade schedule. When a team needs a different Kubernetes version, its own CRDs, or stronger isolation for compliance, give it its own cluster with the same contract.
- **Upgrades affect every tenant.** Set maintenance windows, require PodDisruptionBudgets in your templates, and let teams know before an upgrade lands.
- **Networking is its own post.** Private clusters, egress control, and hub-and-spoke designs all shape the contract, but they deserve more room than I can give them here.

## Hand your developers the right tools

The contract only works if developers can use it without learning your plumbing. If the supported path isn't the easiest path, developers will go around you. Use whatever your developers already have, but these are where I'd start:

- **[GitHub Copilot CLI](https://github.com/features/copilot/cli) with [Containerization Assist MCP](https://github.com/Azure/containerization-assist)** to get an app ready to ship. It analyzes a repo, then generates and validates a Dockerfile and Kubernetes manifests with probes, resource settings, and security settings.
- **[AKS desktop](https://learn.microsoft.com/azure/aks/aks-desktop-overview)** to see and fix what's running. It gives an app-centric view of a namespace with an [AI assistant for troubleshooting](/2026/09/24/aks-desktop-ai-troubleshooting), and developers sign in with the access the platform granted them.
- **[Application Insights](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview)** to prove the app behaves, not just that pods are running.

The [AKS extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-aks-tools), [Agent Skills for AKS](/2026/04/08/agent-skills-for-aks), and the [AKS-MCP server](https://github.com/Azure/aks-mcp) are worth a look too.

:::info
Remember, AI tools don't get a bypass. Agents act under the developer's identity and RBAC, and generated manifests go through the same pull request review and admission policy as everything else.
:::

Here's how that plays out. A developer opens a pull request that requests an environment. After merge, GitOps applies it, and the team gets a managed namespace with quota, a workload identity, Application Insights, and an HTTPS URL.

A week later, say a config change makes the app slower without breaking a single pod. The developer opens AKS desktop to confirm the workload is healthy, finds the slow dependency call in Application Insights traces, reverts the change in Git, and verifies the fix. They never had to ask the platform team for access.

## Measure whether it's working

A platform is a product, so treat adoption as a signal, not a mandate. Track these from day one:

- **Adoption**: teams on the supported path versus going around it
- **Time to first deploy**: from request to a running app
- **Delivery**: [DORA metrics](https://dora.dev/guides/dora-metrics-four-keys/) before and after onboarding
- **Requests to your team**: each one tells you which row to automate next
- **Developer feedback**: ask often, and act on it

## Trade-offs to keep in mind

- **Less control on Automatic.** Some preconfigured settings can't be changed. Check the [feature comparison](https://learn.microsoft.com/azure/aks/intro-aks-automatic) before you depend on one. If you need specific node configuration, Standard is the better call.
- **Two clusters means two of everything.** Two sets of credentials, two upgrade cadences, and two places to look when something breaks.
- **Managed defaults move.** Pin what your contract depends on, such as your template and module versions.
- **The contract is portable, the defaults aren't.** Deployment Safeguards, managed namespaces, and Entra-based identity are Azure services. Keep the contract in your own templates or APIs, and moving becomes a swap of the middle column instead of a rewrite.

## Wrapping up

Start from the contract, not the tool list. Let AKS run the rows it already covers, spend your team's time on the rows it doesn't, and grow into more clusters and tools only when you need them.

Not sure where to start? Pick your most-requested manual task, write down the promise behind it, and find its row in the table above.

This post is the first in a series where I'll build this platform piece by piece:

1. **The environment contract**: a self-service API with Crossplane and OpenTofu (or kro and ASO), delivered by a GitOps extension
2. **The golden path**: from a developer's laptop to AKS Automatic with Containerization Assist, workload identity, and one pull request
3. **Diagnose and recover**: AKS desktop, Application Insights, and GitOps for finding and fixing issues
4. **Scaling out with Fleet**: from one shared cluster to many with [Azure Kubernetes Fleet Manager](https://learn.microsoft.com/azure/kubernetes-fleet/overview)

Which rows are you still assembling by hand? Let us know in [GitHub Discussions](https://github.com/Azure/AKS/discussions), and that'll help shape the series.
