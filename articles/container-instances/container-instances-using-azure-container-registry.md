---
title: Implementeren naar Azure Containerexemplaren uit het register van de Azure-Container | Azure Docs
description: Implementeren naar Azure Containerexemplaren uit het register van de Azure-Container
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: aa1c4ea379c10dff246e2f924a345f9fa444aa64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Implementeren naar Azure Containerexemplaren uit het register van de Azure-Container

Het register van de Container Azure is een register op basis van Azure, persoonlijke voor installatiekopieën van de Docker-container. In dit artikel bevat informatie over het implementeren van container afbeeldingen die zijn opgeslagen in het register van de Azure-Container om exemplaren van Azure-Container.

## <a name="using-the-azure-cli"></a>Azure CLI gebruiken

De Azure CLI bevat-opdrachten voor het maken en beheren van containers in Azure Container instanties. Als u opgeeft met een persoonlijke installatiekopie in de `create` opdracht, kunt u ook de afbeelding register wachtwoord vereist om te verifiëren met het register van de container opgeven.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

De `create` opdracht ondersteunt ook het opgeven van de `registry-login-server` en `registry-username`. De aanmeldingsserver voor het Azure Container register is echter altijd *registryname*. azurecr.io en de standaardgebruikersnaam is *registryname*, zodat deze waarden zijn afgeleid van de naam van de installatiekopie als dat niet expliciet is opgegeven.

## <a name="using-an-azure-resource-manager-template"></a>Met behulp van een Azure Resource Manager-sjabloon

U kunt de eigenschappen van de registratie van uw Azure-Container opgeven in een Azure Resource Manager-sjabloon door het `imageRegistryCredentials` eigenschap in de definitie van de container:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Om te voorkomen dat uw wachtwoord container-register rechtstreeks in de sjabloon opslaan, wordt aangeraden te slaan als een geheim in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) en ernaar wordt verwezen in de sjabloon met behulp van de [systeemeigen integratie tussen de Azure Resource Manager en Sleutelkluis](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="using-the-azure-portal"></a>Azure Portal gebruiken

Als u installatiekopieën van de container in het register van de Container Azure onderhouden, kunt u eenvoudig een container maken in Azure Container-exemplaren die gebruikmaken van de Azure-portal.

1. Ga in de Azure-portal naar het register van de container.

2. Kies opslagplaatsen.

    ![Het menu Azure Container register in de Azure portal][acr-menu]

3. Kies de opslagplaats die u implementeren wilt uit.

4. Met de rechtermuisknop op het label voor de container-installatiekopie die u wilt implementeren.

    ![Snelmenu voor het starten van de container met exemplaren van Azure-Container][acr-runinstance-contextmenu]

5. Voer een naam voor de container en een naam voor de resourcegroep. U kunt ook de standaardwaarden wijzigen als u wenst.

    ![Menu voor exemplaren van Azure-Container maken][acr-create-deeplink]

6. Zodra de implementatie is voltooid, kunt u navigeren naar de containergroep in het deelvenster meldingen om het IP-adres en andere eigenschappen te vinden.

    ![Details weergeven voor Azure-Container exemplarengroep container][aci-detailsview]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bouwen van containers, toepassen op een privé-container-register en deze implementeren in Azure Containerexemplaren door [het voltooien van de zelfstudie](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
