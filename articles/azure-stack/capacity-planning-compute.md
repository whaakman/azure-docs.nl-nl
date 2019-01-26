---
title: COMPUTE-capaciteitsplanning voor Azure Stack | Microsoft Docs
description: Meer informatie over de compute-capaciteitsplanning voor Azure Stack-implementaties.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeffgilb
ms.reviewer: prchint
ms.custom: mvc
ms.openlocfilehash: 314d40ba365f6dc9a279744ac3af874057fd2321
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076788"
---
# <a name="azure-stack-compute-capacity-planning"></a>Azure Stack-compute-capaciteitsplanning
De [VM-grootten in Azure Stack ondersteund](./user/azure-stack-vm-sizes.md) vormen een subset van die worden ondersteund op Azure. Azure legt resourcebeperkingen langs veel vectoren om te voorkomen dat overmatig van resources (server-service op het niveau en lokale). Zonder enige beperkingen met betrekking tot tenantverbruik opleggen, afnemen de tenant-ervaringen wanneer andere tenants overconsume resources. Voor netwerken uitgaande verkeer van de virtuele machine zijn er bandbreedte limieten in plaats van Azure Stack die overeenkomen met de Azure-beperkingen. Storage-resources, opslag IOPs-limieten geïmplementeerd in Azure Stack om te voorkomen dat basic overmatig van resources door tenants voor toegang tot opslag.  

## <a name="vm-placement-and-virtual-to-physical-core-overprovisioning"></a>VM-plaatsing en aan fysieke virtuele kern overmatige inrichting
In Azure Stack is er geen manier voor een tenant om op te geven van een specifieke server moet worden gebruikt voor VM-plaatsing. De enige overweging bij het plaatsen van virtuele machines is of er voldoende geheugen beschikbaar op de host voor dit type virtuele machine is. Azure Stack komt niet overcommit geheugen. echter, een overcommit van het aantal kernen is toegestaan. Aangezien plaatsing algoritmen niet op de bestaande virtuele kernverhouding fysieke overmatige inrichting als factor vindt, kan elke host een andere breedteverhouding hebben. 

In Azure, voor het bereiken van hoge beschikbaarheid van het productiesysteem van een multi-VM, worden VM's geplaatst in een beschikbaarheidsset om te worden verdeeld over meerdere foutdomeinen. Een foutdomein in een beschikbaarheidsset is in Azure Stack gedefinieerd als een enkel knooppunt in de schaaleenheid.

Terwijl de infrastructuur van Azure Stack tegen uitvallen is, de onderliggende technologie (Failoverclustering) nog steeds leidt tot enige uitvaltijd voor virtuele machines op een betrokken fysieke server in het geval van een hardwarestoring optreedt. Op dit moment ondersteunt Azure Stack met een beschikbaarheidsset met een maximum van drie foutdomeinen zodat deze overeenkomt met Azure. Virtuele machines in een beschikbaarheidsset geplaatst worden fysiek van elkaar geïsoleerd door ze zo gelijkmatig mogelijk te spreiden over meerdere foutdomeinen (Azure Stack-knooppunten). Als er een hardwarestoring optreedt, wordt virtuele machines van de mislukte foutdomein opnieuw gestart in de andere knooppunten, maar, indien mogelijk wordt gehouden in afzonderlijke foutdomeinen van de andere virtuele machines in dezelfde beschikbaarheidsset. Wanneer de hardware weer online komt, worden virtuele machines worden uitgevoerd voor het onderhouden van hoge beschikbaarheid.

Er is een andere concept, dat wordt gebruikt door Azure voor hoge beschikbaarheid in de vorm van update-domeinen in beschikbaarheidssets. Een updatedomein is een logische groep onderliggende hardware die op hetzelfde moment onderhoud kan ondergaan of opnieuw kan worden opgestart. In Azure Stack zijn virtuele machines live op andere online hosts in het cluster wordt gemigreerd voordat de hun onderliggende host is bijgewerkt. Omdat er geen tenant-downtime tijdens het bijwerken van een host is, wordt de updatefunctie-domein in Azure Stack alleen bestaat voor de sjabloon voor compatibiliteit met Azure.

## <a name="azure-stack-resiliency-resources"></a>Azure Stack-resources voor tolerantie
Om toe te staan voor patches en updates van een Azure Stack-geïntegreerd systeem, en als u tegen problemen met de fysieke hardware, een deel van het totale servergeheugen is gereserveerd en niet beschikbaar voor plaatsing van de tenant-virtuele machines (VM).

Als een server uitvalt, kunnen virtuele machines die worden gehost op de server wordt opnieuw gestart op resterende, beschikbare servers om te bieden voor VM-beschikbaarheid. Op deze manier tijdens het proces voor patches en updates, worden alle virtuele machines die worden uitgevoerd op een server bevinden zich gemigreerd naar andere beschikbare, server. Deze VM-beheer of verkeer kan alleen worden bereikt als er gereserveerde capaciteit om toe te staan voor het opnieuw opstarten of de migratie plaatsvindt.

De volgende berekening resulteert in de totale beschikbare geheugen die kan worden gebruikt voor de tenant-VM-plaatsing. Deze capaciteit van het geheugen is voor het geheel van de Azure Stack-schaaleenheid.

  Beschikbaar geheugen voor VM-plaatsing = totale Server – tolerantie Reserve – Azure Stack-infrastructuur geheugenoverhead <sup>1</sup>

  Tolerantie reserve = H + R * (N-1) + V * (N-2)

> Waar:
> - H = grootte van één server-geheugen
> - N = grootte van de schaaleenheid (aantal servers)
> - R = besturingssysteem reserveren voor OS-overhead<sup>2</sup>
> - V = grootste virtuele machine in de schaaleenheid

  <sup>1</sup> azure Stack-infrastructuur Overhead = 208 GB

  <sup>2</sup> besturingssysteem reserveren voor overhead = 15% van het knooppunt geheugen. De reservewaarde besturingssysteem is een schatting en zijn afhankelijk van de capaciteit van het fysieke geheugen van de server en de overhead van algemene besturingssysteem.

De waarde V grootste virtuele machine in de schaaleenheid is dynamisch op basis van de grootste tenant VM geheugengrootte. De grootste waarde van de virtuele machine kan bijvoorbeeld worden 7 GB of 112 GB of een andere ondersteunde VM geheugengrootte in de Azure Stack-oplossing.

De bovenstaande berekening is een schatting en onderhevig aan wijzigingen op basis van de huidige versie van Azure Stack. Mogelijkheid tot het implementeren van de tenant-VM's en services is gebaseerd op de details van de geïmplementeerde oplossing. Deze berekening voorbeeld is slechts een handleiding en niet het absolute antwoord van de mogelijkheid tot het implementeren van virtuele machines.



## <a name="next-steps"></a>Volgende stappen
[Opslag-capaciteitsplanning](capacity-planning-storage.md)
