---
title: Een Azure-VM vanaf een VHD voor gebruikers implementeren | Microsoft Docs
description: Wordt uitgelegd hoe u een VHD-installatiekopie van de gebruiker voor het maken van een Azure VM-instantie implementeert.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 9c163ddf7859246fcdaa28edfd4b598a24a32be2
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196790"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>Een Azure-VM van een gebruiker VHD implementeren

In dit artikel wordt uitgelegd hoe u een gegeneraliseerde VHD-installatiekopie voor het maken van een nieuwe virtuele machine van Azure-resource, met de opgegeven Azure Resource Manager-sjabloon en Azure PowerShell-script te implementeren.


## <a name="vhd-deployment-template"></a>VHD-implementatiesjabloon

De Azure Resource Manager-sjabloon voor het kopiëren [VHD implementatie](cpp-deploy-json-template.md) naar een lokaal bestand met de naam `VHDtoImage.json`.  Dit bestand als u waarden opgeven voor de volgende parameters wilt bewerken. 

|  **Parameter**             |   **Beschrijving**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | Bestaande Azure-resource-groepsnaam.  Doorgaans gebruikt u dezelfde Replicatiegroep die zijn gekoppeld aan uw key vault  |
| Sjabloonbestand               | Volledig pad naar het bestand `VHDtoImage.json`                                    |
| userStorageAccountName     | Naam van het storage-account                                                    |
| sNameForPublicIP           | DNS-naam voor het openbare IP-adres. Moet een kleine letter                                  |
| subscriptionId             | Azure-abonnement-id                                                  |
| Locatie                   | Standaard Azure geografische locatie van de resourcegroep                       |
| vmName                     | Naam van de virtuele machine                                                    |
| VaultName                  | Naam van de key vault                                                          |
| vaultResourceGroup         | Resourcegroep van de key vault
| certificateUrl             | URL van het certificaat, met inbegrip van de versie die is opgeslagen in de key vault, bijvoorbeeld:  https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7 |
| vhdUrl                     | URL van de virtuele harde schijf                                                   |
| vmSize                     | Grootte van de virtuele machine-instantie                                           |
| publicIPAddressName        | Naam van het openbare IP-adres                                                  |
| virtualNetworkName         | Naam van het virtuele netwerk                                                    |
| nicName                    | Naam van de netwerkinterfacekaart voor het virtuele netwerk                     |
| adminUserName              | Gebruikersnaam van het administrator-account                                          |
| adminPassword              | Administrator-wachtwoord                                                          |
|  |  |


## <a name="powershell-script"></a>PowerShell-script

Kopiëren en bewerken van het volgende script voor het leveren van waarden voor de `$storageaccount` en `$vhdUrl` variabelen.  Uitvoeren voor het maken van een virtuele machine van Azure-resource van uw bestaande gegeneraliseerde VHD.

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine is geïmplementeerd, bent u klaar om [certificeren van uw VM-installatiekopie](./cpp-certify-vm.md).
