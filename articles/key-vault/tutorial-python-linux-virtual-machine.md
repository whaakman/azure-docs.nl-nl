---
title: 'Zelfstudie: Azure Key Vault gebruiken met een virtuele Azure-machine in Python | Microsoft Docs'
description: In deze zelfstudie configureert u een Python-toepassing om een geheim vanuit een sleutelkluis te lezen
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
ms.openlocfilehash: 1e364003093d5e37a75830386cafe855b0bdcad2
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467398"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-virtual-machine-in-python"></a>Zelfstudie: Azure Key Vault gebruiken met een virtuele Azure-machine in Python

Azure Key Vault helpt u bij het beveiligen van geheimen zoals de API-sleutels en databaseverbindingsreeksen die nodig zijn voor toegang tot uw toepassingen, services en IT-resources.

In deze zelfstudie voert u de stappen uit om een Azure-webtoepassing gegevens te laten lezen uit Azure Key Vault met behulp van beheerde identiteiten voor Azure-resources. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in de sleutelkluis.
> * Een virtuele Azure-machine maken.
> * Een [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) inschakelen voor de virtuele machine.
> * De vereiste machtigingen verlenen aan de consoletoepassing om gegevens te lezen uit de sleutelkluis.
> * Een geheim ophalen uit de sleutelkluis.

Lees voordat u verdergaat de [basisconcepten over Key Vault](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Vereisten
Voor alle platforms hebt u het volgende nodig:

* Git ([downloaden](https://git-scm.com/downloads)).
* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versie 2.0.4 of hoger. Dit is beschikbaar voor Windows, Mac en Linux.

### <a name="managed-service-identity-and-how-it-works"></a>Managed Service Identity en hoe het werkt
In deze zelfstudie wordt Managed Service Identity (MSI) gebruikt.

In Azure Key Vault kunnen referenties veilig worden opgeslagen zodat ze zich niet in uw code bevinden. Om deze op te halen, moet u zich verifiëren bij Key Vault. En om u te verifiëren bij Key Vault, hebt u een referentie nodig. Dat is een klassiek bootstrap-probleem. Via Azure en Azure Active Directory (Azure AD) biedt MSI een 'bootstrap-identiteit' die het eenvoudiger maakt om dingen aan de gang te krijgen.

Wanneer u MSI inschakelt voor een Azure-service, zoals Virtual Machines, App Service of Functions, wordt in Azure een [service-principal](key-vault-whatis.md#basic-concepts) gemaakt voor het exemplaar van de service in Azure Active Directory. Azure stuurt de referenties voor de service-principal naar het exemplaar van de service. 

![MSI](media/MSI.png)

Vervolgens wordt met de ​​code een lokale metagegevensservice aangeroepen die beschikbaar is in de Azure-resource, om een ​​toegangstoken te verkrijgen.
Uw code gebruikt het toegangstoken dat wordt verkregen van het lokale MSI-eindpunt om zich te authenticeren bij een Azure Key Vault-service. 

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

* Naam van de sleutelkluis: De naam moet een tekenreeks zijn met 3-24 tekens en mag alleen 0-9, a-z, A-Z en streepjes (-) bevatten.
* Naam van de resourcegroep.
* Locatie: **US - west**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="add-a-secret-to-the-key-vault"></a>Een geheim toevoegen aan de sleutelkluis

We gaan een geheim toevoegen om te laten zien hoe dit werkt. U wilt bijvoorbeeld een SQL-verbindingsreeks opslaan of andere gegevens die u veilig wilt bewaren, maar wel beschikbaar wilt stellen aan uw toepassing.

Typ de volgende opdrachten om een geheim te maken in de sleutelkluis met de naam *AppGeheim*. Met dit geheim wordt de waarde **MijnGeheim** opgeslagen.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een VM met de opdracht [az vm create](/cli/azure/vm).

In het volgende voorbeeld wordt een virtuele machine met de naam *myVM* gemaakt en voegt u een gebruikersaccount met de naam *azureuser* toe. Door de parameter `--generate-ssh-keys` wordt automatisch een SSH-sleutel gegenereerd en in de standaardsleutellocatie (*~/.ssh*) geplaatst. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Het maken van de virtuele machine en de ondersteunende resources duurt enkele minuten. In het volgende voorbeeld van uitvoer ziet u dat het maken van de virtuele machine is geslaagd:

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

Noteer uw eigen waarde voor `publicIpAddress` in de uitvoer van uw virtuele machine. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stappen.

## <a name="assign-an-identity-to-the-virtual-machine"></a>Een identiteit toewijzen aan de virtuele machine
In deze stap maken we een door het systeem toegewezen identiteit voor de virtuele machine. Voer de volgende opdracht uit in de Azure CLI:

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

De uitvoer van de opdracht is als volgt. Noteer de waarde van **systemAssignedIdentity**. 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-the-virtual-machine-identity-permission-to-the-key-vault"></a>De virtuele machine de identiteitsmachtiging geven voor de sleutelkluis
We kunnen nu de identiteitsmachtiging voor de sleutelkluis geven. Voer de volgende opdracht uit:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-virtual-machine"></a>Aanmelden bij de virtuele machine

Meld u aan bij de virtuele machine door [deze zelfstudie](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon) te volgen.

## <a name="create-and-run-the-sample-python-app"></a>De Python-voorbeeld-app maken en uitvoeren

Het volgende voorbeeldbestand heeft de naam *Sample.py*. Het maakt gebruik van de bibliotheek [requests](https://pypi.org/project/requests/2.7.0/) (aanvragen) om HTTP GET-aanroepen uit te voeren.

## <a name="edit-samplepy"></a>Sample.py bewerken
Nadat u Sample.py hebt gemaakt, opent u het bestand en kopieert u de volgende code. De code is een proces dat uit twee stappen bestaat: 
1. Haal een token uit het lokale MSI-eindpunt op de virtuele machine op. Het eindpunt haalt vervolgens een token op uit Azure Active Directory.
2. Geef het token door aan de sleutelkluis en haal het geheim op. 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from an MSI-enabled Azure resource      
    # Note that the resource here is https://vault.azure.net for the public cloud, and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # Extracting data in JSON format 
    # This request gets an access token from Azure Active Directory by using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access token received from the previous HTTP GET call to the key vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Door de volgende opdracht uit te voeren, wordt de geheime waarde weergegeven: 

```
python Sample.py
```

In de voorgaande code ziet u hoe u bewerkingen met Azure Key Vault uitvoert op een virtuele Windows-machine. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [REST-API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
