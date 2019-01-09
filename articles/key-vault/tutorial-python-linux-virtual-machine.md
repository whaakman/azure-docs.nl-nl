---
title: 'Zelfstudie: Azure Key Vault gebruiken met Azure Linux Virtual Machine in Python | Microsoft Docs'
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
ms.openlocfilehash: f5d74c2283d25d5774bd46bb9fe94795ff98fe9b
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53720561"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-linux-virtual-machine-in-python"></a>Zelfstudie: Azure Key Vault gebruiken met Azure Linux Virtual Machine in Python

Azure Key Vault helpt u bij het beveiligen van geheimen zoals API-sleutels, databaseverbindingsreeksen die nodig zijn voor toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie voert u de stappen uit die nodig zijn om een Azure-webtoepassing gegevens te laten lezen uit Azure Key Vault met behulp van beheerde identiteiten voor Azure-resources. Hieronder leert u:

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
In deze stap wijzen we een door het systeem toegewezen identiteit toe aan de virtuele machine met de volgende opdracht in Azure CLI

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

## <a name="give-virtual-machine-identity-permission-to-key-vault"></a>Identiteit van virtuele machine machtigen voor Key Vault
Nu kunnen we de hierboven gemaakte identiteit machtigen voor Key Vault met de volgende opdracht

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Aanmelden bij de virtuele machine

U kunt deze [zelfstudie](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon) volgen

## <a name="create-and-run-sample-python-app"></a>Python-voorbeeld-app maken en uitvoeren

Het volgende bestand is slechts een voorbeeldbestand met de naam 'Sample.py'. Hierbij wordt gebruikgemaakt van de bibliotheek [requests](https://pypi.org/project/requests/2.7.0/) (aanvragen) voor HTTP GET-aanroepen.

## <a name="edit-samplepy"></a>Sample.py bewerken
Nadat u Sample.py hebt gemaakt, opent u het bestand en kopieert u de onderstaande code

Het proces hieronder bestaat uit 2 stappen. 
1. Een token uit het lokale MSI-eindpunt op de virtuele machine ophalen zodat er een token uit Azure Active Directory wordt opgehaald
2. Het token doorgeven aan Key Vault en het geheim ophalen 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets a access_token from Azure Active Directory using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to Key Vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Door de onderstaande opdracht uit te voeren wordt de geheime waarde weergegeven 

```
python Sample.py
```

In de bovenstaande code ziet u hoe u bewerkingen met Azure Key Vault uitvoert op een virtuele Windows-machine in Azure. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [REST-API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
