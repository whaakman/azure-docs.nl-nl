---
title: Zelfstudie - een virtuele Linux-machine en een ASP.NET-consoletoepassing voor het opslaan van geheimen in Azure Key Vault | Microsoft Docs
description: In deze zelfstudie leert u hoe het configureren van een ASP.NET Core-App met een geheim vanuit Azure Key vault kan lezen.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 8c142b4c1663a7ccd462836e9aac4ee80accaae4
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67874824"
---
# <a name="tutorial-use-a-linux-vm-and-a-net-app-to-store-secrets-in-azure-key-vault"></a>Zelfstudie: Een Linux-VM en een .NET-app gebruiken voor het opslaan van geheimen in Azure Key Vault

Azure Key Vault helpt u bij het beveiligen van geheimen, zoals API-sleutels en tekenreeksen voor databaseverbindingen die nodig zijn voor toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie maakt instellen u een .NET-consoletoepassing om informatie te lezen uit Azure Key Vault met behulp van beheerde identiteiten voor Azure-resources. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een sleutelkluis maken
> * Een geheim Store in Key Vault
> * Een Azure Linux-machine maken
> * Een [Beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) inschakelen voor de virtuele machine
> * De vereiste machtigingen voor de consoletoepassing gegevens lezen uit Key Vault
> * Ophalen van een geheim uit Key Vault

