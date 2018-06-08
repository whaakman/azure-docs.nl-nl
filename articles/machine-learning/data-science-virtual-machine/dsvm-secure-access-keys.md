---
title: Store toegang tot de referenties op de gegevens wetenschappelijke virtuele Machine veilig - Azure | Microsoft Docs
description: Opslaan van toegang op de gegevens wetenschappelijke virtuele Machine veilig referenties.
keywords: deep leren, AI en hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, georuimtelijke analytics, team gegevens wetenschappelijke processen
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 9ec734cf456050250396b00aa09b61bace7e9aa0
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830262"
---
# <a name="store-access-credentials-on-the-data-science-virtual-machine-securely"></a>Opslaan van toegang op de gegevens wetenschappelijke virtuele Machine veilig referenties

Een veelvoorkomende uitdaging bij het bouwen van cloud-apps is het beheren van de referenties die in uw code moeten worden opgenomen voor verificatie bij cloudservices. Het is belangrijk dat deze referenties veilig worden verwerkt. In het ideale geval worden ze nooit weergegeven op werkstations van ontwikkelaars of ingecheckt in broncodebeheer. 

[Service-identiteit (MSI) beheerd](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) zorgt ervoor dat het oplossen van dit probleem eenvoudiger door middel van een Azure-services een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder dat u geen referenties hoeft in uw code te verifiëren. Een algemene patroon voor het beveiligen van referenties is MSI gebruiken in combinatie met de [Azure Keyvault](https://docs.microsoft.com/azure/key-vault/), een Azure-service voor het opslaan van geheimen en cryptografische sleutels veilig worden beheerd. U kunt toegang tot de sleutelkluis met behulp van de beheerde service-identiteit en ophalen van de gemachtigde geheimen en de cryptografische sleutels uit de Sleutelkluis. 

De documentatie MSI en Sleutelkluis is een uitgebreide informatiebron voor gedetailleerde informatie over deze services. De rest van dit artikel is een eenvoudige primer het basisgebruik van MSI en Sleutelkluis weergegeven op de gegevens wetenschappelijke virtuele Machine (DSVM). 

## <a name="walkthrough-of-using-msi-to-securely-access-azure-resources"></a>Overzicht van het gebruik van MSI veilig toegang krijgen tot Azure-resources

## <a name="1-create-msi-on-the-dsvm"></a>1. Maken van MSI op de DSVM 


```
# Pre-requisites: You have already created a Data Science VM in the usual way

# Create identity principal for the VM
az vm assign-identity -g <Resource Group Name> -n <Name of the VM>
# Get the principal id of the DSVM
az resource list -n <Name of the VM> --query [*].identity.principalId --out tsv
```


## <a name="2-assign-keyvault-access-permission-to-vm-principal"></a>2. Keyvault-machtiging toewijzen aan VM-principal
```
# Pre-requisites: You have already create an empty Key Vault resource on Azure using Portal or Azure CLI 

# Assign only get and set permission but not the capability to list the keys
az keyvault set-policy --object-id <PrincipalID of the DSVM from previous step> --name <Key Vault Name> -g <Resource Group of Key Vault>  --secret-permissions get set
```

## <a name="3-access-a-secret-in-the-keyvault-from-the-dsvm"></a>3. Toegang tot een geheim in de Keyvault van de DSVM

```
# Get the access token for VM
x=`curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true`
token=`echo $x | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`

# Access Keyvault using access token. 
curl https://<Vault Name>.vault.azure.net/secrets/SQLPasswd?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

## <a name="4-access-storage-keys-from-the-dsvm"></a>4. Toegangssleutels voor opslag van de DSVM

```
# Pre-requisites: You have granted your VM's MSI access to use storage account access keys based on instruction from this article:https://docs.microsoft.com/azure/active-directory/managed-service-identity/tutorial-linux-vm-access-storage that describes this process in more detail.

y=`curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true`
ytoken=`echo $y | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`
curl https://management.azure.com/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup of Storage account>/providers/Microsoft.Storage/storageAccounts/<Storage Account Name>/listKeys?api-version=2016-12-01 --request POST -d "" -H "Authorization: Bearer $ytoken"

#Now you can access the data in the storage account from the retrieved Storage account keys
```
## <a name="5-access-keyvault-from-python"></a>5. De Keyvault toegang met Python

```python
from azure.keyvault import KeyVaultClient
from msrestazure.azure_active_directory import MSIAuthentication

"""MSI Authentication example."""

# Get credentials
credentials = MSIAuthentication(
    resource='https://vault.azure.net'
)

# Create a KeyVault client
key_vault_client = KeyVaultClient(
credentials
)

key_vault_uri = "https://<key Vault Name>.vault.azure.net/"

secret = key_vault_client.get_secret(
key_vault_uri,  # Your KeyVault URL
"SQLPasswd",       # Name of your secret that already exists in the Key Vault
""              # The version of the secret. Empty string for latest
)
print("My secret value is {}".format(secret.value))
```

## <a name="6-azure-cli-access-from-vm"></a>6. Toegang tot Azure CLI van virtuele machine

```
# With a Managed Service Identity setup on the DSVM, users on the DSVM can execute Azure CLI to perform the authorized functions. Here are commands to access Key Vault from a Azure CLI without having to login to an Azure account. 
# Pre-requisites: MSI is already setup on the DSVM as indicated earlier and specific permission like accessing storage account keys, reading specific secrets and writing new secrets is provided to the MSI . 

# Authenticate to Azure CLI without requiring an Azure Account. 
az login --msi

# Retrieve a secret from Key Vault. 
az keyvault secret show --vault-name <Vault Name> --name SQLPasswd

# Create a new Secret in Key Vault
az keyvault secret set --name MySecret --vault-name <Vault Name> --value "Helloworld"

# List Storage Account Access Keys
az storage account keys list -g <Storage Account Resource Group> -n <Storage Account Name>
```
