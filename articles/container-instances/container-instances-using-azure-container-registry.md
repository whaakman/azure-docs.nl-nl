---
title: Implementeren naar Azure Containerexemplaren uit het register van de Azure-Container
description: Exemplaren van de Azure-Container uit het register van de Azure-Container implementeren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4205b47dc67920021812c1e573a98de64ad198ec
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Implementeren naar Azure Containerexemplaren uit het register van de Azure-Container

Het register van de Container Azure is een register op basis van Azure, persoonlijke voor installatiekopieën van de Docker-container. In dit artikel bevat informatie over het implementeren van container afbeeldingen die zijn opgeslagen in het register van de Azure-Container om exemplaren van Azure-Container.

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

De Azure CLI bevat-opdrachten voor het maken en beheren van containers in Azure Container instanties. Als u opgeeft met een persoonlijke installatiekopie in de [az container maken] [ az-container-create] opdracht, kunt u ook de afbeelding register wachtwoord vereist om te verifiëren met het register van de container opgeven.

```azurecli-interactive
az container create --resource-group myResourceGroup --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword
```

De [az container maken] [ az-container-create] opdracht ondersteunt ook het opgeven van `--registry-login-server` en `--registry-username`. De aanmeldingsserver voor het Azure Container register is echter altijd *registryname*. azurecr.io en de standaardgebruikersnaam is *registryname*, zodat deze waarden zijn afgeleid van de naam van de installatiekopie als dat niet expliciet is opgegeven.

## <a name="deploy-with-azure-resource-manager-template"></a>Met Azure Resource Manager-sjabloon implementeren

U kunt de eigenschappen van de registratie van uw Azure-Container opgeven in een Azure Resource Manager-sjabloon door het `imageRegistryCredentials` eigenschap in de definitie van de container:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Om te voorkomen dat uw wachtwoord container-register rechtstreeks in de sjabloon opslaan, wordt aangeraden te slaan als een geheim in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) en ernaar wordt verwezen in de sjabloon met behulp van de [systeemeigen integratie tussen de Azure Resource Manager en Sleutelkluis](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Met Azure-portal implementeren

Als u installatiekopieën van de container in het register van de Container Azure onderhouden, kunt u eenvoudig een container maken in Azure Container-exemplaren die gebruikmaken van de Azure-portal.

1. Ga in de Azure-portal naar het register van de container.

2. Selecteer **opslagplaatsen**, selecteert u vervolgens de opslagplaats die u wilt implementeren, met de rechtermuisknop op het label voor de container-installatiekopie die u wilt implementeren, en selecteer **instantie**.

    !['Exemplaar uitvoeren' in het register van Azure-Container in Azure portal][acr-runinstance-contextmenu]

3. Voer een naam voor de container en een naam voor de resourcegroep. U kunt ook de standaardwaarden wijzigen als u wenst.

    ![Menu voor exemplaren van Azure-Container maken][acr-create-deeplink]

4. Zodra de implementatie is voltooid, kunt u navigeren naar de containergroep in het deelvenster meldingen om het IP-adres en andere eigenschappen te vinden.

    ![Details weergeven voor Azure-Container exemplarengroep container][aci-detailsview]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bouwen van containers, toepassen op een privé-container-register en deze implementeren in Azure Containerexemplaren door [het voltooien van de zelfstudie](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create