---
title: Service-principal voor Azure Kubernetes-cluster | Microsoft-documenten
description: Service-principal voor Azure Active Directory maken en beheren in een Azure Container Service-cluster met Kubernetes
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: danlep
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: b76020e3e5855a63c416851d9b9adefdbdc5874a
ms.contentlocale: nl-nl
ms.lasthandoff: 05/03/2017


---

# <a name="about-the-azure-active-directory-service-principal-for-a-kubernetes-cluster-in-azure-container-service"></a>Over de service-principal voor Azure Active Directory voor een Kubernetes-cluster in Azure Container Service



In Azure Container Service is voor Kubernetes een [service-principal voor Azure Active Directory](../active-directory/active-directory-application-objects.md) vereist als serviceaccount voor gebruik met Azure-API's. De service-principal is nodig om resources zoals door [gebruikers gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md) en de [Azure Load Balancer uit laag vier](../load-balancer/load-balancer-overview.md) dynamisch te beheren.

In dit artikel worden verschillende opties getoond om een service-principal op te geven voor uw Kubernetes-cluster. Bijvoorbeeld: als u de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) hebt geïnstalleerd en ingesteld, kunt u de opdracht [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) uitvoeren om tegelijkertijd het Kubernetes-cluster en de service-principal te maken.



## <a name="requirements-for-the-service-principal"></a>Vereisten voor de service-principal

Hieronder ziet u de vereisten voor de service-principal voor Azure Active Directory in een Kubernetes-cluster in Azure Container Service. 

* **Bereik**: de resourcegroep waarin het cluster is geïmplementeerd

* **Rol**: **inzender**

* **Clientgeheim**: moet een wachtwoord zijn. U kunt momenteel geen service-principal gebruiken om verificatie via een certificaat in te stellen.

> [!NOTE]
> Elke service-principal is gekoppeld aan een Azure Active Directory-toepassing. De service-principal voor een Kubernetes-cluster kan zijn gekoppeld aan elke geldige Azure Active Directory-toepassingsnaam.
> 


## <a name="service-principal-options-for-a-kubernetes-cluster"></a>Service-principalopties voor een Kubernetes-cluster

### <a name="option-1-pass-the-service-principal-client-id-and-client-secret"></a>Optie 1: de client-id en het clientgeheim voor de service-principal doorgeven

Geef bij het maken van het Kubernetes-cluster de **client-id** (ook wel de `appId` genoemd, voor toepassings-id) en het **clientgeheim** (`password`) als parameters op voor een bestaande service-principal. Als u een bestaande service-principal gebruikt, moet u ervoor zorgen dat deze voldoet aan de vereisten in de vorige sectie. Zie [Een service-principal maken](#create-a-service-principal-in-azure-active-directory) verderop in dit artikel als u een service-principal wilt maken.

U kunt deze parameters opgeven tijdens het [implementeren van het Kubernetes-cluster](./container-service-deployment.md) via de portal, de Azure CLI (Command-Line Interface) 2.0, Azure PowerShell of een andere methode.

>[!TIP] 
>Zorg ervoor dat u bij het opgeven van de **client-id** gebruikmaakt van de `appId` en niet de `ObjectId` van de service-principal.
>

In het volgende voorbeeld wordt één manier getoond om de parameters door te geven met behulp van de Azure CLI 2.0 (zie de [instructies voor installeren en instellen](/cli/azure/install-az-cli2)). In dit voorbeeld wordt gebruikgemaakt van de [Kubernetes-snelstartsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).

1. [Download](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) het bestand met sjabloonparameters `azuredeploy.parameters.json` van GitHub.

2. Voer de waarden voor `servicePrincipalClientId` en `servicePrincipalClientSecret` in het bestand in om de service-principal op te geven. (U moet ook uw eigen waarden voor `dnsNamePrefix` en `sshRSAPublicKey` opgeven. De laatste waarde is de openbare SSH-sleutel voor toegang tot de cluster.) Sla het bestand op.

    ![Parameters voor de service-principal doorgeven](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Voer de volgende opdracht uit met behulp van `--parameters` om het pad naar het bestand azuredeploy.parameters.json in te stellen. Met deze opdracht wordt het cluster geïmplementeerd in een resourcegroep die u `myResourceGroup` hebt genoemd, in de regio VS - west.

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


### <a name="option-2-generate-the-service-principal-when-creating-the-cluster-with-the-azure-cli-20"></a>Optie 2: de service-principal genereren tijdens het maken van het cluster met de Azure CLI 2.0

Als u de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) hebt geïnstalleerd en ingesteld, kun u de opdracht [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) uitvoeren om [het cluster te maken](./container-service-create-acs-cluster-cli.md).

Net zoals bij andere opties voor het maken van Kubernetes-clusters kunt u parameters voor een bestaande service-principal opgeven tijdens het uitvoeren van `az acs create`. Als u deze parameters echter weglaat, wordt in Azure Container Service automatisch een service-principal gemaakt. Dit vindt transparant plaats tijdens de implementatie. 

Met de volgende opdracht wordt een Kubernetes-cluster gemaakt en worden beide SSH-sleutels en de referenties voor de service-principal gegenereerd:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

## <a name="create-a-service-principal-in-azure-active-directory"></a>Een service-principal maken in Azure Active Directory

Azure biedt verschillende methoden om een service-principal te maken in Azure Active Directory voor gebruik in uw Kubernetes-cluster. 

In de volgende voorbeeldopdrachten ziet u hoe u dit kunt doen met de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2). U kunt ook een service-principal maken met behulp van de [Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md), via de [klassieke portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) of op een andere manier.

> [!IMPORTANT]
> Bekijk eerst de vereisten voor de service-principal die eerder in dit artikel worden beschreven.
>

```azurecli
az login

az account set --subscription "mySubscriptionID"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

Er wordt uitvoer geretourneerd die er ongeveer zo uitziet (dit voorbeeld is geredigeerd):

![Een service-principal maken](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Gemarkeerd zijn de **client-id** (`appId`) en het **clientgeheim** (`password`) die u gebruikt als parameters voor de service-principal voor de implementatie van de cluster.


Bevestig uw service-principal door een nieuwe shell te openen en de volgende opdrachten uit te voeren, ter vervanging in `appId`, `password` en `tenant`:

```azurecli 
az login --service-principal -u yourClientID -p yourClientSecret --tenant yourTenant

az vm list-sizes --location westus
```

## <a name="additional-considerations"></a>Aanvullende overwegingen


* Als u de **client-id** voor de service-principal opgeeft, kunt u de waarde van de `appId` gebruiken (zoals beschreven in dit artikel) of de bijbehorende service-principal `name` (bijvoorbeeld:        `https://www.contoso.org/example`).

* Als u de opdracht `az acs create` gebruikt om de service-principal automatisch te genereren, worden de referenties voor de service-principal naar het bestand ~/.azure/acsServicePrincipal.json geschreven op de computer die wordt gebruikt om de opdracht uit te voeren.

* Op de hoofd- en knooppunt-VM's in de Kubernetes-cluster worden de referenties voor de service-principal opgeslagen in het bestand /etc/kubernetes/azure.json.

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Kubernetes](container-service-kubernetes-walkthrough.md) in de Cluster Container Service-cluster.

