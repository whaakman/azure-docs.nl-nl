---
title: Azure CLI-voorbeeldscript - koppelpunt besturingssysteemschijf | Microsoft Docs
description: Azure CLI-voorbeeldscript - schijf koppelen-besturingssysteem
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c32ea5e6cade34a9c8dac0eab523ebcaa10ef039
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Problemen met een besturingssysteemschijf virtuele machines

Dit script koppelt u de schijf van het besturingssysteem van een mislukte of problematisch virtuele machine als een gegevensschijf aan een tweede virtuele machine. Dit kan nuttig zijn bij het oplossen van problemen of -gegevens herstellen schijf. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ vm weergeven](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Retourneert een lijst met virtuele machines. In dit geval wordt de query-optie gebruikt om te retourneren van de schijf van de virtuele machine-besturingssysteem. Deze waarde wordt vervolgens toegevoegd aan de naam van een variabele 'uri'. |
| [AZ vm verwijderen](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Hiermee verwijdert u een virtuele machine. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Hiermee maakt u een virtuele machine.  |
| [AZ vm schijf koppelen](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Er wordt een schijf aan een virtuele machine toegevoegd. |
| [AZ vm lijst-ip-adressen](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Retourneert de IP-adressen van een virtuele machine. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
