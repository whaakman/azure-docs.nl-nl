---
title: Sleutelkluis instellen voor virtuele Linux-machines met de Azure CLI 1.0 | Microsoft Docs
description: Het instellen van de Sleutelkluis voor gebruik met een virtuele machine van Azure Resource Manager met de Azure CLI 1.0.
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: a9225429e878415334b0c8a66777902395606d63
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-the-azure-cli-10"></a>Sleutelkluis instellen voor virtuele machines in Azure Resource Manager met de Azure CLI 1.0
In de Azure Resource Manager-stack zijn geheimen/certificaten gemodelleerd als resources die worden geleverd door de bronprovider van de Sleutelkluis. Zie voor meer informatie over Azure Sleutelkluis, [wat is Azure Sleutelkluis?](../../key-vault/key-vault-whatis.md) In de volgorde voor Sleutelkluis moet worden gebruikt met virtuele machines van Azure Resource Manager, de *EnabledForDeployment* eigenschap voor Sleutelkluis moet zijn ingesteld op true. U kunt dit doen in verschillende clients. In dit artikel leest u hoe Sleutelkluis instellen voor gebruik met Azure Virtual Machines.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak met een van de volgende versies van de CLI voltooien

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="use-cli-10-to-set-up-key-vault"></a>CLI-1.0 gebruiken voor het instellen van de Sleutelkluis
Zie voor informatie over het maken van een sleutelkluis via de opdrachtregelinterface (CLI) [Key Vault beheren met CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Voor CLI 1.0 moet u de sleutelkluis maken voordat u het beleid voor de implementatie toewijzen. Vervolgens kunt u het beleid met behulp van de volgende opdracht:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken voor het instellen van de Sleutelkluis
Wanneer u een sjabloon gebruikt, moet u om in te stellen de `enabledForDeployment` eigenschap `true` voor de Sleutelkluis-resource.

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
