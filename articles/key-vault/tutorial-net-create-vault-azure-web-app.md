---
title: 'Zelfstudie: Azure Key Vault gebruiken met een Azure-web-app in .NET | Microsoft Docs'
description: In deze zelfstudie configureert u een ASP.NET core-toepassing een geheim lezen uit uw key vault.
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
ms.openlocfilehash: 2c2bb3e4064294bb8d4a63b009069fd6834ca31e
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370887"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Zelfstudie: Azure Key Vault gebruiken met een Azure-web-app in .NET

Met Azure Key Vault kunt u geheimen beveiligen, zoals API-sleutels en databaseverbindingsreeksen. Het biedt u toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie leert u hoe u een Azure-webtoepassing maakt waarmee gegevens uit een Azure-sleutelkluis kunnen worden gelezen. Tijdens dit proces wordt gebruikgemaakt van beheerde identiteiten voor Azure-resources. Zie [Azure App Service](../app-service/overview.md) voor meer informatie over Azure-webtoepassingen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Voeg een geheim toe aan de sleutelkluis.
> * Een geheim ophalen uit de sleutelkluis.
> * Een Azure-web-app maken.
> * Een beheerde identiteit voor de web-app inschakelen.
> * Machtiging voor de web-app toewijzen.
> * De WebApp uitvoeren op Azure.

Voordat u begint, Lees [Key Vault-basisconcepten](key-vault-whatis.md#basic-concepts). 

Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Vereisten

* Voor Windows: [.NET Core-SDK 2.1 of hoger](https://www.microsoft.com/net/download/windows)
* Voor Mac: [Visual Studio voor Mac](https://visualstudio.microsoft.com/vs/mac/)
* Voor Windows, Mac en Linux:
  * [Git](https://git-scm.com/downloads)
  * Voor deze zelfstudie moet u de Azure CLI lokaal uitvoeren. U moet de Azure CLI versie 2.0.4 of hoger is geïnstalleerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](https://review.docs.microsoft.com/cli/azure/install-azure-cli).
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="about-managed-service-identity"></a>Informatie over Managed Service Identity

Azure Key Vault worden referenties veilig opgeslagen, zodat ze niet worden weergegeven in uw code. U moet u echter verifiëren bij Azure Key Vault om uw sleutels op te halen. En om u te verifiëren bij Key Vault, hebt u een referentie nodig. Dit is een klassiek bootstrap-dilemma. In MSI (Managed Service Identity) is dit probleem opgelost doordat er een _bootstrap-identiteit_ wordt geboden dat het proces vereenvoudigt.

Wanneer u MSI voor een Azure-service, zoals virtuele Machines van Azure, Azure App Service of Azure Functions inschakelt, Azure maakt een [service-principal](key-vault-whatis.md#basic-concepts). MSI-bestand doet dit voor het exemplaar van de service in Azure Active Directory (Azure AD) en de referenties voor de service-principal injects naar dat exemplaar.

![MSI-diagram](media/MSI.png)

Vervolgens roept de code voor een toegangstoken, een lokale metadata-service die beschikbaar is op de Azure-resource. Uw code gebruikt het toegangstoken dat wordt verkregen van het lokale MSI-eindpunt om zich te authenticeren bij een Azure Key Vault-service.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Als u zich bij Azure wilt aanmelden met de Azure CLI, voert u het volgende in:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create).

Vervolgens selecteert u een Resourcegroepnaam en vul in de tijdelijke aanduiding. In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio US - west:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

U gebruikt deze resourcegroep in de hele zelfstudie.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Als u een sleutelkluis in de resourcegroep wilt maken, biedt u de volgende informatie:

* Key vault-naam: een reeks van 3 tot 24 tekens die mag alleen cijfers (0-9), letters (a-z, A-Z) en afbreekstreepjes (-)
* Naam van de resourcegroep
* Locatie: **US - west**

Voer de volgende opdracht in de Azure-CLI:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Op dit moment is uw Azure-account de enige die is gemachtigd voor het uitvoeren van bewerkingen op deze nieuwe kluis.

## <a name="add-a-secret-to-the-key-vault"></a>Een geheim toevoegen aan de sleutelkluis

U kunt nu een geheim toevoegen. Dit kan een SQL-verbindingsreeks zijn of enige andere gegevens die u zowel veilig als beschikbaar wilt houden op de toepassing.

Een geheim maken in de key vault met de naam **AppSecret**, voer de volgende opdracht: 

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Met dit geheim wordt de waarde **MijnGeheim** opgeslagen.

Als u wilt weergeven van de waarde die opgenomen in het geheim als tekst zonder opmaak, voer de volgende opdracht:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Met deze opdracht bevat de geheime informatie, met inbegrip van de URI. 

Wanneer u deze stappen hebt uitgevoerd, beschikt u over een URI voor een geheim in een sleutelkluis. Noteer deze informatie voor later gebruik in deze zelfstudie. 

## <a name="create-a-net-core-web-app"></a>Een .NET Core-web-app maken

Volg de instructies in een .NET Core-web-app maken en deze publiceren naar Azure, [een ASP.NET Core web-app maken in Azure](../app-service/app-service-web-get-started-dotnet.md). 

U kunt ook deze video bekijken:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>De oplossing openen en bewerken

1. Ga naar de **pagina's** > **About.cshtml.cs** bestand.

1. Installeer deze NuGet-pakketten:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)

1. Importeren van de volgende code om de *About.cshtml.cs* bestand:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

   Uw code in de klasse AboutModel moet er als volgt:

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
                /* The next four lines of code show you how to use AppAuthentication library to fetch secrets from your key vault */
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;
            }
            /* If you have throttling errors see this tutorial https://docs.microsoft.com/azure/key-vault/tutorial-net-create-vault-azure-web-app */
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
            }
        }

        // This method implements exponential backoff if there are 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory, which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```

