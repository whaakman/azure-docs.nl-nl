---
title: Azure CLI Script voorbeeld - een virtuele machine maken met een VHD | Microsoft Docs
description: 'Azure CLI-Script voorbeeld: een virtuele machine maken met een virtuele harde schijf.'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 6234473d9f7f0eb18ea85e52273eb82a9ce04da5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Een virtuele machine met een virtuele harde schijf maken

In dit voorbeeld maakt een VHD met een virtuele machine.
Het maakt een resourcegroep, een opslagaccount en een container en een virtuele machine wordt gemaakt door het uploaden van de VHD naar de container.
Vervangt de ssh openbare sleutel met de openbare sleutel zodat u toegang hebt tot de virtuele machine.

U moet een opstartbare VHD.
U kunt downloaden van de VHD die wordt gebruikt vanuit https://azclisamples.blob.core.windows.net/vhds/sample.vhd of uw eigen VHD gebruiken. Het script wordt gezocht naar `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine, beschikbaarheidsset, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [lijst met opslagaccounts AZ](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_list) | Een lijst met storage-accounts |
| [AZ opslag Controleer accountnaam](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_check_name) | Controleert of de naam van een account geldig is en nog niet bestaat |
| [lijst met opslagaccounts die sleutels AZ](https://docs.microsoft.com/cli/azure/storage/account/keys#az_storage_account_keys_list) | Een lijst met sleutels voor de storage-accounts |
| [AZ storage-blob bestaat](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_exists) | Controleert of de blob bestaat |
| [AZ storage-container maken](https://docs.microsoft.com/cli/azure/storage/container#az_storage_container_create) | Maakt een container in een opslagaccount. |
| [uploaden van blob storage AZ](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_upload) | Maakt een blob in de container door de VHD te uploaden. |
| [AZ vm lijst](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Gebruikt met `--query` controleren of de naam van de VM in gebruik is. | 
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Maakt de virtuele machines. |
| [AZ vm toegang set-linux-gebruikers](https://docs.microsoft.com/cli/azure/vm/access#az_vm_access_set_linux_user) | Hiermee stelt u de SSH-sleutel in de huidige om gebruikerstoegang te verlenen aan de virtuele machine. |
| [AZ vm lijst-ip-adressen](https://docs.microsoft.com/cli/azure/vm#az_vm_list-ip-addresses) | Hiermee haalt u het IP-adres van de virtuele machine die is gemaakt. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
