---
page_type: sample
languages:
  - python
products:
  - azure
  - azure-redis-cache
description: "This sample creates a multi-container application in an Azure Kubernetes Service (AKS) cluster."
---

# Azure Voting App

This sample creates a multi-container application in an Azure Kubernetes Service (AKS) cluster. The application interface has been built using Python / Flask. The data component is using Redis.

To walk through a quick deployment of this application, see the AKS [quick start](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough?WT.mc_id=none-github-nepeters).

To walk through a complete experience where this code is packaged into container images, uploaded to Azure Container Registry, and then run in and AKS cluster, see the [AKS tutorials](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-prepare-app?WT.mc_id=none-github-nepeters).

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## To Deploy in Azure Kubernets Cluster

https://stackoverflow.com/questions/75672941/error-in-creating-service-connection-in-azure-devop-to-azure-kubernetes-service
This is because Kubernets Cluster is created using 1.26 which do not create Secrets for service Account.


### Use Cluster Configuration from Azure Portal
enabled Azure AD and Azure RBAC


a user as a Kubernetes Cluster Admin :
- Add role for Service Cluster Admin Role
- Add role for Kubernets Service RBAC Role
- Owner

validate by running
az aks get-credentials --resource-group <resource group> --name <clusterName> --admin
THIS will ovveride .kube/config

In Azure DevOps project, Create a new Service Connection
- Use KubeConfig
- Add KubeConig from the file .kube/config
- 
- Accept-untrusted certificate as True
- Provide a Service Connection Name
- Select Verify

used the same service connection for Azure Kubernetes task in Azure DevOps
- new service connection for kubernets created 

create a Azure Pipeline Deployment File

    trigger: none

    pool:
      vmImage: ubuntu-latest

    steps:
    - task: Kubernetes@1
      displayName: Azure voting app
      inputs:
        connectionType: Kubernetes Service Connection
        kubernetesServiceEndpoint: <name of new service connection for Azure cluster>
        command: apply
        arguments: -f azure-vote-all-in-one-redis.yaml

Use Azure Pipeline Wizard to deploy using custom YAML file

choose the new file.
