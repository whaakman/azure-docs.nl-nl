---
title: Netwerkdoorvoer van virtuele Azure-machine | Microsoft Docs
description: Meer informatie over de netwerkdoorvoer van virtuele machine van Azure.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: kumud
ms.openlocfilehash: 29c4926f56070874fe17622170e697986df0fbc3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743074"
---
# <a name="virtual-machine-network-bandwidth"></a>Virtuele machine netwerkbandbreedte

Azure biedt tal van VM-grootten en -typen, elk met een andere combinatie van prestaties levert. Een mogelijkheid is netwerk doorvoer (of bandbreedte), gemeten in megabits per seconde (Mbps). Omdat virtuele machines worden gehost op gedeelde hardware, moet de netwerkcapaciteit redelijk worden gedeeld met de virtuele machines delen dezelfde hardware. Grotere virtuele machines zijn relatief meer bandbreedte dan kleinere virtuele machines toegewezen.
 
De bandbreedte van het netwerk toegewezen aan elke virtuele machine wordt gemeten op de uitgaande gegevens (uitgaand) verkeer van de virtuele machine. Alle netwerkverkeer verlaten van de virtuele machine wordt meegeteld voor de limiet van de toegewezen, ongeacht de bestemming. Bijvoorbeeld, als een virtuele machine een limiet van 1000 Mbps heeft, geldt dat een limiet of het uitgaande verkeer bestemd voor een andere virtuele machine in hetzelfde virtuele netwerk en buiten Azure is.
 
Inkomend verkeer wordt niet gemeten of die beperkt zijn rechtstreeks. Er zijn echter andere factoren, zoals CPU- en opslaglimieten die kunnen van invloed op de mogelijkheid van een virtuele machine voor het verwerken van inkomende gegevens.

Versneld netwerken is een functie die is ontworpen voor betere prestaties van het netwerk, met inbegrip van latentie, doorvoer en CPU-gebruik. Terwijl versneld netwerken de doorvoer van een virtuele machine verbeteren kan, deze mogelijk alleen up wordt gemaakt op de virtuele machine bandbreedte toegewezen. Zie voor meer informatie over versnelde netwerken, versneld netwerken voor [Windows](create-vm-accelerated-networking-powershell.md) of [Linux](create-vm-accelerated-networking-cli.md) virtuele machines.
 
Virtuele machines van Azure, moet een, maar kan er meerdere, netwerkinterfaces die zijn gekoppeld aan deze hebben. Bandbreedte die is toegewezen aan een virtuele machine is de som van al het uitgaande verkeer voor alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine. Met andere woorden, is de toegewezen bandbreedte per virtuele machine, ongeacht het aantal netwerkinterfaces zijn gekoppeld aan de virtuele machine. Zie voor meer het aantal netwerkinterfaces van verschillende Azure-VM-grootten ondersteuning Azure [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-grootten. 

## <a name="expected-network-throughput"></a>Verwachte netwerkdoorvoer

Verwachte uitgaande doorvoer en het aantal netwerkinterfaces dat wordt ondersteund door elke VM-grootte wordt beschreven in Azure [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-grootten. Selecteer een type, zoals algemeen gebruik, en selecteer vervolgens een grootte-serie op de resulterende pagina, zoals de Dv2-serie. Elke reeks bevat een tabel met netwerken specificaties in de laatste kolom met de titel **max. aantal NIC's / verwachte netwerkprestaties (Mbps)**. 

De maximale doorvoer is van toepassing op de virtuele machine. Doorvoer wordt niet beïnvloed door de volgende factoren:
- **Het aantal netwerkinterfaces**: De limiet voor bandbreedte is cumulatieve van al het uitgaande verkeer van de virtuele machine.
- **Versnelde netwerken**: Hoewel de functie handig is bij het bereiken van de gepubliceerde limiet, verandert de limiet niet meer.
- **Het doel van verkeer**: Alle bestemmingen tellen mee voor de limiet voor uitgaande.
- **Protocol**: Al het uitgaande verkeer via alle protocollen telt voor de limiet.

## <a name="next-steps"></a>Volgende stappen

- [Netwerkdoorvoer optimaliseren voor het besturingssysteem van een virtuele machine](virtual-network-optimize-network-bandwidth.md)
- [Test de netwerkdoorvoer](virtual-network-bandwidth-testing.md) voor een virtuele machine.
