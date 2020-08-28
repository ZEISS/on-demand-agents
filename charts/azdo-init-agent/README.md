# Introduction


This chart bootstraps an init-agent for [On-Demand Agents](https://blogs.zeiss.com/tech/ms-azure-devops-on-demand-agents/) on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

 - Kubernetes 1.8+ or newer

## Configuration

The following tables lists the configurable parameters of the `azdo-init-agent` chart and their default values.

| Parameter                         | Description                           | Default                                                   |
| --------------------------------- | ------------------------------------- | --------------------------------------------------------- |
| `image.repository`                | init-agent image                      | `cizi/init-agent`                                    |
| `image.tag`                       | Specify image tag                     | `latest`                                                  |
| `image.pullSecrets`               | Specify image pull secrets            | `nil` (does not add image pull secrets to deployed pods)  |
| `image.pullPolicy`                | Image pull policy                     | `Always`                                                  |
| `replicas`                        | Number of azdo-init-agent instaces started | `3`                                                   |  
| `azdoUrl`                 | Azure DevOps Url             | `nil` (must be provided during installation)             |
| `azdoToken`                   | Azure DevOps personal access token     | `nil` (must be provided during installation)             |
| `azdoPool`                    | Azure DevOps Agent pool name           | `nil` (must be provided during installation)             |
| `azdevopsWorkspace`               | Azure DevOps Agent workspace           | `/workspace`                                             |


## Prerequisite
1. Create a personal access token with the authorized scope **Agent Pools(read, manage)**  following these [instructions](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops#create-personal-access-tokens-to-authenticate-access). You will have to provide later the base64 encoded value of this token to the `azdoToken` value of the chart.

2. Create a new [agent pool](https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/pools-queues#creating-agent-pools-and-queues). We will use the same agent pool name as a Kubernetes Namespace for our on-demand agents.

## Installing the Chart

The chart can be installed with the following command:

```bash
export PAT_TOKEN=$(echo -n '<PAT TOKEN>' | base64)

helm install --namespace <AZDO_POOL> --set azdoToken=${PAT_TOKEN} --set azdoUrl=<AZDO_URL> --set azdoPool=<AZDO_POOL> -n azdo-init-agent  zeiss/azdo-init-agent  .
```

Your deployment should look like this if everything works fine:

```bash
kubectl get pods --namespace <NAMESPACE>
NAME           READY     STATUS    RESTARTS   AGE
azdo-agent-0   1/1       Running   0          1m
azdo-agent-1   1/1       Running   0          1m
azdo-agent-2   1/1       Running   0          1m
```

## Scale up the number of Init agents

The number of Init agents can be easily increased to `5` by using the following command:

```bash
kubectl scale --namespace <AZDO_POOL> azdo-init-agent --replicas 5
```

## Uninstalling the Chart

The chart can be uninstalled/deleted as follows:

```bash
helm uninstall -n <AZDO_POOL> azdo-init-agent
```

This command removes all the Kubernetes resources associated with the chart and deletes the helm release.
