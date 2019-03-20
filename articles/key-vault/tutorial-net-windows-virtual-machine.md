---
title: 'Zelfstudie: Azure Key Vault gebruikt met een Windows-machine in .NET | Microsoft Docs'
description: In deze zelfstudie configureert u een ASP.NET core-toepassing een geheim lezen uit uw key vault.
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: c66a7d7af2a73e26878b92f34e0f42ce0b3ae7f2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57437494"
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

U uw zojuist gemaakte resource-groep in deze zelfstudie.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Voor het maken van een key vault in de resourcegroep die u in de vorige stap hebt gemaakt, moet u de volgende informatie leveren:

* Key vault-naam: een reeks van 3 tot 24 tekens die mag alleen cijfers (0-9), letters (a-z, A-Z) en afbreekstreepjes (-)
* Naam van de resourcegroep
* Locatie: **US - west**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Op dit moment is uw Azure-account de enige die is gemachtigd voor het uitvoeren van bewerkingen op deze nieuwe sleutelkluis.

## <a name="add-a-secret-to-the-key-vault"></a>Een geheim toevoegen aan de sleutelkluis

We gaan een geheim toevoegen om te laten zien hoe dit werkt. De geheime sleutel is mogelijk een SQL-verbindingsreeks of andere informatie die u nodig hebt om veilig en beschikbaar zijn voor uw toepassing te houden.

Een geheim maken in de key vault met de naam **AppSecret**, voer de volgende opdracht:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Met dit geheim wordt de waarde **MijnGeheim** opgeslagen.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
U kunt een virtuele machine maken met behulp van een van de volgende methoden:

* [De Azure CLI](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli)
* [PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)
* [Azure Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-an-identity-to-the-vm"></a>Een identiteit toewijzen aan de virtuele machine
In deze stap maakt maken u een systeem toegewezen identiteit voor de virtuele machine met de volgende opdracht in de Azure CLI:

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

## <a name="assign-permissions-to-the-vm-identity"></a>Machtigingen toewijzen aan de identiteit van VM
U kunt nu de eerder gemaakte Identiteitsmachtigingen toewijzen aan uw key vault met de volgende opdracht:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Aanmelden bij de virtuele machine

Als u wilt aanmelden bij de virtuele machine, volg de instructies in [verbinding maken met en meld u aan bij een virtuele Azure-machine waarop Windows wordt uitgevoerd](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="install-net-core"></a>.NET Core installeren

Als u wilt installeren .NET Core, gaat u naar de [.NET downloads](https://www.microsoft.com/net/download) pagina.

## <a name="create-and-run-a-sample-net-app"></a>Maken en uitvoeren van een .NET-voorbeeld-app

Open een opdrachtprompt.

U kunt "Hallo wereld" in de console afdrukken door het uitvoeren van de volgende opdrachten:

```
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-the-console-app"></a>De console-app bewerken

Open de *Program.cs* bestand en voeg deze pakketten toe:

```
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

```
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
