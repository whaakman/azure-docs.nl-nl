---
title: Instellen van Azure Sleutelkluis voor virtuele Linux-machines | Microsoft Docs
description: Het instellen van de Sleutelkluis voor gebruik met een virtuele machine van Azure Resource Manager met de CLI 2.0.
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 2cc9b4c978e9a4deb0c8443c4b0f9e301a7cf492
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli-20"></a>Het instellen van de Sleutelkluis voor virtuele machines met de Azure CLI 2.0

In de Azure Resource Manager-stack zijn geheimen/certificaten gemodelleerd als resources die worden geleverd door de Sleutelkluis. Zie voor meer informatie over Azure Sleutelkluis, [wat is Azure Sleutelkluis?](../../key-vault/key-vault-whatis.md) In de volgorde voor Sleutelkluis moet worden gebruikt met Azure Resource Manager virtuele machines, de *EnabledForDeployment* eigenschap voor Sleutelkluis moet zijn ingesteld op true. In dit artikel leest u hoe Sleutelkluis instellen voor gebruik met virtuele Azure-machines (VM's) met behulp van de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Als u wilt deze stappen uitvoert, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login).

## <a name="create-a-key-vault"></a>Een sleutelkluis maken
Een sleutelkluis maken en toewijzen van het implementatiebeleid voor met [az keyvault maken](/cli/azure/keyvault#create). Het volgende voorbeeld wordt een sleutelkluis met de naam `myKeyVault` in de `myResourceGroup` resourcegroep:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Een Sleutelkluis voor gebruik met virtuele machines bijwerken
Stel het beleid voor implementatie op een bestaande sleutel vault met [az keyvault update](/cli/azure/keyvault#update). De volgende updates de sleutelkluis met de naam `myKeyVault` in de `myResourceGroup` resourcegroep:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken voor het instellen van de Sleutelkluis
Wanneer u een sjabloon gebruikt, moet u om in te stellen de `enabledForDeployment` eigenschap `true` voor de sleutel Vault resource als volgt:

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
Zie voor andere opties die u configureren kunt wanneer u een Sleutelkluis maken met behulp van sjablonen, [een sleutelkluis maken](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
