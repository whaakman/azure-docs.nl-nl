---
title: 'Zelfstudie: Azure Key Vault gebruiken met Azure Linux Virtual Machine in .NET | Microsoft Docs'
description: 'Zelfstudie: Een ASP.NET Core-toepassing configureren voor het lezen van een geheim uit Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 928339a245525933ae142a5d73137ce699cf1f7c
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712327"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-linux-virtual-machine-in-net"></a>Zelfstudie: Azure Key Vault gebruiken met Azure Linux Virtual Machine in .NET

Azure Key Vault helpt u bij het beveiligen van geheimen zoals API-sleutels, databaseverbindingsreeksen die nodig zijn voor toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie voert u de stappen uit die nodig zijn om een consoletoepassing gegevens te laten lezen uit Azure Key Vault met behulp van beheerde identiteiten voor Azure-resources. Deze zelfstudie is gebaseerd op [Azure Web Apps](../app-service/app-service-web-overview.md). Hieronder leert u:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in de sleutelkluis.
> * Een geheim ophalen uit de sleutelkluis.
> * Een virtuele Azure-machine maken.
> * Een [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) inschakelen voor de virtuele machine.
> * De vereiste machtigingen verlenen aan de consoletoepassing om gegevens te lezen uit de sleutelkluis.
> * Geheimen lezen uit Key Vault

Lees voordat we verdergaan de [basisconcepten](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Vereisten
* Alle platformen:
  * Git ([downloaden](https://git-scm.com/downloads)).
  * Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versie 2.0.4 of hoger. Deze is beschikbaar voor Windows, Mac en Linux.

In deze zelfstudie wordt Managed Service Identity gebruikt

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Wat is Managed Service Identity en hoe werkt het?
Voordat we verder gaan, moeten we het over MSI hebben. Azure Key Vault kan referenties veilig opslaan zodat ze zich niet in uw code bevinden, maar om ze op te halen moet u zich authenticeren bij Azure Key Vault. En om u te authenticeren bij Key Vault, hebt u een referentie nodig! Een klassiek bootstrap-probleem. Door de magie van Azure en Azure AD biedt MSI een 'bootstrap-identiteit' die het veel eenvoudiger maakt om dingen op gang te brengen.

Het werkt als volgt. Wanneer u MSI inschakelt voor een Azure-service zoals Virtual Machines, App Service of Functions, maakt Azure een [service-principal](key-vault-whatis.md#basic-concepts) voor de instantie van de service in Azure Active Directory, en injecteert de referenties voor de service-principal in de instantie van de service. 

![MSI](media/MSI.png)

Vervolgens roept uw ​​code een lokale metadataservice aan die beschikbaar is op de Azure-resource om een ​​toegangstoken te verkrijgen.
Uw code gebruikt het toegangstoken dat wordt verkregen van het lokale MSI_ENDPOINT zich te authenticeren bij een Azure Key Vault-service. 

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

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een VM met de opdracht [az vm create](/cli/azure/vm#az_vm_create).

In het volgende voorbeeld wordt een virtuele machine met de naam *myVM* gemaakt en voegt u een gebruikersaccount met de naam *azureuser* toe. De parameter `--generate-ssh-keys` wordt gebruikt om automatisch een SSH-sleutel te genereren en deze te plaatsen in de standaardsleutellocatie (*~/.ssh*). Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Het maken van de virtuele machine en de ondersteunende resources duurt enkele minuten. In het volgende voorbeeld van uitvoer ziet u dat het maken van de virtuele machine is geslaagd.

```
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

Noteer uw eigen `publicIpAddress` in de uitvoer van uw virtuele machine. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stappen.

## <a name="assign-identity-to-virtual-machine"></a>Identiteit toewijzen aan een virtuele machine
In deze stap wijzen we een door het systeem toegewezen identiteit toe aan de virtuele machine met de volgende opdracht

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Hieronder wordt de door het systeem toegewezen identiteit (systemAssignedIdentity) weergegeven. De uitvoer van de bovenstaande opdracht ziet er als volgt uit 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-vm-identity-permission-to-key-vault"></a>VM-identiteit machtigen voor Key Vault
Nu kunnen we de hierboven gemaakte identiteit machtigen voor Key Vault met de volgende opdracht

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Aanmelden bij de virtuele machine

Meld u nu via een terminal aan bij de virtuele machine

```
ssh azureuser@<PublicIpAddress>
```

## <a name="install-dot-net-core-on-linux"></a>.Net Core installeren onder Linux
### <a name="register-the-microsoft-product-key-as-trusted-run-the-following-two-commands"></a>Registreer de Microsoft-productsleutel als vertrouwd. Voer de volgende twee opdrachten uit

```
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
```

### <a name="set-up-desired-version-host-package-feed-based-on-operating-system"></a>Stel de gewenste versie van de hostpakketfeed in op basis van het besturingssysteem
```
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

### <a name="install-net-core"></a>.NET Core installeren

En controleer de .NET-versie

```
sudo apt-get install dotnet-sdk-2.1.4
dotnet --version
```

## <a name="create-and-run-sample-dot-net-app"></a>.Net-voorbeeld-app maken en uitvoeren

Als u de onderstaande opdrachten uitvoert, wordt 'Hallo wereld' weergegeven in de console

```
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-console-app"></a>Console-app bewerken
Open het bestand Program.cs en voeg deze pakketten toe
```
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```
Wijzig vervolgens het klassebestand met de volgende code. Dit proces omvat twee stappen. 
1. Een token uit het lokale MSI-eindpunt op de virtuele machine ophalen zodat er een token uit Azure Active Directory wordt opgehaald
2. Het token doorgeven aan Key Vault en het geheim ophalen 

```
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

In de bovenstaande code ziet u hoe u bewerkingen met Azure Key Vault uitvoert op een virtuele Linux-machine in Azure. 


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [REST-API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
