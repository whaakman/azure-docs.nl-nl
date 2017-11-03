---
title: Azure CLI Script voorbeeld - een virtuele Linux-machine maken met NGINX | Microsoft Docs
description: Azure CLI Script voorbeeld - een virtuele Linux-machine maken met NGINX
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0a6c33d84f1fab85e6ed2933c47c041ca2e59520
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-nginx"></a>Een virtuele machine maken met NGINX

Dit script wordt een Azure virtuele Machine gemaakt en gebruikt de aangepaste Scriptextensie voor Azure virtuele Machine voor het installeren van NGINX. Nadat het script is uitgevoerd, kunt u toegang tot een website demo voor het openbare IP-adres van de virtuele machine.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a>Aangepaste scriptextensie

De extensie voor aangepaste scripts kopieert u dit script op de virtuele machine. Het script wordt vervolgens uitgevoerd om te installeren en configureren van een NGINX-webserver. 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Maakt de virtuele machine. Met deze opdracht geeft ook de afbeelding van de virtuele machine moet worden gebruikt en de beheerdersreferenties.  |
| [AZ vm open-poort](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Maakt een groep van de netwerkbeveiligingsregel dat binnenkomend verkeer toegestaan. In dit voorbeeld wordt is poort 80 geopend voor HTTP-verkeer. |
| [Azure vm-extensie instellen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Wordt toegevoegd en wordt de extensie van een virtuele machine naar een virtuele machine wordt uitgevoerd. In dit voorbeeld wordt de extensie voor aangepaste scripts worden gebruikt voor het installeren van NGINX.|
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
