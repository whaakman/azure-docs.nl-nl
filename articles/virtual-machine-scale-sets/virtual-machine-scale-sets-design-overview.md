---
title: Ontwerpoverwegingen voor Azure Virtual Machine Scale Sets | Microsoft Docs
description: Meer informatie over overwegingen bij het ontwerp voor uw Azure Virtual Machine Scale Sets
keywords: virtuele Linux-machine, virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
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
ms.author: manayar
ms.openlocfilehash: 67bbad7e73f33d73d4c3f1d4f7e5599d2ef914e3
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53791044"
---
# <a name="design-considerations-for-scale-sets"></a>Ontwerpoverwegingen voor Schaalsets
Dit artikel worden besproken ontwerpoverwegingen voor Schaalsets voor virtuele machines. Raadpleeg voor informatie over wat er in Virtual Machine Scale Sets zijn [Virtual Machine Scale Sets overzicht](virtual-machine-scale-sets-overview.md).

## <a name="when-to-use-scale-sets-instead-of-virtual-machines"></a>Wanneer u scale Hiermee stelt u in plaats van virtuele machines?
Over het algemeen zijn schaalsets nuttig voor het implementeren van maximaal beschikbare infrastructuur waarin een set van machines vergelijkbare configuratie heeft. Sommige functies zijn echter alleen beschikbaar in schaalsets terwijl andere functies alleen beschikbaar in virtuele machines zijn. Als u wilt maken van een gefundeerde beslissing nemen over het gebruik van elke technologie, moet u eerst een overzicht van enkele van de meest gebruikte functies die beschikbaar in schaalsets, maar geen virtuele machines zijn uitvoeren:

### <a name="scale-set-specific-features"></a>Scale set-specifieke functies

