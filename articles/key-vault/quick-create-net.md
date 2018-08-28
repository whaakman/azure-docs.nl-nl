---
title: 'Snelstart: Een geheim uit Azure Key Vault instellen en ophalen met behulp van een Node-web-app | Microsoft Docs'
description: 'Snelstart: Een geheim uit Azure Key Vault instellen en ophalen met behulp van een Node-web-app'
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 0188d06e5c58287e1040f6a15456d3ffe291b04a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42022748"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-net-web-app"></a>Snelstart: Een geheim uit Azure Key Vault instellen en ophalen met behulp van .NET Web App

In deze snelstart gaat u de stappen uitvoeren die nodig zijn om een Azure-webtoepassing met behulp van beheerde service-identiteiten gegevens te laten lezen uit Key Vault. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in Key Vault.
> * Een geheim lezen uit Key Vault.
> * Een Azure-webtoepassing maken.
> * [Beheerde service-identiteiten inschakelen](../active-directory/managed-service-identity/overview.md).
> * De vereiste machtigingen verlenen aan de webtoepassing om gegevens te lezen uit Key Vault.

Lees voordat we verdergaan de [basisconcepten](key-vault-whatis.md#basic-concepts).

>[!NOTE]
Om te begrijpen waarom de onderstaande zelfstudie de aanbevolen procedure is, moeten we enkele concepten begrijpen. Key Vault is een centrale opslagplaats voor het opslaan van geheimen via een programma. Maar hiervoor moeten toepassingen/gebruikers eerst worden geverifieerd bij Key Vault, dat wil zeggen een geheim presenteren. Als u de aanbevolen procedures voor beveiliging wilt volgen, dient dit eerste geheim ook periodiek te worden gerouleerd. Maar met [Managed Service Identity](../active-directory/managed-service-identity/overview.md) krijgen toepassingen die worden uitgevoerd in Azure een identiteit die automatisch wordt beheerd door Azure. Dit helpt bij het oplossen van het **probleem van het introduceren van geheimen** waar gebruikers/toepassingen aanbevolen procedures kunnen volgen en zich geen zorgen hoeven maken over het rouleren van het eerste geheim

## <a name="prerequisites"></a>Vereisten

* In Windows:
  * [Visual Studio 2017 versie 15.7.3 of hoger](https://www.microsoft.com/net/download/windows) met de volgende workloads:
    * ASP.NET-ontwikkeling en webontwikkeling
    * Platformoverschrijdende ontwikkelmogelijkheden van .NET Core
  * [.NET Core 2.1 SDK of hoger](https://www.microsoft.com/net/download/windows)

* Op de Mac:
  * https://visualstudio.microsoft.com/vs/mac/

* Alle platformen:
  * GIT [hier](https://git-scm.com/downloads) downloaden.
  * Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) U hebt Azure CLI versie 2.0.4 of hoger nodig. Deze is beschikbaar voor Windows, Mac en Linux

## <a name="login-to-azure"></a>Aanmelden bij Azure

   Gebruik de volgende opdracht om u aan te melden bij Azure met behulp van de CLI:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az-group-create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Selecteer de naam van een resourcegroep en vul de tijdelijke aanduiding in.
In het volgende voorbeeld wordt een resourcegroep met de naam *<YourResourceGroupName>* gemaakt op de locatie *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

De resourcegroep die u zojuist hebt gemaakt, wordt overal in dit artikel gebruikt.

## <a name="create-an-azure-key-vault"></a>Een Azure Key Vault maken

Vervolgens maakt u een sleutelkluis in de resourcegroep die u in de vorige stap hebt gemaakt. Geef de volgende informatie op:

* Naam van de kluis: **selecteer hier de naam van de sleutelkluis**. De naam van een sleutelkluis moet een tekenreeks van 3 tot 24 tekens lang zijn (0-9, a-z, A-Z en -).
* De naam van de resourcegroep: **Selecteer hier de naam van de resourcegroep**.
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

Met deze opdracht vraagt u de geheime gegevens op, inclusief de URI. Als u deze stappen hebt uitgevoerd, beschikt u over een URI voor een geheim in een Azure-sleutelkluis. Noteer deze informatie. U hebt deze in een latere stap nog nodig.

## <a name="clone-the-repo"></a>De opslagplaats klonen

Kloon de opslagplaats zodat u een lokale kopie ervan hebt en de bron kunt bewerken. Voer hiervoor de volgende opdracht uit:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>De oplossing openen en bewerken

Bewerk het bestand program.cs om zodat het voorbeeld met de naam van uw specifieke sleutelkluis wordt uitgevoerd.

1. Navigeer naar de map key-vault-dotnet-core-quickstart.
2. Open het bestand key-vault-dotnet-core-quickstart.sln in Visual Studio 2017.
3. Open het bestand Program.cs en vervang de tijdelijke aanduiding <YourKeyVaultName> door de naam van de sleutelkluis die u eerder hebt gemaakt.

Deze oplossing maakt gebruik van NuGet-bibliotheken van [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

## <a name="run-the-app"></a>De app uitvoeren

Kies in het hoofdmenu van Visual Studio 2017 'Fouten opsporen > Starten zonder foutopsporing'. Wanneer de browser wordt weergegeven, gaat u naar de pagina Over. De waarde voor AppSecret wordt weergegeven.

## <a name="publish-the-web-application-to-azure"></a>De webtoepassing publiceren in Azure

Deze app wordt naar Azure gepubliceerd om deze live als web-app in actie te zien, en ook om er zeker van te zijn dat de geheime waarde wordt opgehaald.

1. In Visual Studio selecteert u **key-vault-dotnet-core-quickstart** Project.
2. Selecteer **Publish** en daarna **Start**.
3. Maak een nieuwe **App Service** en selecteer **Publish**.
4. Wijzig de naam van de app in 'keyvaultdotnetcorequickstart'.
5. Selecteer **Maken**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-managed-service-identities-msi"></a>Managed Service Identity (MSI) inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Azure Key Vault om ze op te halen. Managed Service Identity (MSI) maakt dit eenvoudiger door Azure-services van een automatisch beheerde identiteit in Azure Active Directory (Azure AD) te voorzien. U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

1. Ga terug naar de Azure CLI.
2. Voer de opdracht identity assign uit om de identiteit voor deze toepassing te maken:

```azurecli
az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
```

>[!NOTE]
>U kunt dit trouwens ook doen door naar de portal te gaan en **Managed service identity** op**On** te zetten in de eigenschappen van de webtoepassing.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Machtigingen toewijzen aan uw toepassing voor het lezen van geheimen uit Key Vault

Maak een notitie van de uitvoer wanneer u [de toepassing naar Azure publiceert][]. Gebruik hierbij de notatie:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Voer deze opdracht uit met de naam van uw sleutelkluis en de waarde van PrincipalId die van hierboven is gekopieerd:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get

```

**Tijdens het uitvoeren van de toepassing ziet u nu de geheime waarde die is opgehaald**

## <a name="next-steps"></a>Volgende stappen

* [Startpagina van Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentatie voor Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
* [Azure-SDK voor .NET](https://github.com/Azure/azure-sdk-for-net)
* [Azure REST API-naslaginformatie](https://docs.microsoft.com/rest/api/keyvault/)
