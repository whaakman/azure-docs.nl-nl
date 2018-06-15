---
title: Implementeren naar Azure Containerexemplaren uit het register van de Azure-Container
description: Informatie over het implementeren van containers in Azure Container-exemplaren die gebruikmaken van installatiekopieën van de container in het register van een Azure-container.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 7a7d2aa61f25bc4782c6a1a6744e329935477f8c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32168112"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Implementeren naar Azure Containerexemplaren uit het register van de Azure-Container

Het register van de Container Azure is een Azure, persoonlijke register voor de installatiekopieën van de Docker-container. In dit artikel wordt beschreven hoe container installatiekopieën die zijn opgeslagen in een Azure container register naar Containerexemplaren Azure implementeert.

## <a name="prerequisites"></a>Vereisten

**Azure Container register**: U hebt een Azure container register-- en ten minste één container afbeelding in het register--de stappen in dit artikel nodig. Als u een register moet, Zie [maken van een container register met de Azure CLI](../container-registry/container-registry-get-started-azure-cli.md).

**Azure CLI**: de opdrachtregelvoorbeelden in dit artikel gebruik de [Azure CLI](/cli/azure/) en zijn geformatteerd voor de Bash-shell. U kunt [Azure CLI installeren](/cli/azure/install-azure-cli) lokaal, of gebruik de [Azure Cloud Shell][cloud-shell-bash].

## <a name="configure-registry-authentication"></a>Register-verificatie configureren

In productiescenario's, toegang tot het register van een Azure container moet worden opgegeven met behulp van [service-principals](../container-registry/container-registry-auth-service-principal.md). Service-principals kunnen u voor op rollen gebaseerde toegangsbeheer naar de container afbeeldingen. U kunt bijvoorbeeld een service-principal configureren met alleen pull toegang tot een register.

In deze sectie kunt u een Azure sleutelkluis en een service-principal maken en de service-principal referenties opgeslagen in de kluis.

### <a name="create-key-vault"></a>Sleutelkluis maken

Als u niet al een kluis in [Azure Key Vault](/azure/key-vault/), één met de Azure CLI met de volgende opdrachten te maken.

Update de `RES_GROUP` variabele met de naam van de resourcegroep in te maken van de sleutelkluis en `ACR_NAME` met de naam van het register van de container. Geef een naam voor de nieuwe sleutelkluis in `AKV_NAME`. De kluisnaam van de moet uniek zijn binnen Azure en moet 3 tot 24 alfanumerieke tekens lang zijn, beginnen met een letter, eindigen met een letter of cijfer, en mag geen opeenvolgende afbreekstreepjes bevatten.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Service-principal maken en opslaan van referenties

Nu moet u een service-principal maken en de referenties opgeslagen in de sleutelkluis.

De volgende opdracht maakt gebruik van [az ad sp maken-voor-rbac] [ az-ad-sp-create-for-rbac] voor het maken van de service-principal en [az keyvault geheime set] [ az-keyvault-secret-set] voor het opslaan van de service-principal **wachtwoord** in de kluis.

```azurecli
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role reader \
                --query password \
                --output tsv)
```

De `--role` argument in de voorgaande opdracht configureert u de service-principal met de *lezer* functie, die deze alleen push toegang tot het register verleent. Als u wilt zowel push als pull toegang verlenen, wijzigen de `--role` argument voor *Inzender*.

Vervolgens opslaan van de service-principal *appId* in de kluis die is de **gebruikersnaam** u voor verificatie met Azure Container register doorgeven.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

U hebt een Azure Sleutelkluis gemaakt en opgeslagen twee geheimen:

* `$ACR_NAME-pull-usr`: De service principal-ID, voor gebruik als het register van de container **gebruikersnaam**.
* `$ACR_NAME-pull-pwd`: De service principal wachtwoord, voor gebruik als het register van de container **wachtwoord**.

U kunt nu verwijst naar deze geheime gegevens met name wanneer u of uw toepassingen en services pull-installatiekopieën uit het register.

## <a name="deploy-container-with-azure-cli"></a>Container met Azure CLI implementeren

Nu dat de referenties voor de service-principal worden opgeslagen in Azure Key Vault geheimen, kunnen uw toepassingen en services deze gebruiken voor toegang tot uw persoonlijke register.

Voer het volgende [az container maken] [ az-container-create] opdracht voor het implementeren van een exemplaar van de container. De opdracht de service-principal-referenties die zijn opgeslagen in Azure Key Vault gebruikt om het register van de container te verifiëren en wordt ervan uitgegaan dat u eerder hebt gepusht de [aci helloworld](container-instances-quickstart.md) installatiekopie toe aan het register. Update de `--image` waarde als u wilt gebruiken van een andere installatiekopie uit het register.

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_NAME.azurecr.io/aci-helloworld:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

De `--dns-name-label` waarde moet uniek zijn binnen Azure, dus de voorgaande opdracht een willekeurig getal met het label van DNS-naam van de container voegt. De uitvoer van de opdracht geeft bijvoorbeeld de container volledig gekwalificeerde domeinnaam (FQDN):

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_NAME.azurecr.io/aci-helloworld:v1 --registry-login-server $ACR_NAME.azurecr.io --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
"aci-demo-25007.eastus.azurecontainer.io"
```

Nadat de container met succes gestart is, kunt u navigeren naar de FQDN in uw browser om te controleren of dat de toepassing correct wordt uitgevoerd.

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

Zie voor meer informatie over het verwijzen naar Azure Sleutelkluis geheimen in Resource Manager-sjabloon [Azure Sleutelkluis gebruiken voor veilige parameterwaarde worden doorgegeven tijdens de implementatie van](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Met Azure-portal implementeren

Als u installatiekopieën van de container in het register van de Container Azure onderhouden, kunt u eenvoudig een container maken in Azure Container-exemplaren die gebruikmaken van de Azure-portal.

1. Ga in de Azure-portal naar het register van de container.

1. Selecteer **opslagplaatsen**, selecteert u vervolgens de opslagplaats die u wilt implementeren, met de rechtermuisknop op het label voor de container-installatiekopie die u wilt implementeren, en selecteer **instantie**.

    !['Exemplaar uitvoeren' in het register van Azure-Container in Azure portal][acr-runinstance-contextmenu]

1. Voer een naam voor de container en een naam voor de resourcegroep. U kunt ook de standaardwaarden wijzigen als u wenst.

    ![Menu voor exemplaren van Azure-Container maken][acr-create-deeplink]

1. Zodra de implementatie is voltooid, kunt u navigeren naar de containergroep in het deelvenster meldingen om het IP-adres en andere eigenschappen te vinden.

    ![Details weergeven voor Azure-Container exemplarengroep container][aci-detailsview]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over verificatie met Azure Container register [verifiëren met een Azure container registry](../container-registry/container-registry-authentication.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-powershell]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az_container_create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set