- Als u opgeeft in de configuratie van de schaalset, kunt u bijwerken de *capaciteit* eigenschap meer virtuele machines parallel te implementeren. Dit proces is het beter dan het schrijven van een script voor de organisatie van veel afzonderlijke virtuele machines tegelijkertijd implementeren.
- U kunt [voor automatisch schalen van Azure gebruiken voor het automatisch schalen van een schaalset](./virtual-machine-scale-sets-autoscale-overview.md) , maar niet voor afzonderlijke VM's.
- U kunt [terugzetten van een installatiekopie schaalset-VM's](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage) maar [niet voor afzonderlijke VM's](https://docs.microsoft.com/rest/api/compute/virtualmachines).
- U kunt [overprovision](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning) schaalset-VM's voor hogere mate van betrouwbaarheid en snellere implementatietijden. U kunt geen afzonderlijke virtuele machines overprovision, tenzij u aangepaste code schrijven voor deze actie uitvoeren.
- Kunt u een [Upgradebeleid](./virtual-machine-scale-sets-upgrade-scale-set.md) om gemakkelijk upgrades voor virtuele machines in de schaalset worden uitgerold. Met afzonderlijke virtuele machines, moet u uzelf updates indeelt.

### <a name="vm-specific-features"></a>VM-specifieke functies

Sommige functies zijn momenteel alleen beschikbaar in virtuele machines:

- U kunt een installatiekopie maken van een afzonderlijke virtuele machine, maar niet vanaf een virtuele machine in een schaalset.
- U kunt een afzonderlijke virtuele machine van systeemeigen schijven migreren naar managed disks, maar u kunt VM-exemplaren in een schaalset niet migreren.
- U kunt openbare IPv6-adressen toewijzen aan afzonderlijke VM virtuele netwerkinterfacekaarten (NIC's), maar niet voor VM-exemplaren in een schaalset. U kunt openbare IPv6-adressen aan load balancers in het zicht van een afzonderlijke virtuele machines of Virtual Machine scale sets virtuele machines.

## <a name="storage"></a>Storage

### <a name="scale-sets-with-azure-managed-disks"></a>Schaalsets met Azure Managed Disks
Schaalsets kunnen worden gemaakt met [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md) in plaats van traditionele Azure storage-accounts. Beheerde schijven bieden de volgende voordelen:
- U hoeft niet vooraf een set met Azure storage-accounts maken voor virtuele machines met schaalsets.
- U kunt definiëren [gekoppelde gegevensschijven](virtual-machine-scale-sets-attached-disks.md) instellen voor de virtuele machines in uw schaalset.
- Schaalsets kunnen worden geconfigureerd om te [ondersteuning voor maximaal 1000 virtuele machines in een set](virtual-machine-scale-sets-placement-groups.md). 

Als u een bestaande sjabloon hebt, kunt u ook [bijwerken van de sjabloon voor het gebruik van Managed Disks](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Gebruiker beheerde opslag
Een schaalset die niet met Azure Managed Disks is gedefinieerd, is afhankelijk van gebruikers gecreëerde storage-accounts voor het opslaan van de OS-schijven van de virtuele machines in de set. Een ratio van 20 VM's per opslagaccount of minder wordt aanbevolen om bereiken van maximale i/o en ook profiteren van _piekmomenten_ (Zie hieronder). Het is ook raadzaam dat u het begin van de namen van opslagaccounts verdeeld over het alfabet. Hierdoor helpt belasting verdelen over verschillende interne systemen. 


## <a name="overprovisioning"></a>Overmatige inrichting
Schaalsets die momenteel standaard ingesteld op 'piekmomenten' virtuele machines. Overprovisioning is ingeschakeld, de schaal daadwerkelijk fabriceert van meer virtuele machines bevat dan u gevraagd om instellen en vervolgens de extra virtuele machines worden verwijderd zodra het aangevraagde aantal virtuele machines zijn ingericht. Overmatige inrichting succespercentages verbetert en minder implementatietijd. U niet worden in rekening gebracht voor de extra virtuele machines, en ze niet meetellen voor de grenzen van uw quotum.

Tijdens piekmomenten inrichting succespercentages verbeteren, kan dit leiden tot verwarring gedrag voor een toepassing die niet is ontworpen voor het afhandelen van extra virtuele machines die wordt weergegeven en vervolgens verdwijnen. Als u wilt inschakelen op piekmomenten uitschakelen, zorg ervoor dat u hebt de volgende tekenreeks in de sjabloon: `"overprovision": "false"`. Meer informatie vindt u de [schaal ingesteld REST API-documentatie](/rest/api/virtualmachinescalesets/create-or-update-a-set).

Als uw schaalset maakt gebruik van gebruikers beheerde opslag, en u overmatige inrichting uitschakelen, kunt u meer dan 20 VM's per opslagaccount hebt, maar het wordt afgeraden om door te gaan boven de 40 voor i/o-prestaties. 

## <a name="limits"></a>Limieten
Een schaalset die is gebouwd op een Marketplace-installatiekopie (ook wel bekend als een platforminstallatiekopie) en geconfigureerd voor het gebruik van Azure Managed Disks biedt ondersteuning voor een capaciteit van maximaal 1000 virtuele machines. Als u uw schaalset ter ondersteuning van meer dan 100 virtuele machines configureert, wordt er niet in alle scenario's werken hetzelfde (voor een voorbeeld van de load balancing). Zie voor meer informatie, [werken met grote virtuele-machineschaalsets](virtual-machine-scale-sets-placement-groups.md). 

Een schaalset die is geconfigureerd met de gebruiker beheerde opslagaccounts is momenteel beperkt tot 100 virtuele machines (en 5 opslagaccounts worden aanbevolen voor deze).

Een schaalset die is gebouwd op een aangepaste installatiekopie (één die door u zijn gebouwd) hebben een capaciteit van maximaal 600 virtuele machines wanneer geconfigureerd met Azure Managed disks. Als de schaalset is geconfigureerd met de opslagaccounts die door de gebruiker wordt beheerd, moet deze alle OS VHD's met gegevensschijven binnen een opslagaccount maken. Als gevolg hiervan het maximale aantal virtuele machines in een schaalset die is gebouwd op een aangepaste installatiekopie aanbevolen en de gebruiker beheerde opslag is 20. Als u overmatige inrichting uitschakelt, kunt u maximaal 40 gaan.

Voor meer virtuele machines dan deze limieten, toestaan, moet u meerdere schaalsets implementeren zoals wordt weergegeven in [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).

