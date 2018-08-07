---
title: 'Zelfstudie: Een Azure-webtoepassing configureren voor het lezen van een geheim uit Key Vault | Microsoft Docs'
description: 'Zelfstudie: Een Node.js-toepassing configureren voor het lezen van een geheim uit Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.workload: identity
ms.topic: quickstart
ms.date: 08/01/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: cc43081463667eba06af6538f3d78f16544ed2a5
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412239"
---
# <a name="quickstart-how-to-set-and-read-a-secret-from-key-vault-in-a-node-web-app"></a>Snelstart: Instellen en lezen van een geheim uit Key Vault in een Node-web-app 

In deze snelstart wordt getoond hoe u een geheim in Key Vault kunt opslaan en het er met een web-app kunt uithalen. Deze web-app kan lokaal of in Azure worden uitgevoerd. In de snelstart wordt gebruikgemaakt van Node.js en beheerde service-identiteiten (MSI's)

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in Key Vault.
> * Een geheim lezen uit Key Vault.
> * Een Azure-webtoepassing maken.
> * [Beheerde service-identiteiten inschakelen](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview).
> * De vereiste machtigingen verlenen aan de webtoepassing om gegevens te lezen uit Key Vault.

Controleer voordat u verdergaat of u bekend bent met de [basisbeginselen](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Vereisten

* [Node JS](https://nodejs.org/en/)
* [Git](https://www.git-scm.com/)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 of nieuwer
* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="login-to-azure"></a>Aanmelden bij Azure

Als u zich wilt aanmelden bij Azure met behulp van de CLI, typt u:

```azurecli
az login
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Selecteer de naam van een resourcegroep en vul de tijdelijke aanduiding in.
In het volgende voorbeeld wordt een resourcegroep met de naam *<YourResourceGroupName>* gemaakt op de locatie *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

De resourcegroep die u zojuist hebt gemaakt, wordt overal in deze zelfstudie gebruikt.

## <a name="create-an-azure-key-vault"></a>Een Azure Key Vault maken

Vervolgens maakt u een Key Vault met de resourcegroep die u in de vorige stap hebt gemaakt. Hoewel in dit artikel 'ContosoKeyVault' wordt gebruikt als de naam voor de sleutelkluis, moet u een unieke naam gebruiken. Geef de volgende informatie op:

* Naam van de kluis: **selecteer hier de naam van de Key Vault**.
* Naam van de resourcegroep: **selecteer hier de naam van de resourcegroep**.
* De locatie: **VS - oost**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="add-a-secret-to-key-vault"></a>Een geheim toevoegen aan Key Vault

We gaan een geheim toevoegen om te laten zien hoe dit werkt. U kunt een SQL-verbindingsreeks opslaan of andere gegevens die u veilig wilt bewaren, maar wel beschikbaar wilt stellen aan uw toepassing. In deze zelfstudie gebruiken we het geheim **AppSecret** en slaan we de waarde van **MySecret** op in het geheim.

Typ de onderstaande opdrachten om in Key Vault een geheim te maken met de naam **AppSecret** waarin de waarde van **MySecret** wordt opgeslagen:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Als u de waarde in het geheim als tekst zonder opmaak wilt weergeven:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Met deze opdracht vraagt u de geheime gegevens op, inclusief de URI. Als u deze stappen hebt uitgevoerd, beschikt u over een URI voor een geheim in een Azure-sleutelkluis. Schrijf deze gegevens op. U hebt deze in een latere stap nog nodig.

## <a name="clone-the-repo"></a>De opslagplaats klonen

Kloon de opslagplaats zodat u een lokale kopie ervan hebt en de bron kunt bewerken. Voer hiervoor de volgende opdracht uit:

```
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Afhankelijkheden installeren

Hier installeren we de afhankelijkheden. Voer de volgende opdrachten uit: cd key-vault-node-quickstart npm install

In dit project worden twee knooppuntmodules gebruikt:

* [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) 
* [azure-keyvault](https://www.npmjs.com/package/azure-keyvault)

## <a name="publish-the-web-application-to-azure"></a>De webtoepassing publiceren in Azure

Hieronder vindt u de paar stappen die moeten worden uitgevoerd

- In de eerste stap wordt een plan voor een [Azure App Service](https://azure.microsoft.com/services/app-service/) gemaakt. In dit plan kunt u meerdere web-apps opslaan.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
- Vervolgens wordt een webtoepassing gemaakt. Vervang in het volgende voorbeeld <app_name> door een globaal unieke naam (geldige tekens zijn a-z, 0-9 en -). De runtime wordt ingesteld op NODE|6.9. Voer az webapp list-runtimes uit om alle ondersteunde runtimes te bekijken
    ```
    # Bash
    az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
    # PowerShell
    az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9"
    ```
    Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:
    ```
    {
      "availabilityState": "Normal",
      "clientAffinityEnabled": true,
      "clientCertEnabled": false,
      "cloningInfo": null,
      "containerSize": 0,
      "dailyMemoryTimeQuota": 0,
      "defaultHostName": "<app_name>.azurewebsites.net",
      "enabled": true,
      "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
      < JSON data removed for brevity. >
    }
    ```
    Blader naar de pas gemaakte web-app. U ziet een werkende web-app. Vervang <app-naam> door een unieke app-naam.

    ```
    http://<app name>.azurewebsites.net
    ```
    Met de bovenstaande opdracht wordt ook een door Git-ingeschakelde app gemaakt waarmee u vanuit de lokale Git in Azure kunt implementeren. 
    De lokale Git wordt met de volgende URL geconfigureerd: https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git

- Een implementatiegebruiker maken Als de vorige opdracht is uitgevoerd, kunt u een externe Azure-instantie aan de lokale Git-opslagplaats toevoegen. Vervang <url> door de URL van de externe Git-instantie die u hebt verkregen via Git inschakelen voor uw app.

    ```
    git remote add azure <url>
    ```

## <a name="enable-managed-service-identity"></a>Beheerde service-identiteit inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. Managed Service Identity (MSI) levert Azure-services met een automatisch beheerde identiteit in Azure Active Directory (Azure AD), waarmee dit probleem eenvoudiger kan worden opgelost. U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

Voer de opdracht identity assign uit om de identiteit voor deze toepassing te maken:

```azurecli
az webapp identity assign --name <app_name> --resource-group "<YourResourceGroupName>"
```

U kunt dit trouwens ook doen door naar de portal te gaan en **Managed service identity** op**On** te zetten in de eigenschappen van de webtoepassing.

### <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Machtigingen toewijzen aan uw toepassing voor het lezen van geheimen uit Key Vault

Schrijf de uitvoer van de bovenstaande opdracht op of kopieer deze. Deze moet de volgende indeling hebben:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Voer deze opdracht uit met de naam van uw sleutelkluis en de waarde van PrincipalId die van hierboven is gekopieerd:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>De Node-app implementeren in Azure en de geheime waarde ophalen

Alles is nu ingesteld. Voer de volgende opdracht uit om de app in Azure te implementeren

```
git push azure master
```

Als u hierna naar https://<app_name>.azurewebsites.net bladert, ziet u de geheime waarde.
Controleer of u de naam <YourKeyVaultName> hebt vervangen door de naam van de kluis

## <a name="next-steps"></a>Volgende stappen

* [Startpagina van Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentatie voor Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
* [Azure SDK voor Node](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
* [Azure REST API-naslaginformatie](https://docs.microsoft.com/rest/api/keyvault/)