## <a name="run-the-web-app"></a>De web-app uitvoeren

1. Selecteer in het hoofdmenu van Visual Studio 2017, **Debug** > **Start**, met of zonder foutopsporing. 
1. In de browser, Ga naar de **over** pagina.  
    De waarde voor **AppGeheim** wordt weergegeven.

## <a name="enable-a-managed-identity"></a>Een beheerde identiteit inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere geheimen, maar uw code moet worden geverifieerd bij Key Vault om ze op te halen. Het [Overzicht van beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) helpt bij het oplossen van dit probleem door Azure-services een automatisch beheerde identiteit in Azure AD te geven. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, met inbegrip van Key Vault, zonder dat om referenties in uw code weer te geven.

In de Azure-CLI voor het maken van de identiteit voor deze toepassing, voer de opdracht toewijzen-identity:

```azurecli
az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
```

Vervang \<Appnaam > met de naam van de gepubliceerde app op Azure.  
    Als de naam van uw gepubliceerde app is bijvoorbeeld **MyAwesomeapp.azurewebsites.net**, Vervang \<Appnaam > met **MyAwesomeapp**.

Noteer de `PrincipalId` wanneer u de toepassing publiceert in Azure. De uitvoer van de opdracht in stap 1 moet de volgende indeling hebben:

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

>[!NOTE]
>Met de opdracht in deze procedure is het equivalent van gaan de [Azure-portal](https://portal.azure.com) en schakelen tussen de **identiteit / systeem toegewezen** instelt op **op** in de web-App de eigenschappen.

## <a name="assign-permissions-to-your-app"></a>Machtigingen toewijzen aan uw app

Vervang \<YourKeyVaultName > met de naam van uw sleutelkluis, en vervang \<PrincipalId > door de waarde van de **PrincipalId** in de volgende opdracht:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Met deze opdracht geeft de identiteit (MSI) van de app service toestemming te doen **ophalen** en **lijst** bewerkingen op uw key vault.

## <a name="publish-the-web-app-to-azure"></a>De web-app publiceren in Azure

Uw WebApp publiceren in Azure opnieuw om te controleren of dat uw live web-app kan worden gebruikt voor het ophalen van de geheime waarde.

1. In Visual Studio selecteert u het project **key-vault-dotnet-core-quickstart**.
2. Selecteer **Publiceren** > **Start**.
3. Selecteer **Maken**.

Wanneer u de toepassing uitvoert, ziet u, als het goed is, dat de geheime waarde wordt opgehaald.

Nu hebt u een web-app gemaakt in .NET die worden opgeslagen en de geheimen ophaalt uit uw key vault.

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer ze niet meer nodig hebt zijn, kunt u de virtuele machine en uw key vault verwijderen.

## <a name="next-steps"></a>Volgende stappen

>[!div class="nextstepaction"]
>[Gids voor Azure Key Vault-ontwikkelaars](https://docs.microsoft.com/azure/key-vault/key-vault-developers-guide)
