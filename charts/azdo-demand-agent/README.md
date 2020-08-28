# Introduction


This chart bootstraps an demand agent for [On-Demand Agents](https://blogs.zeiss.com/tech/ms-azure-devops-on-demand-agents/) on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.


## Prerequisites

 - Kubernetes 1.8+ or newer

## Configuration

The following tables lists the configurable parameters of the `azdo-init-agent` chart and their default values.

| Parameter                         | Description                           | Default                                                   |
| --------------------------------- | ------------------------------------- | --------------------------------------------------------- |
| `agent.repository`                       | demand agent image repository                     | `cizi/demand-agent`                                                  |
| `agent.type`                       | type of agent, which is nothing but docker tag                    | `nodejs`                                                  |
| `agent.pullSecrets`               | Specify agent image pull secrets            | `nil` (does not add image pull secrets to deployed pods)  |
| `agent.pullPolicy`                | agent image pull policy                     | `Always`                                                  |


## Prerequisite

Make sure you have install init-agents using the [azdo-init-agent](../../azdo-init-agent/README.md#prerequisite) helm chart , which includes configuration and secret required for the demand agent.

## Installing the Chart

The chart can be installed with the following command:

```bash

helm install --namespace <AZDO_POOL> --wait -n nodejs-124243 --set agent.type=nodejs  zeiss/azdo-demand-agent
```

`--wait` flag is important here, as helm will Waits until then agent is ready and connected to Azure DevOps.



## Uninstalling the Chart

The chart can be uninstalled/deleted as follows:

```bash
helm uninstall -n <AZDO_POOL> nodejs-124243
```

This command removes all the Kubernetes resources associated with the chart and deletes the helm release.