Voordat we verdergaan, meer informatie over [key vault-basisconcepten](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Vereisten

* [Git](https://git-scm.com/downloads).
* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* [Azure CLI 2.0 of hoger](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) of Azure Cloud Shell.

[!INCLUDE [Azure Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="understand-managed-service-identity"></a>Informatie over Managed Service Identity

Azure Key Vault kan referenties veilig opslaan zodat ze zich niet in uw code bevinden, maar om ze op te halen moet u zich authenticeren bij Azure Key Vault. En om u te verifiëren bij Key Vault, hebt u een referentie nodig. Dat is een klassiek bootstrap-probleem. Met Azure en Azure Active Directory (Azure AD) krijgt u Managed Service Identity (MSI) een bootstrap-identiteit waardoor het veel eenvoudiger te starten.

Wanneer u de MSI-bestand voor een Azure-service, zoals virtuele Machines, App Service of functies inschakelt, maakt Azure een service principal voor het exemplaar van de service in Azure Active Directory. Azure stuurt de referenties voor de service-principal naar het exemplaar van de service.

![MSI](media/MSI.png)

Vervolgens wordt met de ​​code een lokale metagegevensservice aangeroepen die beschikbaar is in de Azure-resource, om een ​​toegangstoken te verkrijgen.
Uw code gebruikt het toegangstoken dat wordt verkregen van het lokale MSI_ENDPOINT zich te authenticeren bij een Azure Key Vault-service.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Als u zich met behulp van Azure CLI wilt aanmelden bij Azure, voert u het volgende in:

```azurecli-interactive
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met behulp van de `az group create` opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Maak een resourcegroep in de locatie VS-West. Kies een naam voor de resourcegroep en vervang `YourResourceGroupName` in het volgende voorbeeld:

```azurecli-interactive
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

U gebruikt deze resourcegroep in de hele zelfstudie.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Maak vervolgens een key vault in uw resourcegroep. Geef de volgende informatie op:

* Key vault-naam: een reeks van 3 tot 24 tekens die mag alleen cijfers, letters en afbreekstreepjes bevatten (0-9, a-z, A-Z, en \- ).
* Naam van de resourcegroep
* Locatie: **US - west**

```azurecli-interactive
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Op dit moment is alleen uw Azure-account gemachtigd voor het uitvoeren van bewerkingen op deze nieuwe kluis.

## <a name="add-a-secret-to-the-key-vault"></a>Een geheim toevoegen aan de sleutelkluis

Nu toevoegen u een geheim. In een Praktijkscenario u mogelijk worden opslaan van een SQL-verbindingsreeks of andere informatie die u moet veilig houden, maar toegankelijk maken voor uw toepassing.

Typ de volgende opdrachten voor het maken van een geheim in de key vault voor deze zelfstudie. Het geheim heet **AppSecret** en de waarde is **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-linux-virtual-machine"></a>Een virtuele Linux-machine maken

Maak een virtuele machine met de `az vm create` opdracht.

In het volgende voorbeeld wordt een virtuele machine met de naam **myVM** gemaakt en voegt u een gebruikersaccount met de naam **azureuser** toe. De `--generate-ssh-keys` parameter op ons gebruikt voor het automatisch genereren van een SSH-sleutel en plaatst deze in de standaardsleutellocatie ( **~/.ssh**). Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Het maken van de virtuele machine en de ondersteunende resources duurt enkele minuten. Het volgende voorbeeld-uitvoer ziet dat de bewerking voor het maken van de virtuele machine is voltooid.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

Maak een notitie van uw `publicIpAddress` in de uitvoer van uw virtuele machine. Dit adres wordt in latere stappen gebruikt voor toegang tot de VM.

## <a name="assign-an-identity-to-the-vm"></a>Een identiteit toewijzen aan de virtuele machine

In deze stap wijzen we een door het systeem toegewezen identiteit toe aan de virtuele machine met de volgende opdracht:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

De uitvoer van de opdracht moet:

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

Noteer de waarde van `systemAssignedIdentity`. U gebruikt deze in de volgende stap.

## <a name="give-the-vm-identity-permission-to-key-vault"></a>VM-identiteit machtigen voor Key Vault

U krijgt nu Key Vault-machtigingen voor de identiteit die u hebt gemaakt. Voer de volgende opdracht uit:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-vm"></a>Aanmelden bij de nieuwe virtuele machine

Meld u nu via een terminal aan bij de virtuele machine.

```terminal
ssh azureuser@<PublicIpAddress>
```

## <a name="install-net-core-on-linux"></a>.NET Core installeren in Linux

Op uw virtuele Linux-machine:

De sleutel voor de Microsoft-product als vertrouwd registreren door het uitvoeren van de volgende opdrachten:

   ```console
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
   ```

Instellen van de gewenste versie host pakket feed op basis van besturingssysteem:

```console
   # Ubuntu 17.10
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-artful-prod artful main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 17.04
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-zesty-prod zesty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 16.04 / Linux Mint 18
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 14.04 / Linux Mint 17
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
```

.NET installeren en de versie controleren:

   ```console
   sudo apt-get install dotnet-sdk-2.1.4
   dotnet --version
   ```

## <a name="create-and-run-a-sample-net-app"></a>Maken en uitvoeren van een .NET-voorbeeld-app

Voer de volgende opdrachten. U ziet 'Hello World' weergegeven in de console.

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-the-console-app-to-fetch-your-secret"></a>De console-app om op te halen van het geheim bewerken

Open het bestand Program.cs en voeg deze pakketten toe:

   ```csharp
   using System;
   using System.IO;
   using System.Net;
   using System.Text;
   using Newtonsoft.Json;
   using Newtonsoft.Json.Linq;
   ```

Het is een proces in twee stappen voor het wijzigen van de klassebestand om in te schakelen van de app voor toegang tot het geheim in de key vault.

1. Een token uit het lokale MSI-eindpunt ophalen op de virtuele machine die op zijn beurt ophaalt van een token uit Azure Active Directory.
1. Het token doorgeven aan Key Vault en op te halen van het geheim.

   Bewerk het klassebestand bevat de volgende code:

   ```csharp
    class Program
       {
           static void Main(string[] args)
           {
               // Step 1: Get a token from local (URI) Managed Service Identity endpoint which in turn fetches it from Azure Active Directory
               var token = GetToken();

               // Step 2: Fetch the secret value from Key Vault
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
               WebRequest kvRequest = WebRequest.Create("https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01");
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

U hebt nu geleerd hoe u met Azure Key Vault bewerkingen wilt uitvoeren in een .NET-toepassing die wordt uitgevoerd op een Azure Linux-machine.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep, de virtuele machine en alle gerelateerde resources wanneer u ze niet meer nodig hebt. Om dit te doen, selecteert u de resourcegroep voor de VM en selecteert u **Verwijderen**.

Verwijder de sleutelkluis met behulp van de opdracht `az keyvault delete`:

```azurecli-interactive
az keyvault delete --name
                   [--resource group]
                   [--subscription]
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [REST-API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
