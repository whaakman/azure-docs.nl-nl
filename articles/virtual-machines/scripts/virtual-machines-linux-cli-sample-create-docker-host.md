---
title: 'Azure CLI-Script voorbeeld: Maak een Docker-host | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: Maak een Docker-host'
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
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c1e0be0e305ba4d53d8e26c55a92b63e1291171d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-docker"></a>Een virtuele machine maken met Docker

Dit script maakt een virtuele machine met Docker ingeschakeld en begint met een Docker-container NGINX uitgevoerd. Nadat het script is uitgevoerd, kunt u het NGINX-webserver openen via de FQDN van de virtuele machine van Azure. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de implementatie te maken. Elk item in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en netwerkbeveiligingsgroep is verbonden. Met deze opdracht geeft ook de afbeelding van de virtuele machine moet worden gebruikt en de beheerdersreferenties.  |
| [AZ vm open-poort](https://docs.microsoft.com/cli/azure/vm#az_vm_open_port) | Maakt een groep van de netwerkbeveiligingsregel dat binnenkomend verkeer toegestaan. In dit voorbeeld wordt is poort 80 geopend voor HTTP-verkeer. |
| [Azure vm-extensie instellen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Wordt toegevoegd en wordt de extensie van een virtuele machine naar een virtuele machine wordt uitgevoerd. In dit voorbeeld wordt de Docker-VM-extensie gebruikt voor het configureren van een Docker-host.|
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep, met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
