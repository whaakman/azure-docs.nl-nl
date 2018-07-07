---
title: Verifiëren met Azure Container Registry uit Azure Kubernetes Service
description: Informatie over het bieden van toegang tot afbeeldingen in uw persoonlijke containerregister van Azure Kubernetes Service met behulp van een service-principal voor Azure Active Directory.
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/24/2018
ms.author: marsma
ms.openlocfilehash: 36b66fdcd157e4c44fcd451c8cc07ba68242b583
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888759"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Verifiëren met Azure Container Registry uit Azure Kubernetes Service

Wanneer u Azure Container Registry (ACR) met Azure Kubernetes Service (AKS), moet een verificatiemethode tot stand worden gebracht. In dit document worden de aanbevolen configuraties voor verificatie tussen deze twee Azure-services.

## <a name="grant-aks-access-to-acr"></a>GRANT AKS toegang naar ACR

Als u een AKS-cluster maakt, wordt ook een service-principal gemaakt voor het beheren van cluster ervan met Azure-resources. Deze service-principal kan ook worden gebruikt voor verificatie met een ACR-register. Om dit te doen, moet een roltoewijzing worden gemaakt om de service principal lezen toegang tot de ACR-resource.

Het volgende voorbeeld kan worden gebruikt om deze bewerking te voltooien.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Toegang met Kubernetes-geheim

In sommige gevallen kan de service-principal wordt gebruikt door AKS kan niet worden binnen het bereik van het ACR-register. Voor deze gevallen kunt u een unieke service-principal maken en het bereik instellen op alleen de ACR-register.

Het volgende script kan worden gebruikt om de service-principal maken.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create a contributor role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --role Reader --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principle client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

De referenties voor de service-principal kunnen nu worden opgeslagen in een Kubernetes [installatiekopie pull geheim] [ image-pull-secret] en bij het uitvoeren van containers in een AKS-cluster.

De volgende opdracht maakt de Kubernetes geheim. Vervang de servernaam van de met de ACR-aanmeldingsserver, de naam van de gebruiker door de service-principal-id en het wachtwoord met het wachtwoord van de service-principal.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

De Kubernetes-geheim kan worden gebruikt in een schil implementatie met de `ImagePullSecrets` parameter.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
