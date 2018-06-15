---
title: Zoeken en verwijderen van niet-gekoppelde NIC's van Azure | Microsoft Docs
description: Het vinden en verwijderen van de Azure-NIC's die niet zijn gekoppeld aan virtuele machines met de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: c730866fe73305a37b37038699a7f729085a16aa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31427019"
---
# <a name="how-to-find-and-delete-unattached-network-interface-cards-nics-for-azure-vms"></a>Het vinden en verwijderen van niet-gekoppelde netwerkinterface kaarten (NIC's) voor Azure Virtual machines
Wanneer u een virtuele machine (VM) in Azure verwijdert, worden de netwerkinterfacekaarten (NIC's) worden niet standaard verwijderd. Als u maken en verwijderen van meerdere virtuele machines, blijven de ongebruikte NIC's gebruiken van interne IP-adresleases. Bij het maken van andere VM-NIC, is het mogelijk dat ze niet verkrijgen van een IP-adreslease in de adresruimte van het subnet. In dit artikel leest u hoe om te zoeken en verwijderen van niet-gekoppelde NIC's.

## <a name="find-and-delete-unattached-nics"></a>Zoeken en verwijderen van niet-gekoppelde NIC 's

De *virtualMachine* opgeslagen bij de groep-ID en de bron van de virtuele machine de NIC is gekoppeld aan de eigenschap voor een NIC. Het volgende script doorlopen alle NIC's in een abonnement en controleert u of de *virtualMachine* eigenschap is null. Als deze eigenschap null is, wordt de NIC is niet gekoppeld aan een virtuele machine.

Als u wilt weergeven van de niet-gekoppelde NIC's, het heeft wordt ten zeerste aanbevolen voor de eerste keer uitvoert het script dat de *deleteUnattachedNics* variabele *0*. Als u wilt de niet-gekoppelde NIC's verwijderen nadat u de uitvoer van de lijst, voer het script met *deleteUnattachedNics* naar *1*.

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
