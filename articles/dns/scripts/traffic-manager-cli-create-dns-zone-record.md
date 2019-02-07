---
title: 'CLI-voorbeeld: een DNS-zone maken en vastleggen voor een domeinnaam - Azure | Microsoft Docs'
description: In dit voorbeeld van een Azure CLI-script ziet u hoe u een DNS-zone maakt en registreert voor een domeinnaam
services: load-balancer
documentationcenter: traffic-manager
author: vhorne
manager: jeconnoc
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/30/2018
ms.author: victorh
ms.openlocfilehash: 59ec8d4f93b18469818c9ead2e965679e41360ae
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694649"
---
# <a name="azure-cli-script-example-create-a-dns-zone-and-record"></a>Azure CLI-voorbeeldscript: Een DNS-zone en -record maken

In dit voorbeeld van een Azure CLI-script wordt een DNS-zone gemaakt en geregistreerd voor een domeinnaam. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

```azurecli-interactive

# Create a resource group.
az group create \
  -n myResourceGroup \
  -l eastus

# Create a DNS zone. Substitute zone name "contoso.com" with the values for your own.

az network dns zone create \
  -g MyResourceGroup \
  -n contoso.com

# Create a DNS record. Substitute zone name "contoso.com" and IP address "1.2.3.4* with the values for your own.

az network dns record-set a add-record \
  -g MyResourceGroup \
  -z contoso.com \
  -n www \
  -a 1.2.3.4

# Get a list the DNS records in your zone
az network dns record-set list \
  -g MyResourceGroup \ 
  -z contoso.com
```

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht uit om de resourcegroep, DNS-zone en alle gerelateerde resources te verwijderen.

```azurecli
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, een virtuele machine, een beschikbaarheidsset, een load balancer en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network dns zone create](/cli/azure/network/dns/zone#az-network-dns-zone-create) | Hiermee wordt een Azure DNS-zone gemaakt. |
| [az network dns record-set a add-record](/cli/azure/network/dns/record-set) | Hiermee wordt een *A*-record toegevoegd aan een DNS-zone. |
| [az network dns record-set list](/cli/azure/network/dns/record-set) | Hiermee worden alle *A*-recordsets in een DNS-zone weergegeven. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

