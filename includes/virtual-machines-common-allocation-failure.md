---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, azure-resource-manager
author: genlin
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/14/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 2eb7fb82b358d4ec8628bfa546b572ee3cbe47fa
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40210205"
---
Wanneer u een virtuele machine (VM) maken, gestopt (toewijzing opgeheven) virtuele machines opnieuw opstarten of een virtuele machine vergroten of verkleinen, wijst Microsoft Azure compute-resources aan uw abonnement. We zijn voortdurend investeren in extra infrastructuur en onderdelen om ervoor te zorgen dat we alle VM-typen beschikbaar voor ondersteuning van de vraag van klanten altijd beschikken over. Echter kan van tijd tot tijd toewijzingsfouten resource optreden vanwege een ongekende groei in de vraag naar Azure-services in specifieke regio's. Dit probleem kan optreden wanneer u probeert te maken of virtuele machines in een regio te starten terwijl de virtuele machines de volgende foutcode en het bericht weergegeven:

**Foutcode**: AllocationFailed of ZonalAllocationFailed

**Foutbericht**: "toewijzing is mislukt. We hebben niet voldoende capaciteit voor de aangevraagde VM-grootte in deze regio. Meer informatie over het verbeteren van de kans op succes bij http://aka.ms/allocation-guidance"

Dit artikel worden de oorzaken van enkele van de algemene toewijzingsfouten en duidt op mogelijke oplossingen.

