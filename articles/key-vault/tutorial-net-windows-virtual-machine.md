---
title: 'Zelfstudie: Azure Key Vault gebruikt met een Windows-machine in .NET | Microsoft Docs'
description: In deze zelfstudie configureert u een ASP.NET core-toepassing een geheim lezen uit uw key vault.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 3bb4647b39a276e2dd54260c17eca1d700d5ba16
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67874991"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-net"></a>Zelfstudie: Azure Key Vault gebruiken met een Windows-machine in .NET

Azure Key Vault helpt u bij het beveiligen van geheimen zoals API-sleutels, de tekenreeksen voor databaseverbindingen moet u toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie leert u hoe u een consoletoepassing om informatie te lezen uit Azure Key Vault. Om dit te doen, kunt u beheerde identiteiten gebruiken voor Azure-resources. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een resourcegroep.
> * Een sleutelkluis maken.
> * Voeg een geheim toe aan de sleutelkluis.
> * Een geheim ophalen uit de sleutelkluis.
> * Een virtuele Azure-machine maken.
> * Een [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) inschakelen voor de virtuele machine.
> * Machtigingen toewijzen aan de identiteit van de virtuele machine.

Voordat u begint, Lees [Key Vault-basisconcepten](key-vault-whatis.md#basic-concepts). 

Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Vereisten

Voor Windows, Mac en Linux:
  * [Git](https://git-scm.com/downloads)
  * Voor deze zelfstudie moet u de Azure CLI lokaal uitvoeren. U moet de Azure CLI versie 2.0.4 of hoger is geïnstalleerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](https://review.docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="about-managed-service-identity"></a>Informatie over Managed Service Identity

Azure Key Vault worden referenties veilig opgeslagen, zodat ze niet worden weergegeven in uw code. U moet u echter verifiëren bij Azure Key Vault om uw sleutels op te halen. En om u te verifiëren bij Key Vault, hebt u een referentie nodig. Dit is een klassiek bootstrap-dilemma. In MSI (Managed Service Identity) is dit probleem opgelost doordat er een _bootstrap-identiteit_ wordt geboden dat het proces vereenvoudigt.

Wanneer u MSI voor een Azure-service, zoals virtuele Machines van Azure, Azure App Service of Azure Functions inschakelt, Azure maakt een [service-principal](key-vault-whatis.md#basic-concepts). MSI-bestand doet dit voor het exemplaar van de service in Azure Active Directory (Azure AD) en de referenties voor de service-principal injects naar dat exemplaar. 

![MSI](media/MSI.png)

Vervolgens roept de code voor een toegangstoken, een lokale metadata-service die beschikbaar is op de Azure-resource. Om te verifiëren met een Azure Key Vault-service, wordt uw code gebruikt het toegangstoken dat wordt opgehaald uit het lokale eindpunt van de MSI-bestand. 

## <a name="create-resources-and-assign-permissions"></a>Resources maken en machtigingen toewijzen

Voordat u code schrijven moet u enkele resources maken, een geheim in uw key vault plaatsen en machtigingen toe te wijzen.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Als u zich met behulp van Azure CLI wilt aanmelden bij Azure, voert u het volgende in:

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create). 

In dit voorbeeld wordt een resourcegroep gemaakt in de locatie VS-West:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Uw nieuwe resourcegroep wordt gebruikt in deze zelfstudie.

### <a name="create-a-key-vault-and-populate-it-with-a-secret"></a>Een sleutelkluis maken en deze vullen met een geheim

Een sleutelkluis maken in de resourcegroep door te geven de [az keyvault maken](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) opdracht met de volgende informatie:

* Key vault-naam: een reeks van 3 tot 24 tekens die mag alleen cijfers (0-9), letters (a-z, A-Z) en afbreekstreepjes (-)
* Naam van de resourcegroep
* Locatie: **US - west**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Op dit moment is uw Azure-account de enige die is gemachtigd voor het uitvoeren van bewerkingen op deze nieuwe sleutelkluis.

Nu een geheim toevoegen aan uw sleutelkluis met de [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) opdracht


Een geheim maken in de key vault met de naam **AppSecret**, voer de volgende opdracht:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Met dit geheim wordt de waarde **MijnGeheim** opgeslagen.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Een virtuele machine maken met behulp van een van de volgende methoden:

* [De Azure CLI](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli)
* [PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)
* [Azure Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

### <a name="assign-an-identity-to-the-vm"></a>Een identiteit toewijzen aan de virtuele machine
Maken van een systeem toegewezen identiteit voor de virtuele machine met de [az vm identiteit toewijzen](/cli/azure/vm/identity?view=azure-cli-latest#az-vm-identity-assign) opdracht:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Houd er rekening mee de door het systeem toegewezen identiteit die wordt weergegeven in de volgende code. De uitvoer van de voorgaande opdracht zou zijn: 

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

### <a name="assign-permissions-to-the-vm-identity"></a>Machtigingen toewijzen aan de identiteit van VM
Wijs de eerder gemaakte Identiteitsmachtigingen voor uw key met Vault de [az keyvault-beleid instellen](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) opdracht:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

### <a name="sign-in-to-the-virtual-machine"></a>Meld u aan de virtuele machine

Als u wilt aanmelden bij de virtuele machine, volg de instructies in [verbinding maken met en meld u aan met een Azure-machine waarop Windows wordt uitgevoerd](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="set-up-the-console-app"></a>Instellen van de console-app

Een console-app maken en installeer de vereiste pakketten met behulp van de `dotnet` opdracht.

### <a name="install-net-core"></a>.NET Core installeren

Als u wilt installeren .NET Core, gaat u naar de [.NET downloads](https://www.microsoft.com/net/download) pagina.

### <a name="create-and-run-a-sample-net-app"></a>Maken en uitvoeren van een .NET-voorbeeld-app

Open een opdrachtprompt.

U kunt "Hallo wereld" in de console afdrukken door het uitvoeren van de volgende opdrachten:

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

### <a name="install-the-packages"></a>De pakketten installeren

 Installeer de .NET-pakketten zijn vereist voor deze Quick Start vanuit het consolevenster:

 ```console
dotnet add package System.IO;
dotnet add package System.Net;
dotnet add package System.Text;
dotnet add package Newtonsoft.Json;
dotnet add package Newtonsoft.Json.Linq;
```

## <a name="edit-the-console-app"></a>De console-app bewerken

Open de *Program.cs* bestand en voeg deze pakketten toe:

```csharp
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Bewerk het klassebestand bevat de code in het volgende proces voor verificatie in twee stappen:

1. Haal een token uit het lokale MSI-eindpunt op de virtuele machine op. In dat geval ophaalt een token ook uit Azure AD.
1. Het token doorgegeven aan uw key vault en vervolgens uw geheim ophalen. 

```csharp
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from the local (URI) Managed Service Identity endpoint, which in turn fetches it from Azure AD
            var token = GetToken();

            // Step 2: Fetch the secret value from your key vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }

        static string FetchSecretValueFromKeyVault(string token)
        {
            WebRequest kvRequest = WebRequest.Create("https://<YourVaultName>.vault.azure.net/secrets/<YourSecretName>?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```

In de voorgaande code ziet u hoe u bewerkingen met Azure Key Vault uitvoert op een virtuele Windows-machine.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer ze niet meer nodig hebt zijn, verwijdert u de virtuele machine en uw key vault.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [REST-API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
