---
title: Implementeren in Azure Container Instances vanuit Azure Container Registry
description: Informatie over het implementeren van containers in Azure Container Instances met behulp van containerinstallatiekopieën in een Azure container registry.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 01/04/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: f577a7dd9f517be6ab7b632a82227e4807862ba5
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55727905"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Implementeren in Azure Container Instances vanuit Azure Container Registry

[Azure Container Registry](../container-registry/container-registry-intro.md) is een op basis van Azure, beheerde container registry-service gebruikt voor het opslaan van persoonlijke Docker-containerinstallatiekopieën. In dit artikel wordt beschreven hoe u containerinstallatiekopieën die zijn opgeslagen in een Azure-containerregister in Azure Container Instances te implementeren.

## <a name="prerequisites"></a>Vereisten

**Azure-containerregister**: U moet een Azure-containerregister, en ten minste één container-installatiekopie in het register, voltooi de stappen in dit artikel. Als u een register nodig hebt, raadpleegt u [een containerregister maken met de Azure CLI](../container-registry/container-registry-get-started-azure-cli.md).

**Azure CLI**: De opdrachtregelvoorbeelden in dit artikel gebruiken de [Azure CLI](/cli/azure/) en zijn opgemaakt voor de Bash-shell. U kunt [Azure CLI installeren](/cli/azure/install-azure-cli) lokaal, of gebruik de [Azure Cloud Shell][cloud-shell-bash].

## <a name="configure-registry-authentication"></a>Registerverificatie configureren

In productiescenario's, toegang tot een Azure container registry moet worden opgegeven met behulp van [service-principals](../container-registry/container-registry-auth-service-principal.md). Service-principals kunnen u [op rollen gebaseerd toegangsbeheer](../container-registry/container-registry-roles.md) aan uw containerinstallatiekopieën. U kunt bijvoorbeeld een service-principal configureren met alleen pull-toegang tot een register.

In de volgende sectie, kunt u een Azure-sleutelkluis en een service-principal maken en opslaan van de service-principal-referenties in de kluis. 

### <a name="create-key-vault"></a>Sleutelkluis maken

Als u nog geen kluis hebt in [Azure Key Vault](/azure/key-vault/), kunt u met de volgende opdrachten één maken met de Azure CLI.

Update de `RES_GROUP` variabele met de naam van een bestaande resourcegroep waarin u wilt maken van de key vault en `ACR_NAME` met de naam van het containerregister. Geef een naam voor uw nieuwe key vault in `AKV_NAME`. De naam van de kluis moet uniek zijn binnen Azure en moet 3 tot 24 alfanumerieke tekens lang zijn, beginnen met een letter, eindigen met een letter of cijfer, en mag geen opeenvolgende afbreekstreepjes bevatten.

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
                --name http://$ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv)
```

Het argument `--role` in de voorgaande opdracht configureert de service-principal met de rol *acrpull*, die de principal alleen-push-toegang tot het register geeft. Als u zowel push-als pull-toegang wilt geven, wijzigt u het argument `--role` in *acrpush*.

Vervolgens opslaan van de service-principal *appId* in de kluis, dit is de **gebruikersnaam** u doorgeven naar Azure Container Registry voor verificatie.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

U hebt een Azure Key Vault gemaakt en er twee geheimen in opgeslagen:

* `$ACR_NAME-pull-usr`: De service-principal-id, voor gebruik als de **gebruikersnaam** van het containerregister.
* `$ACR_NAME-pull-pwd`: Het service-principal-wachtwoord, voor gebruik als het **wachtwoord** van het containerregister.

U kunt nu op naam naar deze geheime gegevens verwijzen wanneer u of uw toepassingen en services installatiekopieën uit het register halen.

## <a name="deploy-container-with-azure-cli"></a>Container implementeren met Azure CLI

Nu dat de referenties voor de service-principal in Azure Key Vault-geheimen worden opgeslagen, kunnen uw toepassingen en services gebruiken ze toegang tot uw persoonlijke register.

Aanmeldingsnaam voor server van het register met behulp van de eerste keer de [az acr show] [ az-acr-show] opdracht. Naam van de aanmeldingsserver is allemaal kleine letters en die vergelijkbaar is met `myregistry.azurecr.io`.

```azurecli
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --resource-group $RES_GROUP --query "loginServer" --output tsv)
```

Voer de volgende [az container create][az-container-create]-opdracht in om een containerinstantie te implementeren. De opdracht van de service-principal-referenties die zijn opgeslagen in Azure Key Vault gebruikt om te verifiëren bij uw containerregister, en wordt ervan uitgegaan dat u eerder hebt gepusht de [aci-helloworld](container-instances-quickstart.md) installatiekopie naar uw register. Update de `--image` waarde op als u wilt gebruiken van een andere installatiekopie vanuit het register.

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_LOGIN_SERVER/aci-helloworld:v1 \
    --registry-login-server $ACR_LOGIN_SERVER \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

De `--dns-name-label` waarde moet uniek zijn binnen Azure, zodat de voorgaande opdracht wordt een willekeurig getal toegevoegd aan het label van DNS-naam van de container. De uitvoer van de opdracht geeft bijvoorbeeld de Fully Qualified Domain Name (FQDN) van de container weer:

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_LOGIN_SERVER/aci-helloworld:v1 --registry-login-server $ACR_LOGIN_SERVER --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
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

Als u beheer van containerinstallatiekopieën in een Azure container registry, kunt u eenvoudig een container maken in Azure Container Instances met behulp van de Azure-portal. Wanneer u de portal gebruikt om een containerexemplaar van een containerregister te implementeren, moet u van het register inschakelen [beheerdersaccount](../container-registry/container-registry-authentication.md#admin-account). Het beheerdersaccount dat is ontworpen voor één gebruiker toegang tot het register, hoofdzakelijk voor testdoeleinden. 

1. Ga naar het containerregister in Azure portal.

1. Selecteer om te bevestigen dat het beheerdersaccount dat is ingeschakeld, **toegangssleutels**, en klikt u onder **gebruiker met beheerdersrechten** Selecteer **inschakelen**.

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
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az-container-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set