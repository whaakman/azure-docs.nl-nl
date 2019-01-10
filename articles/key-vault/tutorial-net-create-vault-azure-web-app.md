---
title: 'Zelfstudie: Azure Key Vault gebruiken met een Azure-web-app in .NET - Azure Key Vault | Microsoft Docs'
description: 'Zelfstudie: Een ASP.NET Core-toepassing configureren voor het lezen van een geheim in Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: b6dbae0f721983920c2073927fff74100528678e
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53998792"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Zelfstudie: Azure Key Vault gebruiken met een Azure-web-app in .NET

Met Azure Key Vault kunt u geheimen beveiligen, zoals API-sleutels en databaseverbindingsreeksen. Het biedt u toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie leert u hoe u een Azure-webtoepassing maakt waarmee gegevens uit een Azure-sleutelkluis kunnen worden gelezen. Tijdens dit proces wordt gebruikgemaakt van beheerde identiteiten voor Azure-resources. Zie [Azure App Service](../app-service/overview.md) voor meer informatie over Azure-webtoepassingen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in de sleutelkluis.
> * Een geheim ophalen uit de sleutelkluis.
> * Een Azure-webtoepassing maken.
> * Schakel een [Beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) in voor de web-app.
> * De vereiste machtigingen verlenen aan de webtoepassing om gegevens te lezen uit de sleutelkluis.
> * De webtoepassing uitvoeren in Azure.

