---
title: Virtuele machine van Azure netwerkdoorvoer | Microsoft Docs
description: Meer informatie over de netwerkdoorvoer van de virtuele machine van Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: f22b6f361f0c5bea547721309bb0f75b62f18d92
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="virtual-machine-network-bandwidth"></a>Virtuele machine netwerkbandbreedte

Azure biedt tal van VM-grootten en typen, elk met een andere combinatie van prestaties levert. Een mogelijkheid is netwerk doorvoer (of bandbreedte), gemeten in megabits per seconde (Mbps). Omdat de virtuele machines worden gehost op gedeelde hardware, moet de netwerkcapaciteit redelijk worden gedeeld tussen de virtuele machines delen dezelfde hardware. Grotere virtuele machines worden toegewezen relatief meer bandbreedte dan kleinere virtuele machines.
 
De netwerkbandbreedte die wordt toegewezen aan elke virtuele machine wordt gemeten op uitgaande (uitgaand) verkeer van de virtuele machine. Alle netwerkverkeer verlaten van de virtuele machine worden geteld voor de limiet van de toegewezen, ongeacht de bestemming. Bijvoorbeeld, als een virtuele machine een 1000 Mbps limiet heeft, deze limiet is van toepassing of het uitgaande verkeer is bestemd voor een andere virtuele machine in hetzelfde virtuele netwerk of buiten Azure.
 
Inkomend is geen datalimiet of rechtstreeks beperkt. Er zijn echter andere factoren, zoals CPU en opslaglimieten die invloed op de mogelijkheid een virtuele machine hebben kunnen voor inkomende gegevens.

Versnelde netwerken is een functie die is ontworpen voor betere prestaties van het netwerk, met inbegrip van latentie, doorvoer en CPU-gebruik. Terwijl versnelde netwerken van doorvoer van een virtuele machine verbeteren kunt, kan deze worden gedaan alleen tot de virtuele machine bandbreedte toegewezen. Zie voor meer informatie over Accelerated netwerken, Accelerated netwerken voor [Windows](create-vm-accelerated-networking-powershell.md) of [Linux](create-vm-accelerated-networking-cli.md) virtuele machines.
 
Virtuele machines in Azure moet één, maar kan diverse, gekoppeld aan deze netwerkinterfaces hebben. Bandbreedte die is toegewezen aan een virtuele machine is de som van al het uitgaande verkeer voor alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine. Met andere woorden, is de toegewezen bandbreedte per virtuele machine, ongeacht hoeveel netwerkinterfaces zijn gekoppeld aan de virtuele machine. Zie voor meer informatie over het aantal netwerkinterfaces ondersteuning van andere Azure VM-grootten, Azure [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-grootten. 

## <a name="expected-network-throughput"></a>Verwachte netwerkdoorvoer

Verwachte uitgaande doorvoer en het aantal netwerkinterfaces ondersteund door elke VM-grootte is uiteengezet in Azure [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-grootten. Selecteer een type, zoals Algemeen, en selecteer vervolgens een grootte-reeks op de resulterende pagina, zoals de Dv2-serie. Elke reeks bevat een tabel met specificaties in de laatste kolom met de titel toegang **Max NIC's / netwerkervaring voor prestaties (Mbps) verwacht**. 

De maximale doorvoer geldt voor de virtuele machine. Doorvoer wordt niet beïnvloed door de volgende factoren:
- **Het aantal netwerkinterfaces**: de Bandbreedtelimiet cumulatieve van al het uitgaande verkeer van de virtuele machine is.
- **Netwerken versnelde**: hoewel de functie nuttig zijn bij het bereiken van de gepubliceerde limiet zijn kan, de limiet niet gewijzigd.
- **Het doel van verkeer**: alle bestemmingen meetellen voor de uitgaande limiet.
- **Protocol**: al het uitgaande verkeer via alle protocollen telt naar de limiet.

## <a name="next-steps"></a>Volgende stappen

- [Optimaliseren van de netwerkdoorvoer voor een besturingssysteem van de virtuele machine](virtual-network-optimize-network-bandwidth.md)
- [Test netwerkdoorvoer](virtual-network-bandwidth-testing.md) voor een virtuele machine.
