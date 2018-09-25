---
title: Zoeken en verwijderen van niet-gekoppelde NIC's van Azure | Microsoft Docs
description: Over het zoeken en verwijderen van Azure-NIC's die niet zijn gekoppeld aan virtuele machines met de Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: cynthn
ms.openlocfilehash: 9d8345aacb603a6411fdc693ac9f808778d27333
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46954327"
---
# <a name="how-to-find-and-delete-unattached-network-interface-cards-nics-for-azure-vms"></a>Over het zoeken en verwijderen van niet-gekoppelde netwerkinterface kaarten (NIC's) voor virtuele machines van Azure
Wanneer u een virtuele machine (VM) in Azure verwijdert, worden de netwerkinterfacekaarten (NIC's) worden niet standaard verwijderd. Als u maken en verwijderen van meerdere virtuele machines, blijven de niet-gebruikte NIC's gebruiken van het interne IP-adresleases. Als u andere VM-NIC's maken, is het mogelijk dat ze niet verkrijgen van een IP-adresleases in de adresruimte van het subnet. Dit artikel leest u hoe om te zoeken en niet-gekoppelde NIC's verwijderen.

## <a name="find-and-delete-unattached-nics"></a>Niet-gekoppelde NIC's zoeken en verwijderen

De *virtualMachine* opgeslagen bij de groep-ID en resource van de virtuele machine de NIC is gekoppeld aan de eigenschap voor een NIC. Het volgende script wordt uitgevoerd via alle NIC's in een abonnement en controleert u of de *virtualMachine* eigenschap null is. Als deze eigenschap null is, wordt de NIC is niet gekoppeld aan een virtuele machine.

Als u wilt weergeven van alle niet-gekoppelde NIC's, het heeft raden u aan voor eerste uitvoering van het script met de *deleteUnattachedNics* variabele *0*. Als u wilt de niet-gekoppelde NIC's verwijderen nadat u de uitvoer bekijken, voer het script met *deleteUnattachedNics* naar *1*.

```azurecli
# Set deleteUnattachedNics=1 if you want to delete unattached NICs
# Set deleteUnattachedNics=0 if you want to see the Id(s) of the unattached NICs
deleteUnattachedNics=0

unattachedNicsIds=$(az network nic list --query '[?virtualMachine==`null`].[id]' -o tsv)
for id in ${unattachedNicsIds[@]}
do
   if (( $deleteUnattachedNics == 1 ))
   then

       echo "Deleting unattached NIC with Id: "$id
       az network nic delete --ids $id
       echo "Deleted unattached NIC with Id: "$id
   else
       echo $id
   fi
done
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken en beheren van virtuele netwerken in Azure [maken en beheren van VM-netwerken](tutorial-virtual-network.md).
