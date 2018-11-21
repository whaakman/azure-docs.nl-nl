---
title: Zelfstudie - Azure Key Vault gebruiken met Azure Web App in .NET | Microsoft Docs
description: 'Zelfstudie: Een ASP.NET Core-toepassing configureren voor het lezen van een geheim uit Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 6e30a181bc69678051e6d6469a2f1c533de811df
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686208"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-web-app-in-net"></a>Zelfstudie: Azure Key Vault gebruiken met Azure Web App in .NET

Azure Key Vault helpt u bij het beveiligen van geheimen zoals API-sleutels, databaseverbindingsreeksen die nodig zijn voor toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie voert u de stappen uit die nodig zijn om een Azure-webtoepassing gegevens te laten lezen uit Azure Key Vault met behulp van beheerde identiteiten voor Azure-resources. Deze zelfstudie is gebaseerd op [Azure Web Apps](../app-service/app-service-web-overview.md). Hieronder leert u:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in de sleutelkluis.
> * Een geheim ophalen uit de sleutelkluis.
> * Een Azure-webtoepassing maken.
> * Schakel een [Beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) in voor de web-app.
> * De vereiste machtigingen verlenen aan de webtoepassing om gegevens te lezen uit de sleutelkluis.
> * De webtoepassing uitvoeren in Azure

Lees voordat we verdergaan de [basisconcepten](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Vereisten

* In Windows:
  * [.NET Core 2.1 SDK of hoger](https://www.microsoft.com/net/download/windows)

* Op de Mac:
  * Zie [Nieuwe functies in Visual Studio voor Mac](https://visualstudio.microsoft.com/vs/mac/).

* Alle platformen:
  * Git ([downloaden](https://git-scm.com/downloads)).
  * Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versie 2.0.4 of hoger. Deze is beschikbaar voor Windows, Mac en Linux.
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Wat is Managed Service Identity en hoe werkt het?
Voordat we verder gaan, moeten we het over MSI hebben. Azure Key Vault kan referenties veilig opslaan zodat ze zich niet in uw code bevinden, maar om ze op te halen moet u zich authenticeren bij Azure Key Vault. En om u te authenticeren bij Key Vault, hebt u een referentie nodig! Een klassiek bootstrap-probleem. Door de magie van Azure en Azure AD biedt MSI een 'bootstrap-identiteit' die het veel eenvoudiger maakt om dingen op gang te brengen.

Het werkt als volgt. Wanneer u MSI inschakelt voor een Azure-service zoals Virtual Machines, App Service of Functions, maakt Azure een [service-principal](key-vault-whatis.md#basic-concepts) voor de instantie van de service in Azure Active Directory, en injecteert de referenties voor de service-principal in de instantie van de service. 

![MSI](media/MSI.png)

Vervolgens roept uw ​​code een lokale metadataservice aan die beschikbaar is op de Azure-resource om een ​​toegangstoken te verkrijgen.
Uw code gebruikt het toegangstoken dat wordt verkregen van het lokale MSI_ENDPOINT zich te authenticeren bij een Azure Key Vault-service. 

Nu gaan we beginnen met de zelfstudie.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Als u zich bij Azure wilt aanmelden met de Azure CLI, voert u het volgende in:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Selecteer de naam van een resourcegroep en vul de tijdelijke aanduiding in.
In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio US - west:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

De resourcegroep die u net hebt gemaakt, wordt overal in dit artikel gebruikt.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Vervolgens maakt u een sleutelkluis in de resourcegroep die u in de vorige stap hebt gemaakt. Geef de volgende informatie op:

* Naam van de sleutelkluis: de naam moet een tekenreeks zijn met 3-24 tekens en mag alleen (0-9, a-z, A-Z en -) bevatten.
* Naam van de resourcegroep.
* Locatie: **US - west**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
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

## <a name="create-a-net-core-web-app"></a>Maak een .NET Core-web-app

Volg deze [zelfstudie](../app-service/app-service-web-get-started-dotnet.md) om een .NET Core-web-app te maken en deze naar Azure te **publiceren** **OF** bekijk de onderstaande video
> [!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>De oplossing openen en bewerken

1. Navigeer naar Pagina's > bestand About.cshtml.cs.
2. Installeer deze 2 Nuget-pakketten
    - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
    - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importeer het volgende in het bestand About.cshtml.cs

    ```
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
    ```
4. Uw code in de klasse AboutModel zou er als volgt moeten uitzien
    ```
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The below 4 lines of code shows you how to use AppAuthentication library to fetch secrets from your Key Vault*/
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;             

                /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
                do
                {
                    long waitTime = Math.Min(getWaitTime(retries), 2000000);
                    secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                    retry = false;
                } 
                while(retry && (retries++ < 10));
            }
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
                if((int)keyVaultException.Response.StatusCode == 429)
                    retry = true;
            }            
        }

        // This method implements exponential backoff incase of 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```


## <a name="run-the-app"></a>De app uitvoeren

Selecteer **Fouten opsporen** > **Starten** met/zonder foutopsporing in het hoofdmenu van Visual Studio 2017. Wanneer de browser wordt weergegeven, gaat u naar de pagina **Over**. De waarde voor **AppGeheim** wordt weergegeven.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Een beheerde identiteit voor de web-app inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. In [Overzicht van beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) wordt het oplossen van dit probleem eenvoudiger gemaakt door Azure-services een automatisch beheerde identiteit in Azure Active Directory (Azure AD) te geven. U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

1. Ga terug naar de Azure CLI.
2. Voer de opdracht identity assign uit om de identiteit voor deze toepassing te maken: 

   ```azurecli
   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
   ```
   Houd er rekening mee dat u <YourAppName> moet vervangen door de naam van de gepubliceerde app op Azure, dus als uw gepubliceerde app-naam MyAwesomeapp.azurewebsites.net is, vervang dan <YourAppName> door MyAwesomeapp
 
 De uitvoer van de bovenstaande opdracht ziet er zo uit Noteer de PrincipalId wanneer u de toepassing publiceert naar Azure. Gebruik hierbij de notatie:
   ```
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
  ```
>[!NOTE]
>U kunt dit ook doen door naar de [portal](https://portal.azure.com) te gaan en de instelling **Identeit/Systeem toegewezen** op **Aan** te zetten in de eigenschappen van de webtoepassing.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Machtigingen toewijzen aan uw toepassing voor het lezen van geheimen uit Key Vault
        
Voer deze opdracht uit met de naam van uw sleutelkluis en de waarde van **PrincipalId**:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

## <a name="publish-the-web-application-to-azure"></a>De webtoepassing publiceren in Azure

Publiceer deze app nogmaals naar Azure om deze live als web-app in actie te zien en om te controleren of u de geheime waarde kunt ophalen.

1. In Visual Studio selecteert u het project **key-vault-dotnet-core-quickstart**.
2. Selecteer **Publiceren** > **Start**.
3. Selecteer **Maken**.

In de bovenstaande opdracht geeft u de Identity(MSI) van de App Service-machtigingen voor de bewerkingen **get** en **list** op uw Key Vault. <br />
Tijdens het uitvoeren van de toepassing ziet u nu de geheime waarde die is opgehaald. 

En dat is alles. U hebt nu een Web-App in .NET gemaakt die zijn geheimen opslaat in Key Vault en daaruit ophaalt.
