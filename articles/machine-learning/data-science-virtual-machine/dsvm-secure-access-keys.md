---
title: Store veilig toegang tot referenties op de Data Science Virtual Machine - Azure | Microsoft Docs
description: Store-toegang op de Data Science Virtual Machine veilig referenties.
keywords: deep learning, AI, hulpprogramma's voor data science, virtuele machine voor datatechnologie, georuimtelijke analyses, team data science process
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
ms.openlocfilehash: 1bf3150fc79f86e196be120fef78b76be8e47f63
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344503"
---
# <a name="store-access-credentials-on-the-data-science-virtual-machine-securely"></a>Store-toegang op de Data Science Virtual Machine veilig referenties

Een algemene vraag in het bouwen van cloudtoepassingen is over het beheren van de referenties die worden in de code moeten voor verificatie bij cloudservices. Het is belangrijk dat deze referenties veilig worden verwerkt. In het ideale geval ze nooit worden weergegeven op werkstations van ontwikkelaars of ophalen ingecheckt in broncodebeheer. 

[Identiteiten voor een Azure-resources beheerd](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) maakt het oplossen van dit probleem eenvoudiger door middel van een Azure-services een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder dat u hoeft geen referenties in uw code. 

Een manier voor het beveiligen van referenties is het gebruik van MSI in combinatie met [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/), een beheerde Azure-service voor het veilig opslaan van geheimen en cryptografische sleutels. U kunt toegang tot een sleutelkluis met behulp van de beheerde identiteit en ophalen van de geautoriseerde geheimen en cryptografische sleutels uit key vault. 

De beheerde identiteit voor Azure-resources en documentatie voor Key Vault is een uitgebreide informatiebron voor gedetailleerde informatie over deze services. De rest van dit artikel helpt bij het basisgebruik van MSI en Key Vault voor de Data Science Virtual Machine (DSVM) voor toegang tot Azure-resources. 

## <a name="create-a-managed-identity-on-the-dsvm"></a>Een beheerde identiteit op de DSVM maken 


```
# Prerequisite: You have already created a Data Science VM in the usual way.

# Create an identity principal for the VM.
az vm assign-identity -g <Resource Group Name> -n <Name of the VM>
# Get the principal ID of the DSVM.
az resource list -n <Name of the VM> --query [*].identity.principalId --out tsv
```


## <a name="assign-key-vault-access-permission-to-a-vm-principal"></a>Key Vault-machtigingen voor toegang toewijzen aan een VM-principal
```
# Prerequisite: You have already created an empty Key Vault resource on Azure by using the Azure portal or Azure CLI. 

# Assign only get and set permission but not the capability to list the keys.
az keyvault set-policy --object-id <Principal ID of the DSVM from previous step> --name <Key Vault Name> -g <Resource Group of Key Vault>  --secret-permissions get set
```

## <a name="access-a-secret-in-the-key-vault-from-the-dsvm"></a>Toegang tot een geheim in de key vault vanuit de DSVM

```
# Get the access token for the VM.
x=`curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true`
token=`echo $x | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`

# Access the key vault by using the access token. 
curl https://<Vault Name>.vault.azure.net/secrets/SQLPasswd?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

## <a name="access-storage-keys-from-the-dsvm"></a>Toegangssleutels voor opslag van de DSVM

```
# Prerequisite: You have granted your VM's MSI access to use storage account access keys based on instructions from the article at https://docs.microsoft.com/azure/active-directory/managed-service-identity/tutorial-linux-vm-access-storage. This article describes the process in more detail.

y=`curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true`
ytoken=`echo $y | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`
curl https://management.azure.com/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup of Storage account>/providers/Microsoft.Storage/storageAccounts/<Storage Account Name>/listKeys?api-version=2016-12-01 --request POST -d "" -H "Authorization: Bearer $ytoken"

# Now you can access the data in the storage account from the retrieved storage account keys.
```
## <a name="access-the-key-vault-from-python"></a>Toegang tot de sleutelkluis met Python

```python
from azure.keyvault import KeyVaultClient
from msrestazure.azure_active_directory import MSIAuthentication

"""MSI Authentication example."""

# Get credentials.
credentials = MSIAuthentication(
    resource='https://vault.azure.net'
)

# Create a Key Vault client.
key_vault_client = KeyVaultClient(
credentials
)

key_vault_uri = "https://<key Vault Name>.vault.azure.net/"

secret = key_vault_client.get_secret(
key_vault_uri,  # Your key vault URL.
"SQLPasswd",       # The name of your secret that already exists in the key vault.
""              # The version of the secret; empty string for latest.
)
print("My secret value is {}".format(secret.value))
```

## <a name="access-the-key-vault-from-azure-cli"></a>Toegang tot de key vault vanuit Azure CLI

```
# With managed identities for Azure resources set up on the DSVM, users on the DSVM can use Azure CLI to perform the authorized functions. Here are commands to access the key vault from Azure CLI without having to log in to an Azure account. 
# Prerequisites: MSI is already set up on the DSVM as indicated earlier. Specific permission, like accessing storage account keys, reading specific secrets, and writing new secrets, is provided to the MSI. 

# Authenticate to Azure CLI without requiring an Azure account. 
az login --msi

# Retrieve a secret from the key vault. 
az keyvault secret show --vault-name <Vault Name> --name SQLPasswd

# Create a new secret in the key vault.
az keyvault secret set --name MySecret --vault-name <Vault Name> --value "Helloworld"

# List access keys for the storage account.
az storage account keys list -g <Storage Account Resource Group> -n <Storage Account Name>
```
