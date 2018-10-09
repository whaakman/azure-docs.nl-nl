---
title: Implementeren in Azure Container Instances vanuit Azure Container Registry
description: Informatie over het implementeren van containers in Azure Container Instances met behulp van containerinstallatiekopieën in een Azure container registry.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: bbdf9a88c19e8006ffa9669b0c6d95d85506b256
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854453"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Implementeren in Azure Container Instances vanuit Azure Container Registry

Azure Container Registry is een Azure gebaseerd, persoonlijk register voor uw Docker-containerinstallatiekopieën. In dit artikel wordt beschreven hoe u containerinstallatiekopieën die zijn opgeslagen in een Azure-containerregister in Azure Container Instances te implementeren.

## <a name="prerequisites"></a>Vereisten

**Azure Container Registry**: U moet een Azure-containerregister, en ten minste één container-installatiekopie in het register, voltooi de stappen in dit artikel. Als u een register nodig hebt, raadpleegt u [een containerregister maken met de Azure CLI](../container-registry/container-registry-get-started-azure-cli.md).

**Azure CLI**: de opdrachtregelvoorbeelden in dit artikel gebruiken de [Azure CLI](/cli/azure/) en zijn opgemaakt voor de Bash-shell. U kunt [Azure CLI installeren](/cli/azure/install-azure-cli) lokaal, of gebruik de [Azure Cloud Shell][cloud-shell-bash].

## <a name="configure-registry-authentication"></a>Registerverificatie configureren

In productiescenario's, toegang tot een Azure container registry moet worden opgegeven met behulp van [service-principals](../container-registry/container-registry-auth-service-principal.md). Met service-principals kunt u voor op rollen gebaseerde toegangsbeheer bieden voor uw containerinstallatiekopieën. U kunt bijvoorbeeld een service-principal configureren met alleen pull-toegang tot een register.

In deze sectie maakt u een Azure-sleutelkluis en een service-principal maken en opslaan van de service-principal-referenties in de kluis.

### <a name="create-key-vault"></a>Sleutelkluis maken

Als u nog geen kluis hebt in [Azure Key Vault](/azure/key-vault/), kunt u met de volgende opdrachten één maken met de Azure CLI.

Update de `RES_GROUP` variabele met de naam van de resourcegroep waarin u wilt maken van de key vault en `ACR_NAME` met de naam van het containerregister. Geef een naam voor uw nieuwe key vault in `AKV_NAME`. De naam van de kluis moet uniek zijn binnen Azure en moet 3 tot 24 alfanumerieke tekens lang zijn, beginnen met een letter, eindigen met een letter of cijfer, en mag geen opeenvolgende afbreekstreepjes bevatten.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Service-principal maken en referenties opslaan

U moet nu een service-principal maken en de referenties ervan opslaan in uw sleutelkluis.

De volgende opdracht maakt gebruik van [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] te maken van de service-principal en [az keyvault secret set] [ az-keyvault-secret-set] voor het opslaan van de service-principal **wachtwoord** in de kluis.

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

Het argument `--role` in de voorgaande opdracht configureert de service-principal met de rol *lezer*, die deze alleen-push-toegang tot het register verleent. Als u zowel push-als pull-toegang wilt verlenen, wijzigt u het argument `--role` in *Inzender*.

Vervolgens opslaan van de service-principal *appId* in de kluis, dit is de **gebruikersnaam** u doorgeven naar Azure Container Registry voor verificatie.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

U hebt een Azure Key Vault gemaakt en er twee geheimen in opgeslagen:

* `$ACR_NAME-pull-usr`: de service principal-ID, voor gebruik als de **gebruikersnaam** van het containerregister.
* `$ACR_NAME-pull-pwd`: het service principal-wachtwoord, voor gebruik als het **wachtwoord** van het containerregister.

U kunt nu op naam naar deze geheime gegevens verwijzen wanneer u of uw toepassingen en services installatiekopieën uit het register halen.

## <a name="deploy-container-with-azure-cli"></a>Container implementeren met Azure CLI

Nu dat de referenties voor de service-principal in Azure Key Vault-geheimen worden opgeslagen, kunnen uw toepassingen en services gebruiken ze toegang tot uw persoonlijke register.

Voer de volgende [az container create][az-container-create]-opdracht in om een containerinstantie te implementeren. De opdracht van de service-principal-referenties die zijn opgeslagen in Azure Key Vault gebruikt om te verifiëren bij uw containerregister, en wordt ervan uitgegaan dat u eerder hebt gepusht de [aci-helloworld](container-instances-quickstart.md) installatiekopie naar uw register. Update de `--image` waarde op als u wilt gebruiken van een andere installatiekopie vanuit het register.

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

De `--dns-name-label` waarde moet uniek zijn binnen Azure, zodat de voorgaande opdracht wordt een willekeurig getal toegevoegd aan het label van DNS-naam van de container. De uitvoer van de opdracht geeft bijvoorbeeld de Fully Qualified Domain Name (FQDN) van de container weer:

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_NAME.azurecr.io/aci-helloworld:v1 --registry-login-server $ACR_NAME.azurecr.io --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
"aci-demo-25007.eastus.azurecontainer.io"
```

Nadat de container met succes gestart is, kunt u navigeren naar de FQDN-naam in uw browser om te controleren of dat de toepassing correct wordt uitgevoerd.

## <a name="deploy-with-azure-resource-manager-template"></a>Implementeren met Azure Resource Manager-sjabloon

U kunt de eigenschappen van uw Azure Container Registry opgeven in een Azure Resource Manager-sjabloon door op te nemen de `imageRegistryCredentials` eigenschap in de definitie van de container:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Zie voor meer informatie over het verwijzen naar Azure Key Vault-geheimen in een Resource Manager-sjabloon [Azure Key Vault gebruikt om door te geven beveiligde parameterwaarde tijdens de implementatie van](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Implementeren met Azure portal

Als u beheer van containerinstallatiekopieën in Azure Container Registry, kunt u eenvoudig een container maken in Azure Container Instances met behulp van de Azure-portal.

1. Ga naar het containerregister in Azure portal.

1. Selecteer **opslagplaatsen**, en selecteer vervolgens de opslagplaats die u wilt implementeren, met de rechtermuisknop op de tag voor de installatiekopie van de container die u wilt implementeren, en selecteer **uitvoeringsinstantie**.

    !['Exemplaar uitvoeren' in Azure Container Registry in Azure portal][acr-runinstance-contextmenu]

1. Voer een naam op voor de container en een naam voor de resourcegroep. U kunt ook de standaardwaarden wijzigen als u wenst.

    ![Menu voor Azure Container Instances maken][acr-create-deeplink]

1. Nadat de implementatie is voltooid, kunt u navigeren naar de containergroep in het deelvenster meldingen zoeken naar het IP-adres en andere eigenschappen.

    ![Details voor Azure Container Instances-containergroep weergeven][aci-detailsview]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over verificatie op Azure Container Registry [verifiëren met een Azure container registry](../container-registry/container-registry-authentication.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-powershell]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az-container-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set