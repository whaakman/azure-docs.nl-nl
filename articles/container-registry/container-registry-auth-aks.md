---
title: Verifiëren met Azure Container Registry uit Azure Kubernetes Service
description: Informatie over het bieden van toegang tot afbeeldingen in uw persoonlijke containerregister van Azure Kubernetes Service met behulp van een service-principal voor Azure Active Directory.
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: marsma
ms.openlocfilehash: d2f7769469c9f3ebdbef5fc6ee1f09b1acd573ef
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715826"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Verifiëren met Azure Container Registry uit Azure Kubernetes Service

Wanneer u Azure Container Registry (ACR) met Azure Kubernetes Service (AKS), moet een verificatiemethode tot stand worden gebracht. Dit artikel worden de aanbevolen configuraties voor verificatie tussen deze twee Azure-services.

## <a name="grant-aks-access-to-acr"></a>GRANT AKS toegang naar ACR

Wanneer u een AKS-cluster maakt, maakt Azure ook een service principal voor de ondersteuning van cluster ervan met andere Azure-resources. U kunt deze automatisch gegenereerde service-principal gebruiken voor verificatie met een ACR-register. Om dit te doen, moet u een Azure AD maken [roltoewijzing](../role-based-access-control/overview.md#role-assignment) die van het cluster-service-principal toegang naar het containerregister verleent.

Gebruik het volgende script aan het AKS gegenereerde service principal toegang verlenen tot een Azure container registry. Wijzig de `AKS_*` en `ACR_*` variabelen voor uw omgeving voordat u het script is uitgevoerd.

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

In sommige gevallen moet u mogelijk niet de vereiste rol toewijzen aan de automatisch gegenereerde AKS service-principal verlenen van toegang tot ACR. Bijvoorbeeld, vanwege het beveiligingsmodel van uw organisatie, u mogelijk niet gemachtigd in uw Azure AD-directory een rol toewijzen aan de AKS gegenereerde service-principal. In dat geval kunt u een nieuwe service-principal maken en vervolgens het toegang geven tot de container registry met behulp van een Kubernetes-geheim installatiekopie pull.

Het volgende script gebruiken om te maken van een nieuwe service-principal (gebruikt u de referenties voor de Kubernetes-geheim installatiekopie pull). Wijzig de `ACR_NAME` variabele voor uw omgeving voordat u het script is uitgevoerd.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create a 'Reader' role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --role Reader --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principal client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

U kunt nu de service-principal-referenties opslaan in een Kubernetes [installatiekopie pull geheim][image-pull-secret], die uw AKS-cluster verwijst bij het uitvoeren van containers.

Gebruik de volgende **kubectl** opdracht naar de Kubernetes-geheim maken. Vervang `<acr-login-server>` met de volledig gekwalificeerde naam van uw Azure-containerregister (dit is in de indeling 'acrname.azurecr.io'). Vervang `<service-principal-ID>` en `<service-principal-password>` door de waarden die u hebt verkregen door het vorige script is uitgevoerd. Vervang `<email-address>` met elk grammaticaal correcte e-mailadres.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

U kunt nu de Kubernetes-geheim in pod-implementaties gebruiken door de naam op te geven (in dit geval 'acr-verificatie') in de `imagePullSecrets` parameter:

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