Als uw Azure-probleem niet wordt besproken in dit artikel, gaat u naar de [Azure-forums op MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt uw probleem boeken op deze forums of @AzureSupport op Twitter. U kunt ook een Azure-ondersteuningsaanvraag bestand door het selecteren van Get-ondersteuning op de [ondersteuning van Azure](https://azure.microsoft.com/support/options/) site.

Pas uw gewenste VM-type beschikbaar in uw voorkeursregio is, adviseren we klanten die problemen met implementatie rekening te houden met de instructies in de volgende tabel als een tijdelijke oplossing. 

Zoek het scenario dat het meest geschikt is voor uw situatie en probeer de aanvraag voor geheugentoewijzing door gebruik te maken van de bijbehorende voorgestelde oplossing om de kans op succes. U kunt ook u kunt altijd probeer het later opnieuw. Dit is omdat er onvoldoende bronnen zijn vrijgegeven in de cluster, regio of zone voor uw aanvraag. 


## <a name="resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Grootte van een virtuele machine of VM's toevoegen aan een bestaande beschikbaarheidsset

### <a name="cause"></a>Oorzaak

Een verzoek om de grootte van een virtuele machine of het toevoegen van dat een virtuele machine aan een bestaande beschikbaarheidsset moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande beschikbaarheidsset is ingesteld. De aangevraagde VM-grootte wordt ondersteund door het cluster, maar het cluster kan momenteel geen voldoende capaciteit. 

### <a name="workaround"></a>Tijdelijke oplossing

Als de virtuele machine deel van een andere beschikbaarheidsset uitmaken kan, maakt u een virtuele machine in een andere beschikbaarheidsset (in dezelfde regio). Deze nieuwe virtuele machine kan vervolgens worden toegevoegd aan hetzelfde virtuele netwerk.

Stoppen (toewijzing ongedaan maken) alle virtuele machines in dezelfde beschikbaarheidsset instellen, start vervolgens elke service.
Om te stoppen: klik op Resource groups > [uw resourcegroep] > Resources > [uw beschikbaarheidsset] > virtuele Machines > [uw virtuele machine] > stoppen.
Nadat alle virtuele machines stoppen, selecteert u de eerste virtuele machine en klik vervolgens op starten.
Deze stap zorgt ervoor dat er een nieuwe poging van de toewijzing wordt uitgevoerd en dat een nieuw cluster kan worden geselecteerd die voldoende capaciteit heeft.

## <a name="restart-partially-stopped-deallocated-vms"></a>Gedeeltelijk gestopt (toewijzing opgeheven) virtuele machines opnieuw opstarten

### <a name="cause"></a>Oorzaak

Gedeeltelijke ongedaan wordt gemaakt betekent dat u bent (toewijzing opgeheven) een of meer gestopt, maar niet alle virtuele machines in een beschikbaarheidsset ingesteld. Wanneer u de toewijzing van een virtuele machine ongedaan maken, wordt de bijbehorende bronnen worden vrijgegeven. Opnieuw opstarten van virtuele machines in een beschikbaarheidsset gedeeltelijk toewijzing ongedaan wordt gemaakt, is hetzelfde als de virtuele machines toe te voegen aan een bestaande beschikbaarheidsset. De toewijzingsaanvraag moet daarom worden uitgevoerd op het oorspronkelijke cluster hosts die de bestaande die beschikbaarheidsset mogelijk niet voldoende capaciteit.

### <a name="workaround"></a>Tijdelijke oplossing

Stoppen (toewijzing ongedaan maken) alle virtuele machines in dezelfde beschikbaarheidsset instellen, start vervolgens elke service.
Om te stoppen: klik op Resource groups > [uw resourcegroep] > Resources > [uw beschikbaarheidsset] > virtuele Machines > [uw virtuele machine] > stoppen.
Nadat alle virtuele machines stoppen, selecteert u de eerste virtuele machine en klik vervolgens op starten.
Hiermee zorgt u ervoor dat er een nieuwe poging van de toewijzing wordt uitgevoerd en dat een nieuw cluster kan worden geselecteerd die voldoende capaciteit heeft.

## <a name="restart-fully-stopped-deallocated-vms"></a>Volledig gestopt (toewijzing opgeheven) virtuele machines opnieuw opstarten

### <a name="cause"></a>Oorzaak

Volledige toewijzing is opgeheven betekent dat u bent gestopt (toewijzing opgeheven) alle virtuele machines in een beschikbaarheidsset. De toewijzingsaanvraag aan deze virtuele machines opnieuw opstarten heeft betrekking op alle clusters die ondersteuning bieden voor de gewenste grootte in de regio of zone. Wijzigen van uw aanvraag voor geheugentoewijzing per de suggesties in dit artikel en probeer de aanvraag voor het verbeteren van de kans op succes. 

### <a name="workaround"></a>Tijdelijke oplossing

Als u oudere VM-reeks of formaten, zoals Dv1, zoals DSv1, Av1, D15v2 of DS15v2, kunt u overwegen om naar nieuwere versies. Deze aanbevelingen weergeven voor specifieke VM-grootten.
Als u niet de optie voor het gebruik van een andere VM-grootte hebt, kunt u implementeren naar een andere regio binnen hetzelfde geografische gebied. Voor meer informatie over de beschikbare VM-grootten in elke regio op https://aka.ms/azure-regions

Als u van beschikbaarheidszones gebruikmaakt, probeert u een andere zone in het gebied dat mogelijk beschikbare capaciteit voor de aangevraagde VM-grootte.

Als uw aanvraag voor geheugentoewijzing groot (meer dan 500 kernen), Zie de informatie in de volgende secties het opsplitsen van de aanvraag in kleinere implementaties.

## <a name="allocation-failures-for-older-vm-sizes-av1-dv1-dsv1-d15v2-ds15v2-etc"></a>Toewijzingsfouten voor oudere VM-grootten (Av1, Dv1, zoals DSv1, D15v2, DS15v2, enz.)

Als we Azure-infrastructuur uitbreiden, implementeren we nieuwere generatie hardware die is ontworpen ter ondersteuning van de nieuwste typen virtuele machines. Sommige van de oudere serie VM's niet uitgevoerd op de nieuwste generatie infrastructuur. Om deze reden kunnen klanten toewijzingsfouten af en toe optreden voor deze verouderde SKU's. Als u wilt voorkomen dat dit probleem, we adviseren klanten die werken met verouderde reeks virtuele machines om te overwegen verplaatsen naar de equivalente nieuwere VM's per de volgende aanbevelingen: deze VM's zijn geoptimaliseerd voor de nieuwste hardware en kunt u profiteren van betere prijzen en prestaties. 

|VM-reeks of grootte van de verouderde|Aanbevolen VM-reeks of grootte van de nieuwere|Meer informatie|
|----------------------|----------------------------|--------------------|
|Av1-serie|[Av2-serie](../articles/virtual-machines/windows/sizes-general.md#av2-series)|https://azure.microsoft.com/blog/new-av2-series-vm-sizes/
|Dv1 of DSv1-serie (D1 tot D5)|[Dv3 of Dsv3-serie](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-sup1sup)|https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/
|Dv1 of DSv1-serie (D11 tot D14)|[Ev3 of Esv3-serie](../articles/virtual-machines/windows/sizes-memory.md#esv3-series)|
|D15v2 of DS15v2|Als u theResource Manager-implementatiemodel gebruikt om te profiteren van de grotere VM-grootten, kunt u overwegen E16v3/E16sv3 of E32v3/E32sv3 te verplaatsen. Deze zijn ontworpen om uit te voeren op de nieuwste generatie hardware. Als u van het implementatiemodel van Resource Manager gebruikmaakt om ervoor te zorgen dat uw VM-exemplaar is geïsoleerd voor hardware toegewezen aan één klant, kunt u overwegen om naar de nieuwe geïsoleerde VM-grootten, E64i_v3 of E64is_v3, die zijn ontworpen om uit te voeren op de nieuwste generatie hardware. |https://azure.microsoft.com/blog/new-isolated-vm-sizes-now-available/


## <a name="allocation-failures-for-large-deployments-more-than-500-cores"></a>Toewijzingsfouten voor grote implementaties (meer dan 500 kernen)

Verminder het aantal exemplaren van de aangevraagde VM-grootte en probeer vervolgens opnieuw te implementeren. Bovendien voor grotere implementaties kunt u evalueren [Azure virtuele-machineschaalsets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Kan het aantal VM-exemplaren automatisch vergroten of afnemen in reactie op aanvraag of volgens een ingesteld schema, en u hebt een grotere kans van slagen van de toewijzing van omdat de implementaties kunnen worden verdeeld over meerdere clusters. 

## <a name="background-information"></a>Meer informatie
### <a name="how-allocation-works"></a>De werking van toewijzing
De servers in Azure-datacenters worden in clusters gepartitioneerd. Normaal gesproken wordt er een toewijzingsaanvraag in meerdere clusters uitgevoerd, maar het is mogelijk dat bepaalde beperkingen van de toewijzingsaanvraag het Azure-platform ertoe dwingen de aanvraag in slechts één cluster uit te voeren. In dit artikel verwijzen we naar dit als "vastgemaakt aan een cluster." Afbeelding 1 hieronder ziet u dat het geval van een normale verdeling die wordt uitgevoerd in meerdere clusters. Diagram 2 ziet u het geval van een toewijzing die is vastgemaakt aan het Cluster 2 omdat daar waar de bestaande set met Cloud Service CS_1 of beschikbaarheid wordt gehost.
![Diagram van toewijzing](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Waarom toewijzingsfouten gebeuren
Wanneer een aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, is er een hogere kans niet te zoeken naar gratis bronnen omdat de groep beschikbare resource kleiner is. Bovendien, als uw aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, maar het type resource dat u hebt aangevraagd niet wordt ondersteund door het cluster, uw aanvraag mislukt, zelfs als het cluster gratis resources heeft. Het volgende Diagram 3 ziet u dat het geval waarbij een vastgemaakte toewijzing is mislukt omdat het cluster alleen candidate heeft geen gratis resources. Diagram 4 ziet u het geval waarbij een vastgemaakte toewijzing is mislukt omdat het cluster alleen candidate biedt geen ondersteuning voor de aangevraagde VM-grootte, zelfs als het cluster gratis resources heeft.

![Vastgemaakte toewijzingsfout](./media/virtual-machines-common-allocation-failure/Allocation2.png)


