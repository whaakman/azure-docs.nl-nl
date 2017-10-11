---
title: Instellen van de Sleutelkluis voor Windows-machines in Azure Resource Manager | Microsoft Docs
description: Het instellen van de Sleutelkluis voor gebruik met een virtuele machine van Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: a5083a5216efbfd76fd912ec48c2f0ec3b30c4a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Sleutelkluis instellen voor virtuele machines in Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

In Azure Resource Manager-stack, worden geheimen/certificaten gemodelleerd als resources die worden geleverd door de bronprovider van de Sleutelkluis. Zie voor meer informatie over Sleutelkluis, [wat is Azure Sleutelkluis?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. In de volgorde voor Sleutelkluis moet worden gebruikt met virtuele machines van Azure Resource Manager, de **EnabledForDeployment** eigenschap voor Sleutelkluis moet zijn ingesteld op true. U kunt dit doen in verschillende clients.
> 2. De Sleutelkluis moet worden gemaakt in hetzelfde abonnement en dezelfde locatie als de virtuele Machine.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>PowerShell gebruiken voor het instellen van de Sleutelkluis
Zie voor informatie over het maken van een sleutelkluis met behulp van PowerShell [aan de slag met Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).

Voor nieuwe sleutelkluizen, kunt u deze PowerShell-cmdlet:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Voor bestaande sleutelkluizen, kunt u deze PowerShell-cmdlet:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Ons CLI voor het instellen van de Sleutelkluis
Zie voor informatie over het maken van een sleutelkluis via de opdrachtregelinterface (CLI) [Key Vault beheren met CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

U hebt de sleutelkluis maken voordat u het beleid voor de implementatie toewijzen voor CLI. U gebruikt hiervoor de volgende opdracht:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken voor het instellen van de Sleutelkluis
Terwijl u een sjabloon gebruikt, moet u de `enabledForDeployment` eigenschap `true` voor de Sleutelkluis-resource.

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

Zie voor andere opties die u configureren kunt wanneer u een sleutelkluis maken met behulp van sjablonen, [een sleutelkluis maken](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
