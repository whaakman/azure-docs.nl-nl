---
title: Een VM implementeren vanaf de Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u een virtuele machine implementeren vanuit een vooraf geconfigureerde virtuele machine van Azure Marketplace.
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
ms.openlocfilehash: 3ec4e0b047e94dc7481c51390c6e4370fdea5efa
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258795"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Een virtuele machine implementeren vanuit Azure Marketplace

In dit artikel wordt uitgelegd hoe u een vooraf geconfigureerde virtuele machine (VM) van een Azure Marketplace, met behulp van de opgegeven Azure PowerShell-script te implementeren.  Met dit script wordt ook aangegeven dat de WinRM HTTP en HTTPS-eindpunten op de virtuele machine.  Het script is vereist dat u nog een certificaat geüpload naar Azure Key Vault, hebt zoals beschreven in [certificaten voor Azure Key Vault maken](./cpp-create-key-vault-cert.md). 


## <a name="vm-deployment-template"></a>VM-sjabloon voor implementatie

De quickstart-sjabloon voor virtuele machine van Azure-implementatie, is beschikbaar als de bestanden online [azuredeploy.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json).  Deze bevat de volgende parameters:

|  **Parameter**        |   **Beschrijving**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName | Naam van het storage-account                       |
| dnsNameForPublicIP    | DNS-naam voor het openbare IP-adres. Moet een kleine letter.    |
| adminUserName         | De gebruikersnaam van beheerder                          |
| adminPassword         | Het wachtwoord van beheerder                          |
| imagePublisher        | Installatiekopie-uitgever                                   |
| imageOffer            | Installatiekopie-aanbieding                                       |
| imageSKU              | Installatiekopie-SKU                                         |
| vmSize                | Grootte van de virtuele machine                                    |
| vmName                | Naam van de virtuele machine                                    |
| vaultName             | Naam van de key vault                             |
| vaultResourceGroup    | Resourcegroep van de key vault                   |
| certificateUrl        | URL voor het certificaat, met inbegrip van versie in Key Vault, bijvoorbeeld  `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
|  |  |


## <a name="deployment-script"></a>Script voor implementatie

De volgende Azure PowerShell-script bewerken en uitvoeren als de opgegeven Azure Marketplace VM wilt implementeren.

```powershell

New-AzureRmResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Volgende stappen

Wanneer u een vooraf geconfigureerde virtuele machine hebt geïmplementeerd, kunt u configureren en toegang tot de oplossingen en services die deze bevat of gebruikt voor verdere ontwikkeling. 
