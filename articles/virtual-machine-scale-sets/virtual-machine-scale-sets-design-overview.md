---
title: Ontwerpoverwegingen voor het virtuele Azure-Machine-Schaalsets | Microsoft Docs
description: Meer informatie over overwegingen bij het ontwerpen voor uw Azure virtuele-Machineschaalsets
keywords: virtuele Linux-machine, virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: efb9f7f7daa5dbb8cd3120b21ef812106fdc7fb9
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="design-considerations-for-scale-sets"></a>Ontwerpoverwegingen voor Schaalsets
Dit artikel wordt beschreven ontwerpoverwegingen voor het virtuele-Machineschaalsets. Raadpleeg voor informatie over wat virtuele-Machineschaalsets zijn [overzicht van virtuele machines Scale Sets](virtual-machine-scale-sets-overview.md).

## <a name="when-to-use-scale-sets-instead-of-virtual-machines"></a>Het gebruik van scale Hiermee stelt u in plaats van virtuele machines?
In het algemeen zijn schaalsets nuttig voor het implementeren van maximaal beschikbare infrastructuur waarin een set machines vergelijkbare configuratie hebben. Sommige functies zijn echter alleen beschikbaar in schaalsets terwijl andere functies alleen beschikbaar in virtuele machines zijn. Om te beslissen over het gebruik van elke technologie, moet u eerst een overzicht van enkele van de meest gebruikte functies die beschikbaar in-schaalsets, maar geen virtuele machines zijn uitvoeren:

### <a name="scale-set-specific-features"></a>Scale set-specifieke functies

