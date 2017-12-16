---
title: Azure CLI Sample Script - versleutelen van een Windows VM | Microsoft Docs
description: Azure CLI Sample Script - versleutelen van een Windows VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 7e1e9a69d226b03e6722c2d19b7a091cfe4e71c7
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Versleutelen van een virtuele Windows-machine in Azure

Dit script maakt een beveiligde Azure Sleutelkluis, versleutelingssleutels, Azure Active Directory service-principal en virtuele Windows-machine (VM). De virtuele machine wordt vervolgens versleuteld met behulp van de versleutelingssleutel van de Sleutelkluis en service principal referenties.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, Azure Sleutelkluis, service-principal, virtuele machine en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ keyvault maken](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Hiermee maakt u een Azure Key Vault voor het opslaan van beveiligde gegevens, zoals versleutelingssleutels. |
| [AZ keyvault-sleutel maken](https://docs.microsoft.com/cli/azure/keyvault/key#az_keyvault_key_create) | Maakt een versleutelingssleutel in de Sleutelkluis. |
| [AZ ad sp maken-voor-rbac](https://docs.microsoft.com/cli/azure/ad/sp#az_ad_sp_create_for_rbac) | Maakt een Azure Active Directory service-principal veilig verifiëren en toegang tot de versleutelingssleutels. |
| [AZ keyvault-beleid instellen](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Stelt de machtigingen op de Sleutelkluis de service principal toegang verlenen tot versleutelingssleutels. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en NSG is verbonden. Met deze opdracht geeft ook de afbeelding van de virtuele machine moet worden gebruikt en de beheerdersreferenties.  |
| [AZ vm-codering inschakelen](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_enable) | Hiermee schakelt u versleuteling op een virtuele machine met behulp van de service principal referenties en de versleutelingssleutel. |
| [AZ vm versleuteling weergeven](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_show) | Toont de status van het VM-versleutelingsproces. |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [virtuele machine van Windows Azure-documentatie](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
