---
title: 'Snelstart: Een geheim uit Azure Key Vault instellen en ophalen met behulp van een Node-web-app | Microsoft Docs'
description: 'Snelstart: Een geheim uit Azure Key Vault instellen en ophalen met behulp van een .NET-web-app'
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: a53130dcc489764ce9284f15b8de0de37e0827e5
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686667"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-net-web-app"></a>Snelstart: Een geheim uit Azure Key Vault instellen en ophalen met behulp van een .NET-web-app

In deze snelstart voert u de stappen uit die nodig zijn om een Azure-webtoepassing gegevens te laten lezen uit Azure Key Vault met behulp van beheerde identiteiten voor Azure-resources. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in de sleutelkluis.
> * Een geheim ophalen uit de sleutelkluis.
> * Een Azure-webtoepassing maken.
> * Schakel een [beheerde service-identiteit](../active-directory/managed-identities-azure-resources/overview.md) in voor de web-app.
> * De vereiste machtigingen verlenen aan de webtoepassing om gegevens te lezen uit de sleutelkluis.

Lees voordat we verdergaan de [basisconcepten](key-vault-whatis.md#basic-concepts).

>[!NOTE]
>Key Vault is een centrale opslagplaats voor het opslaan van geheimen via een programma. Maar hiervoor moeten toepassingen en gebruikers eerst worden geverifieerd bij Key Vault, wat betekent dat ze een geheim moeten presenteren. Als u de aanbevolen procedures voor beveiliging wilt volgen, moet dit eerste geheim periodiek worden gerouleerd. 
>
>Met [beheerde service-identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) krijgen toepassingen die in Azure worden uitgevoerd, een identiteit die automatisch door Azure wordt beheerd. Dit helpt bij het oplossen van het *probleem van het introduceren van geheimen* zodat gebruikers en toepassingen aanbevolen procedures kunnen volgen en zich geen zorgen hoeven maken over het rouleren van het eerste geheim.

## <a name="prerequisites"></a>Vereisten

* In Windows:
  * [Visual Studio 2017 versie 15.7.3 of hoger](https://www.microsoft.com/net/download/windows) met de volgende workloads:
    * ASP.NET-ontwikkeling en webontwikkeling
    * Platformoverschrijdende ontwikkelmogelijkheden van .NET Core
  * [.NET Core 2.1 SDK of hoger](https://www.microsoft.com/net/download/windows)

* Op de Mac:
  * Zie [Nieuwe functies in Visual Studio voor Mac](https://visualstudio.microsoft.com/vs/mac/).

* Alle platformen:
  * Git ([downloaden](https://git-scm.com/downloads)).
  * Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versie 2.0.4 of hoger. Deze is beschikbaar voor Windows, Mac en Linux.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Als u zich bij Azure wilt aanmelden met de Azure CLI, voert u het volgende in:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Selecteer de naam van een resourcegroep en vul de tijdelijke aanduiding in.
In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio US - oost:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

De resourcegroep die u net hebt gemaakt, wordt overal in dit artikel gebruikt.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Vervolgens maakt u een sleutelkluis in de resourcegroep die u in de vorige stap hebt gemaakt. Geef de volgende informatie op:

* Naam van de sleutelkluis: de naam moet een tekenreeks zijn met 3-24 tekens en mag alleen (0-9, a-z, A-Z en -) bevatten.
* Naam van de resourcegroep.
* Locatie: **US - oost**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="add-a-secret-to-the-key-vault"></a>Een geheim toevoegen aan de sleutelkluis

We gaan een geheim toevoegen om te laten zien hoe dit werkt. U wilt bijvoorbeeld een SQL-verbindingsreeks opslaan of andere gegevens die u veilig wilt bewaren, maar wel beschikbaar wilt stellen aan uw toepassing.

Typ de volgende opdrachten om een geheim te maken in de sleutelkluis met de naam **AppGeheim**. Met dit geheim wordt de waarde **MijnGeheim** opgeslagen.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Als u de waarde in het geheim als tekst zonder opmaak wilt weergeven:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Met deze opdracht vraagt u de geheime gegevens op, inclusief de URI. Wanneer u deze stappen hebt uitgevoerd, beschikt u over een URI voor een geheim in een sleutelkluis. Noteer deze informatie. U hebt deze in een latere stap nodig.

## <a name="clone-the-repo"></a>De opslagplaats klonen

Kloon de opslagplaats om een lokale kopie te maken waarin u de bron kunt bewerken. Voer de volgende opdracht uit:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>De oplossing openen en bewerken

Bewerk het bestand program.cs om het voorbeeld met de naam van uw specifieke sleutelkluis uit te voeren:

1. Ga naar de map key-vault-dotnet-core-quickstart.
2. Open het bestand key-vault-dotnet-core-quickstart.sln in Visual Studio 2017.
3. Open het bestand Program.cs en vervang de tijdelijke aanduiding *YourKeyVaultName* door de naam van de sleutelkluis die u eerder hebt gemaakt.

Deze oplossing maakt gebruik van NuGet-bibliotheken van [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

## <a name="run-the-app"></a>De app uitvoeren

Selecteer **Fouten opsporen** > **Starten** zonder foutopsporing in het hoofdmenu van Visual Studio 2017. Wanneer de browser wordt weergegeven, gaat u naar de pagina **Over**. De waarde voor **AppGeheim** wordt weergegeven.

## <a name="publish-the-web-application-to-azure"></a>De webtoepassing publiceren in Azure

Publiceer deze app naar Azure om deze live als web-app in actie te zien en om te controleren of u de geheime waarde kunt ophalen:

1. In Visual Studio selecteert u het project **key-vault-dotnet-core-quickstart**.
2. Selecteer **Publiceren** > **Start**.
3. Maak een nieuwe **App Service** en selecteer **Publiceren**.
4. Wijzig de naam van de app in **keyvaultdotnetcorequickstart**.
5. Selecteer **Maken**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-a-managed-identity-for-the-web-app"></a>Een beheerde identiteit voor de web-app inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. In [Overzicht van beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) wordt het oplossen van dit probleem eenvoudiger gemaakt door Azure-services een automatisch beheerde identiteit in Azure Active Directory (Azure AD) te geven. U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

1. Ga terug naar de Azure CLI.
2. Voer de opdracht identity assign uit om de identiteit voor deze toepassing te maken:

   ```azurecli
   az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
   ```

>[!NOTE]
>U kunt dit ook doen door naar de portal te gaan en de instelling **Identeit/Systeem toegewezen** **Aan** te zetten in de eigenschappen van de webtoepassing.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Machtigingen toewijzen aan uw toepassing voor het lezen van geheimen uit Key Vault

Maak een notitie van de uitvoer wanneer u de toepassing naar Azure publiceert. Gebruik hierbij de notatie:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Voer deze opdracht uit met de naam van uw sleutelkluis en de waarde van **PrincipalId**:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

Tijdens het uitvoeren van de toepassing ziet u nu de geheime waarde die is opgehaald. In de bovenstaande opdracht geeft u de Identity(MSI) van de App Service-machtigingen voor de bewerkingen **get** en **list** op uw Key Vault

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
* [Azure-SDK voor .NET](https://github.com/Azure/azure-sdk-for-net)
* [Azure REST API-naslaginformatie](https://docs.microsoft.com/rest/api/keyvault/)
