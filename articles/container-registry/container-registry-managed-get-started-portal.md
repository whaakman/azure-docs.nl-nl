---
title: "Privé-Docker-register maken - Azure Portal | Microsoft Docs"
description: "Aan de slag met het maken en beheren van privé-Docker-containerregisters met Azure Portal"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 560aee42b0c5a61c37c594d7937f833ab7183d49
ms.contentlocale: nl-nl
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-portal"></a>Een beheerd containerregister maken met Azure Portal

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. In deze handleiding vindt u instructies voor het maken van een beheerd exemplaar van Azure Container Registry via Azure Portal.

Beheerde Azure-containerregisters zijn nog in preview en zijn niet in alle regio's beschikbaar.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-container-registry"></a>Een containerregister maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Zoek in Marketplace naar **Azure Container Registry** en selecteer het product.

3. Klik op **Maken** om de blade ACR maken te openen.

    ![Container Registry-instellingen](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. Voer de volgende gegevens in op de blade **Azure Container Registry**. Klik op **Maken** wanneer u klaar bent.

    a. **Registernaam**: een unieke domeinnaam op het hoogste niveau voor uw specifieke register. De registernaam in dit voorbeeld is *myAzureContainerRegistry1*, maar u moet deze naam vervangen door een eigen unieke naam. De naam mag alleen uit letters en cijfers bestaan.

    b. **Resourcegroep**: selecteer een bestaande [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups) of typ de gewenste naam voor een nieuwe resourcegroep.

    c. **Locatie**: selecteer een Azure Datacenter-locatie waar de service [beschikbaar](https://azure.microsoft.com/regions/services/) is, zoals **Zuid-centraal VS**.

    d. **Beheerder**: schakel eventueel toegang tot het register in voor een beheerder. U kunt deze instelling wijzigen nadat u het register hebt gemaakt.

    e. **Beheerd register gebruiken**: selecteer Ja als ACR automatisch de registeropslag moet beheren, webhooks moet gebruiken en AAD-verificatie moet toepassen.

    f. **Prijscategorie**: selecteer een prijscategorie, zie ACR-prijzen voor meer informatie.

## <a name="log-in-to-acr-instance"></a>Aanmelden bij ACR-exemplaar

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. 

Gebruik hiervoor de Azure CLI 2.0. Meld u indien nodig eerst aan bij Azure, met behulp van de opdracht [az login](/cli/azure/#login). 

```azurecli
az login
```

Gebruik vervolgens de opdracht [az acr login](/cli/azure/acr#login) om u aan te melden bij Azure Container Registry.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a>Azure Container Registry gebruiken

### <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

Gebruik de `az acr`-CLI-opdrachten om query's toe te passen op de installatiekopieën en tags in een opslagplaats.

> [!NOTE]
> Container Registry ondersteunt momenteel de opdracht `docker search` niet voor het toepassen van query's op installatiekopieën en tags.

### <a name="list-repositories"></a>Opslagplaatsen weergeven

In het volgende voorbeeld worden de opslagplaatsen in een register weergegeven in JSON-indeling (JavaScript Object Notation):

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Tags weergeven

In het volgende voorbeeld worden de tags in de opslagplaats **samples/nginx** weergegeven in JSON-indeling:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een beheerd exemplaar van Azure Container Registry gemaakt met behulp van Azure Portal.

> [!div class="nextstepaction"]
> [Uw eerste installatiekopie pushen met de Docker-CLI](container-registry-get-started-docker-cli.md)
