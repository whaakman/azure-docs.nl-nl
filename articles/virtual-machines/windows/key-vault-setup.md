---
title: Instellen van Key Vault voor Windows-VM's in Azure Resource Manager | Microsoft Docs
description: Over het instellen van Key Vault voor gebruik met een virtuele machine van Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: 599b16f633d9a0de5165bdf5cb3d7b82abca655b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597707"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Key Vault instellen voor virtuele machines in Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

In Azure Resource Manager-stack, worden geheimen/certificates gemodelleerd als resources die worden geleverd door de resourceprovider van Key Vault. Zie voor meer informatie over Key Vault, [wat is Azure Key Vault?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Key Vault moet worden gebruikt met virtuele machines van Azure Resource Manager, zodat de **EnabledForDeployment** eigenschap voor Key Vault moet zijn ingesteld op true. U kunt dit doen in verschillende clients.
> 2. De Key Vault moet worden gemaakt in hetzelfde abonnement en dezelfde locatie als de virtuele Machine.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>PowerShell gebruiken voor het instellen van Key Vault
Zie voor informatie over het maken van een key vault met behulp van PowerShell [aan de slag met Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).

Voor nieuwe sleutelkluizen, kunt u deze PowerShell-cmdlet:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Voor bestaande key vaults, kunt u deze PowerShell-cmdlet:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>CLI gebruiken voor het instellen van Key Vault
Zie voor informatie over het maken van een key vault met behulp van de opdrachtregelinterface (CLI) [Key Vault beheren met behulp van CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Voor CLI moet u de key vault maken voordat u de implementatiebeleid toewijst. U gebruikt hiervoor de volgende opdracht:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken voor het instellen van Key Vault
Hoewel u een sjabloon gebruiken, moet u instellen de `enabledForDeployment` eigenschap `true` voor de Key Vault-resource.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Zie voor andere opties die u configureren kunt wanneer u een sleutelkluis maken met behulp van sjablonen, [maken van een key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
