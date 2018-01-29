---
title: Verificatie met Azure Container register van Azure Containerservice
description: "Informatie over het bieden van toegang tot afbeeldingen in het register privé-container van Azure Container Service met behulp van een Azure Active Directory-service-principal."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/12/2018
ms.author: nepeters
ms.openlocfilehash: 86a160d8f2dbfb0e385d9dbed7cf6d789f5a8df6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-service"></a>Verificatie met Azure Container register van Azure Containerservice

Wanneer u gebruikmaakt van Azure Container register (ACR) met Azure Container Service (AKS), moet een verificatiemethode tot stand worden gebracht. In dit document worden de aanbevolen configuraties voor authenticatie tussen deze twee Azure-services.

## <a name="grant-aks-access-to-acr"></a>GRANT AKS toegang tot ACR

Wanneer een AKS-cluster is gemaakt, wordt ook een service-principal gemaakt voor het cluster ervan met Azure-resources te beheren. Deze service-principal kan ook worden gebruikt voor verificatie met een ACR-register. Een roltoewijzing moet worden gemaakt voor de service principal leestoegang tot de resource ACR te verlenen om dit te doen. 

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

## <a name="access-with-kubernetes-secret"></a>Toegang met Kubernetes geheim

In sommige gevallen kan de service-principal wordt gebruikt door AKS bereik kan niet worden ingesteld in het register ACR. Voor dergelijke gevallen kunt u een unieke service-principal maken en kunt u het bereik met alleen het ACR-register.

Het volgende script kan worden gebruikt voor de service-principal maken. 

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

De referenties voor de service-principal kunnen nu worden opgeslagen in een Kubernetes [installatiekopie pull geheim] [ image-pull-secret] en waarnaar wordt verwezen als containers in een cluster AKS wordt uitgevoerd. 

De volgende opdracht maakt de Kubernetes geheim. De naam van de server met de server van de aanmelding ACR, de gebruikersnaam vervangen door de service-principal-id en het wachtwoord met het service-principal wachtwoord.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Het geheim Kubernetes kan worden gebruikt in een schil implementatie met de `ImagePullSecrets` parameter. 

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
