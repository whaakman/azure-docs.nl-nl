---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/10/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1582f61befb9b6d71adbfda9482175d67874ffb9
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850273"
---
# <a name="availability-options-for-virtual-machines-in-azure"></a>Beschikbaarheids opties voor virtuele machines in azure
In dit artikel vindt u een overzicht van de beschik baarheid van Azure virtual machines (Vm's).


## <a name="availability-sets"></a>Beschikbaarheidssets
Een beschikbaarheidsset is een logische groepering van Vm's binnen een Data Center waarmee Azure kan begrijpen hoe uw toepassing is gebouwd om te voorzien in redundantie en beschik baarheid. We raden u aan twee of meer virtuele machines te maken binnen een beschikbaarheidsset om te voorzien in een Maxi maal beschik bare toepassing en om te voldoen aan de [99,95% Azure Sla](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Er zijn geen kosten voor de Beschikbaarheidsset zelf, u betaalt alleen voor elk VM-exemplaar dat u maakt. Wanneer één virtuele machine gebruikmaakt van [Azure Premium ssd's](../articles/virtual-machines/windows/disks-types.md#premium-ssd), is de Azure-sla van toepassing op niet-geplande onderhouds gebeurtenissen.

Een beschikbaarheidsset bestaat uit twee extra groeperingen die bescherming bieden tegen hardwarestoringen en dat updates veilig kunnen worden toegepast-fout domeinen (Fd's) en update domeinen (UDs). Lees meer over het beheren van de beschikbaarheid van [Linux-VM's](../articles/virtual-machines/linux/manage-availability.md) of [Windows VM's](../articles/virtual-machines/windows/manage-availability.md).

### <a name="fault-domains"></a>Foutdomeinen
Een foutdomein is een logische groep onderliggende hardware met een gemeenschappelijke voeding en netwerkswitch, vergelijkbaar met een rack in een on-premises datacenter. Wanneer u virtuele machines in een beschikbaarheidsset maakt, verdeelt het Azure-platform uw virtuele machines automatisch tussen deze foutdomeinen. Deze aanpak beperkt de gevolgen van mogelijke problemen met de fysieke hardware, netwerkstoringen of stroomonderbrekingen.

### <a name="update-domains"></a>Updatedomeinen
Een updatedomein is een logische groep onderliggende hardware die op hetzelfde moment onderhoud kan ondergaan of opnieuw kan worden opgestart. Wanneer u virtuele machines in een beschikbaarheidsset maakt, verdeelt het Azure-platform uw virtuele machines automatisch tussen deze updatedomeinen. Deze aanpak zorgt ervoor dat altijd ten minste één exemplaar van uw toepassing beschikbaar blijft wanneer er periodiek onderhoud wordt uitgevoerd aan het Azure-platform. De volgorde waarin updatedomeinen opnieuw worden opgestart, verloopt tijdens gepland onderhoud niet altijd sequentieel, maar er wordt slechts één updatedomein tegelijk opnieuw opgestart.

![Beschikbaarheidssets](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains"></a>Fout domeinen voor beheerde schijven
Voor virtuele machines die gebruikmaken van [Azure Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md) en deel uitmaken van een beheerde beschikbaarheidsset, worden de virtuele machines afgestemd op Managed Disk-foutdomeinen. Deze afstemming zorgt ervoor dat alle beheerde schijven die zijn gekoppeld aan een virtuele machine, zich binnen hetzelfde Managed Disk-foutdomein bevinden. In een beheerde beschikbaarheidsset kunnen alleen virtuele machines met beheerde schijven worden gemaakt. Het aantal Managed Disk-foutdomeinen verschilt per regio: er zijn twee of drie Managed Disk-foutdomeinen per regio. Meer informatie over deze Managed Disk-fout domeinen voor [Linux-vm's](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) of [Windows-vm's](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set)vindt u hier.

![Beheerde beschikbaarheidsset](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

## <a name="availability-zones"></a>Beschikbaarheidszones

[Beschikbaarheids zones](../articles/availability-zones/az-overview.md), een alternatief voor beschikbaarheids sets, vouw het beheer niveau uit dat u nodig hebt om de beschik baarheid van de toepassingen en gegevens op uw vm's te behouden. Een beschikbaarheidszone is een fysiek afgescheiden zone binnen een Azure-regio. Er zijn drie Beschikbaarheidszones per ondersteunde Azure-regio. Elke beschikbaarheidszone heeft een afzonderlijke voedingsbron en koeling, en een afzonderlijk netwerk. Door uw oplossingen te ontwikkelen voor het gebruik van gerepliceerde Vm's in zones, kunt u uw apps en gegevens beveiligen tegen verlies van een Data Center. Als er een zone wordt aangetast, zijn de gerepliceerde apps en gegevens direct beschikbaar in een andere zone. 

![Beschikbaarheidszones](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Meer informatie over het implementeren van een virtuele [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) -of [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) -machine in een beschikbaarheids zone.


## <a name="next-steps"></a>Volgende stappen
U kunt nu deze functies voor beschikbaarheid en redundantie gaan gebruiken om uw eigen Azure-omgeving te bouwen. Zie voor informatie over aanbevolen procedures de [aanbevolen procedures voor Azure-beschikbaarheid](../articles/best-practices-availability-checklist.md).