Lees, vóórdat u verdergaat, eerst [Basisconcepten van Key Vault](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Vereisten

* In Windows:
  * [.NET Core 2.1 SDK of hoger](https://www.microsoft.com/net/download/windows)

* Op de Mac:
  * [Visual Studio voor Mac](https://visualstudio.microsoft.com/vs/mac/)

* Alle platformen:
  * [Git](https://git-scm.com/downloads)
  * Een Azure-abonnement <br />(Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.)
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)-versie 2.0.4 of hoger, beschikbaar voor Windows, Mac en Linux
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="managed-service-identity-and-how-it-works"></a>Managed Service Identity en hoe het werkt

In Azure Key Vault worden referenties veilig opgeslagen zodat ze zich niet in uw code bevinden. U moet u echter verifiëren bij Azure Key Vault om uw sleutels op te halen. En om u te verifiëren bij Key Vault, hebt u een referentie nodig. Dit is een klassiek bootstrap-dilemma. In MSI (Managed Service Identity) is dit probleem opgelost doordat er een _bootstrap-identiteit_ wordt geboden dat het proces vereenvoudigt.

Wanneer u MSI voor een Azure-service inschakelt (bijvoorbeeld: Virtual Machines, App Service of Functions), maakt Azure een [service-principal](key-vault-whatis.md#basic-concepts). Dit gebeurt voor het exemplaar van de service in Azure AD (Azure Active Directory). De referenties voor de service-principal worden in dit exemplaar geplaatst.

![MSI-diagram](media/MSI.png)

Vervolgens wordt met de ​​code een lokale metagegevensservice aangeroepen die beschikbaar is in de Azure-resource, om een ​​toegangstoken te verkrijgen. Uw code gebruikt het toegangstoken dat wordt verkregen van het lokale MSI_ENDPOINT zich te authenticeren bij een Azure Key Vault-service.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Als u zich met behulp van Azure CLI wilt aanmelden bij Azure, voert u het volgende in:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

1. Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create).
1. Selecteer de naam van een resourcegroep en vul de tijdelijke aanduiding in. In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio US - west:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

U gebruikt deze resourcegroep in de hele zelfstudie.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Als u een sleutelkluis in de resourcegroep wilt maken, biedt u de volgende informatie:

* Naam van de sleutelkluis: een tekenreeks van 3 tot en met 24 tekens, die alleen cijfers, letters en afbreekstreepjes mag bevatten (bijvoorbeeld: 0-9, a-z, A-Z, en -)
* Naam van de resourcegroep
* Locatie: **US - west**

Voer de volgende opdracht in Azure CLI in:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="add-a-secret-to-the-key-vault"></a>Een geheim toevoegen aan de sleutelkluis

U kunt nu een geheim toevoegen. Dit kan een SQL-verbindingsreeks zijn of enige andere gegevens die u zowel veilig als beschikbaar wilt houden op de toepassing.

Typ de volgende opdracht om een geheim te maken in de sleutelkluis met de naam **AppGeheim**. Met dit geheim wordt de waarde **MijnGeheim** opgeslagen.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Als u de waarde in het geheim wilt weergeven als tekst zonder opmaak, voert u de volgende opdracht in:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Met deze opdracht vraagt u de geheime gegevens op, inclusief de URI. Wanneer u deze stappen hebt uitgevoerd, beschikt u over een URI voor een geheim in een sleutelkluis. Noteer deze informatie. U hebt deze in een latere stap nodig.

## <a name="create-a-net-core-web-app"></a>Een .NET Core-web-app maken

Volg deze [zelfstudie](../app-service/app-service-web-get-started-dotnet.md) om een .NET Core-web-app te maken en deze te **publiceren** in Azure. U kunt ook de volgende video bekijken:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>De oplossing openen en bewerken

1. Blader naar het bestand **Pagina’s** > **About.cshtml.cs**.
2. Installeer deze NuGet-pakketten:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importeer de volgende code in het bestand About.cshtml.cs:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

4. De code in de klasse AboutModel moet er als volgt uitzien:

   ```csharp
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

1. Selecteer **Fouten opsporen** > **Starten** met/zonder foutopsporing in het hoofdmenu van Visual Studio 2017. 
1. Wanneer de browser wordt weergegeven, gaat u naar de pagina **Over**.
1. De waarde voor **AppGeheim** wordt weergegeven.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Een beheerde identiteit voor de web-app inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere geheimen, maar uw code moet worden geverifieerd bij Key Vault om ze op te halen. Het [Overzicht van beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) helpt bij het oplossen van dit probleem door Azure-services een automatisch beheerde identiteit in Azure AD te geven. U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

1. Voer in Azure CLI de opdracht assign-identity uit om de identiteit voor deze toepassing te maken:

   ```azurecli

   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"

   ```

   >[!NOTE]
   >U moet \<YourAppName\> vervangen door de naam van de gepubliceerde app in Azure. Als de naam van uw gepubliceerde app bijvoorbeeld **MijnGeweldigeApp.azurewebsites.net** is, vervangt u \<YourAppName\> door **MijnGeweldigeApp**.

1. Noteer de `PrincipalId` wanneer u de toepassing publiceert in Azure. De uitvoer van de opdracht in stap 1 moet de volgende indeling hebben:

   ```json
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
   ```

>[!NOTE]
>U kunt dit ook doen door naar de [portal](https://portal.azure.com) te gaan en de instelling **Identeit/Systeem toegewezen** op **Aan** te zetten in de eigenschappen van de webtoepassing.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Machtigingen toewijzen aan uw toepassing voor het lezen van geheimen uit Key Vault

Vervang \<YourKeyVaultName\> door de naam van de sleutelkluis en vervang \<PrincipalId\> door de waarde van de **PrincipalId** in de volgende opdracht:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Deze opdracht geeft de identiteit (MSI) van de App Service-machtigingen op voor de bewerkingen **get** en **list** in de sleutelkluis.

## <a name="publish-the-web-application-to-azure"></a>De webtoepassing publiceren in Azure

Publiceer de web-app nogmaals in Azure om te controleren of de geheime waarde kan worden opgehaald met behulp van de live-web-app.

1. In Visual Studio selecteert u het project **key-vault-dotnet-core-quickstart**.
2. Selecteer **Publiceren** > **Start**.
3. Selecteer **Maken**.

Wanneer u de toepassing uitvoert, ziet u, als het goed is, dat de geheime waarde wordt opgehaald.

U hebt nu een web-app gemaakt in .NET waarmee de bijbehorende geheimen worden opgeslagen in en opgehaald uit Key Vault.

## <a name="next-steps"></a>Volgende stappen

>[!div class="nextstepaction"]
>[Gids voor Azure Key Vault-ontwikkelaars](https://docs.microsoft.com/azure/key-vault/key-vault-developers-guide)
