---
title: Azure Key Vault instellen voor virtuele Linux-machines | Microsoft Docs
description: Over het instellen van Key Vault voor gebruik met een Azure Resource Manager-machine met de Azure CLI.
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 04f47c0a4f6647ff0d45cc5dac40a677cc45563e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970257"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>Key Vault instellen voor virtuele machines met de Azure CLI

In de Azure Resource Manager-stack, worden geheimen/certificates gemodelleerd als resources die worden geboden door Key Vault. Zie voor meer informatie over Azure Key Vault, [wat is Azure Key Vault?](../../key-vault/key-vault-whatis.md) Key Vault moet worden gebruikt met Azure Resource Manager VM's, zodat de *EnabledForDeployment* eigenschap voor Key Vault moet zijn ingesteld op true. Dit artikel ziet u hoe u Key Vault instellen voor gebruik met Azure virtual machines (VM's) met de Azure CLI. 

Als u wilt deze stappen uitvoert, moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

## <a name="create-a-key-vault"></a>Een sleutelkluis maken
Een sleutelkluis maken en toewijzen van implementatiebeleid voor de met [az keyvault maken](/cli/azure/keyvault#az_keyvault_create). Het volgende voorbeeld wordt een key vault met de naam `myKeyVault` in de `myResourceGroup` resourcegroep:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Een Key Vault voor gebruik met virtuele machines bijwerken
Set-kluis met de beleidsregels voor implementatie op een bestaande sleutel [az keyvault update](/cli/azure/keyvault#az_keyvault_update). De volgende updates de key vault met de naam `myKeyVault` in de `myResourceGroup` resourcegroep:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken voor het instellen van Key Vault
Wanneer u een sjabloon gebruikt, moet u instellen de `enabledForDeployment` eigenschap `true` voor de Key Vault resource als volgt:

```json
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
```

## <a name="next-steps"></a>Volgende stappen
Zie voor andere opties die u configureren kunt wanneer u een Sleutelkluis maken met behulp van sjablonen, [maken van een key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