- Zodra u de schaal configuratie instellen opgeeft, kunt u de eigenschap 'capaciteit' voor het implementeren van meer virtuele machines parallel bijwerken. Dit is veel eenvoudiger dan het schrijven van een script voor de organisatie veel afzonderlijke VM's parallel te implementeren.
- U kunt [Azure automatisch schalen gebruiken voor het automatisch schalen een schaalset](./virtual-machine-scale-sets-autoscale-overview.md) maar geen afzonderlijke virtuele machines.
- U kunt [terugzetten van de installatiekopie schaalset VMs](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-a-vm) maar [geen afzonderlijke virtuele machines](https://docs.microsoft.com/rest/api/compute/virtualmachines).
- U kunt [overprovision](./virtual-machine-scale-sets-design-overview.md) schaalset virtuele machines voor grotere betrouwbaarheid en sneller implementatietijden. U doen dit niet met afzonderlijke virtuele machines tenzij u aangepaste code schrijven voor dit doen.
- Kunt u een [Upgradebeleid](./virtual-machine-scale-sets-upgrade-scale-set.md) gemakkelijker uitrolt upgrades over VM's in de schaalset. Met afzonderlijke virtuele machines, u moet u updates indeelt zelf.

### <a name="vm-specific-features"></a>VM-specifieke functies

Sommige functies zijn alleen beschikbaar in virtuele machines:

- U kunt gegevensschijven koppelen aan specifieke afzonderlijke virtuele machines, maar de bijgesloten gegevensschijven zijn geconfigureerd voor alle virtuele machines in een schaalset.
- Afzonderlijke virtuele machines, maar geen virtuele machines in een schaalset kunt u niet-lege gegevensschijven koppelen.
- U kunt een momentopname van een afzonderlijke VM, maar niet een virtuele machine in een schaalset.
- U kunt een installatiekopie van een afzonderlijke virtuele machine, maar niet op een virtuele machine in een set scale vastleggen.
- U kunt een afzonderlijke virtuele machine migreren vanaf systeemeigen schijven aan beheerde schijven, maar kan geen u dit doen voor virtuele machines in een schaalset.
- U kunt IPv6 openbare IP-adressen toewijzen aan afzonderlijke NIC's van VM maar niet voor virtuele machines in een schaalset. U kunt IPv6 openbare IP-adressen aan load balancers voor een afzonderlijke virtuele machines toewijzen of VM-schaalset.

## <a name="storage"></a>Storage

### <a name="scale-sets-with-azure-managed-disks"></a>Schaalsets met beheerde Azure-schijven
Schaalsets kunnen worden gemaakt met [Azure beheerd schijven](../virtual-machines/windows/managed-disks-overview.md) in plaats van traditionele Azure storage-accounts. Beheerde schijven bieden de volgende voordelen:
- U beschikt niet over een set van Azure storage-accounts vooraf maken voor de VM-schaalset.
- U kunt definiëren [gegevensschijven gekoppeld](virtual-machine-scale-sets-attached-disks.md) ingesteld voor de virtuele machines in de schaal.
- Schaalsets kunnen worden geconfigureerd voor [ondersteuning voor maximaal 1000 virtuele machines in een set](virtual-machine-scale-sets-placement-groups.md). 

Als u een bestaande sjabloon hebt, kunt u ook [bijwerken van de sjabloon voor het gebruik van schijven beheerd](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Opslag gebruikers wordt beheerd
Een schaalset die niet is gedefinieerd met Azure beheerd schijven afhankelijk van de gebruiker gemaakte storage-accounts voor het opslaan van de OS-schijven van de virtuele machines in de set. Een ratio van 20 VM's per storage-account of minder wordt aanbevolen bereiken van maximale i/o en ook te profiteren van _overmatige inrichting_ (Zie hieronder). Het is ook raadzaam dat u het begin van de namen van opslagaccounts verdeeld over het alfabet. Dit doet, helpt load verdeeld over verschillende interne systemen. 


## <a name="overprovisioning"></a>Overmatige inrichting
Schaal stelt momenteel standaard ingesteld op 'overmatige inrichting' virtuele machines. Met overmatige inrichting ingeschakeld, wordt in de schaal daadwerkelijk draaien om zo meer VM's dan u om hebt gevraagd ingesteld, wordt de extra virtuele machines worden verwijderd wanneer het aangevraagde aantal virtuele machines met succes zijn ingericht. Overmatige inrichting verbetert inrichting succespercentages en minder implementatietijd. U wordt niet gefactureerd voor de extra virtuele machines en ze niet meetellen voor uw quotalimieten.

Terwijl overmatige inrichting inrichting succespercentages verbeteren, kunnen er verwarrende gedrag voor een toepassing die niet is ontworpen voor het afhandelen van extra VM's die wordt weergegeven en klik vervolgens verdwijnen. Schakel overmatige inrichting uitschakelen Zorg ervoor dat u hebt de volgende tekenreeks in de sjabloon: `"overprovision": "false"`. Meer informatie vindt u in de [Scale ingesteld REST API-documentatie](/rest/api/virtualmachinescalesets/create-or-update-a-set).

Als uw scale set gebruikers wordt beheerd opslag gebruikt en u overmatige inrichting uitschakelen, kunt u meer dan 20 VM's per storage-account hebben, maar het wordt niet aangeraden om te gaan boven 40 van de i/o-prestaties. 

## <a name="limits"></a>Limieten
Een schaalset gebaseerd op een Marketplace-installatiekopie (ook wel bekend als een platforminstallatiekopie) en geconfigureerd voor het gebruik van Azure beheerd schijven biedt ondersteuning voor een capaciteit van maximaal 1000 virtuele machines. Als u de schaal instelt voor de ondersteuning van meer dan 100 virtuele machines configureert, werken niet in alle scenario's op dezelfde manier (voor een voorbeeld van de taakverdeling). Zie voor meer informatie [werken met grote virtuele machine-schaalsets](virtual-machine-scale-sets-placement-groups.md). 

Een schaal instellen met gebruikers wordt beheerd storage-accounts geconfigureerd is momenteel beperkt tot 100 virtuele machines (en 5 storage-accounts worden aanbevolen voor deze schaal).

Een schaalset gebouwd op een aangepaste installatiekopie (één gebouwd door u) kan een capaciteit van maximaal 300 VM's wanneer geconfigureerd met Azure-beheerde schijven hebben. Als de schaalaanpassingsset is geconfigureerd met opslagaccounts gebruikers wordt beheerd, moet deze alle OS VHD's van schijf binnen een opslagaccount maken. Daardoor het maximale aantal virtuele machines in een set scale is gebouwd op een aangepaste installatiekopie aanbevolen en gebruikers wordt beheerd opslag is 20. Als u overmatige inrichting uitschakelt, kunt u maximaal 40 gaan.

Voor meer virtuele machines dan deze limiet is toegestaan, moet u meerdere schaalsets implementeren, zoals wordt weergegeven in [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).

