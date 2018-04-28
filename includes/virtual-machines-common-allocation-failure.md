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
ms.openlocfilehash: 6377b79d986d32fba8f84c670d6b69d5eda98b8a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
Wanneer u een virtuele machine (VM) maken, gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten of het formaat van een virtuele machine, wijst Microsoft Azure compute-bronnen aan uw abonnement. We zijn voortdurend investeren in extra infrastructuur en functies om ervoor te zorgen dat er altijd alle VM-typen die beschikbaar zijn voor de ondersteuning van de vraag van klanten hebben. Echter kan van tijd tot tijd toewijzingsfouten resource optreden vanwege een ongekende groei van de vraag voor Azure-services in specifieke gebieden. Dit probleem kan optreden wanneer u probeert te maken of virtuele machines in een regio terwijl de virtuele machines worden de volgende foutcode en het bericht weergegeven:

**Foutcode**: AllocationFailed of ZonalAllocationFailed

**Foutbericht**: 'toewijzing is mislukt. Er geen voldoende capaciteit voor de aangevraagde VM-grootte in deze regio. Meer informatie over het verbeteren van de kans op succes bij toewijzing http://aka.ms/allocation-guidance'

Dit artikel wordt uitgelegd van de oorzaken van een deel van de algemene toewijzingsfouten en mogelijke oplossingen wordt voorgesteld.

Als uw Azure probleem niet wordt besproken in dit artikel, gaat u naar de [Azure-forums op MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt het probleem op deze forums of boeken @AzureSupport op Twitter. U kunt ook een aanvraag voor de ondersteuning van Azure indienen door het selecteren van Get-ondersteuning op de [ondersteuning van Azure](https://azure.microsoft.com/support/options/) site.

We raden klanten die problemen met implementatie rekening te houden met de instructies in de volgende tabel als tijdelijke oplossing totdat het type van uw voorkeur VM in uw voorkeur regio beschikbaar is. 

Identificeren van het scenario dat het meest geschikt is voor uw aanvraag en probeer de aanvraag voor geheugentoewijzing met behulp van de bijbehorende voorgestelde oplossing voor vergroot de kans van slagen van de toewijzing. U kunt ook u kunt altijd het later opnieuw. Dit komt doordat er onvoldoende bronnen is mogelijk vrijgemaakt in de regio-, cluster- of zone voor uw aanvraag. 


## <a name="resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Wijzig het formaat van een virtuele machine of virtuele machines toevoegen aan een bestaande beschikbaarheidsset

### <a name="cause"></a>Oorzaak

Een aanvraag om de grootte van een virtuele machine of het toevoegen van dat een virtuele machine naar een bestaande beschikbaarheidsset moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de beschikbaarheid van de bestaande ingesteld. De aangevraagde VM-grootte wordt ondersteund door het cluster, maar het cluster kan momenteel geen voldoende capaciteit. 

### <a name="workaround"></a>Tijdelijke oplossing

Als de virtuele machine deel van een andere beschikbaarheidsset uitmaken kan, maakt u een virtuele machine in een andere beschikbaarheidsset (in dezelfde regio). Deze nieuwe virtuele machine kan vervolgens worden toegevoegd aan hetzelfde virtuele netwerk.

Gestopt (toewijzing ongedaan maken) alle VM's in de dezelfde beschikbaarheid instellen, start vervolgens elke service.
Om te stoppen: klik op resourcegroepen > [uw resourcegroep] > Resources > [uw beschikbaarheidsset] > virtuele Machines > [uw virtuele machine] > stoppen.
Nadat alle virtuele machines stoppen, selecteert u de eerste virtuele machine en klik vervolgens op starten.
Deze stap zorgt ervoor dat er een nieuwe poging van de toewijzing wordt uitgevoerd en dat een nieuw cluster kan worden geselecteerd die voldoende capaciteit heeft.

## <a name="restart-partially-stopped-deallocated-vms"></a>Gedeeltelijk gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten

### <a name="cause"></a>Oorzaak

Gedeeltelijke toewijzing is opgeheven betekent dat u (toewijzing ongedaan gemaakt) een of meer gestopt, maar niet alle virtuele machines in een beschikbaarheidsset ingesteld. Wanneer u een virtuele machine ongedaan gemaakt, worden de bijbehorende bronnen vrijgegeven. Opnieuw opstarten van virtuele machines in een beschikbaarheidsset gedeeltelijk toewijzing ongedaan is gemaakt, is hetzelfde als de virtuele machines toevoegen aan een bestaande beschikbaarheidsset. Daarom kan de aanvraag voor geheugentoewijzing moet worden uitgevoerd op het oorspronkelijke cluster hosts die de bestaande beschikbaarheidsset hebt niet voldoende capaciteit.

### <a name="workaround"></a>Tijdelijke oplossing

Gestopt (toewijzing ongedaan maken) alle VM's in de dezelfde beschikbaarheid instellen, start vervolgens elke service.
Om te stoppen: klik op resourcegroepen > [uw resourcegroep] > Resources > [uw beschikbaarheidsset] > virtuele Machines > [uw virtuele machine] > stoppen.
Nadat alle virtuele machines stoppen, selecteert u de eerste virtuele machine en klik vervolgens op starten.
Hiermee zorgt u ervoor dat er een nieuwe poging van de toewijzing wordt uitgevoerd en dat een nieuw cluster kan worden geselecteerd die voldoende capaciteit heeft.

## <a name="restart-fully-stopped-deallocated-vms"></a>Volledig gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten

### <a name="cause"></a>Oorzaak

Volledige toewijzing is opgeheven betekent dat u gestopt (toewijzing opgeheven) alle VM's in een beschikbaarheidsset. De aanvraag voor geheugentoewijzing opnieuw opstarten van deze virtuele machines heeft betrekking op alle clusters die ondersteuning bieden voor de gewenste grootte binnen de regio of de zone. Uw aanvraag voor geheugentoewijzing per de suggesties in dit artikel te wijzigen en probeer de aanvraag voor het verbeteren van de kans van slagen van de toewijzing. 

### <a name="workaround"></a>Tijdelijke oplossing

Als u oudere VM-reeks of de grootte, zoals Dv1, DSv1, Av1, D15v2 of DS15v2, kunt u verplaatsen naar nieuwere versies. Deze aanbevelingen weergeven voor het specifieke VM-grootten.
Als u de optie voor het gebruik van een andere VM-grootte niet hebt, probeer te implementeren naar een andere regio binnen de dezelfde geo. Voor meer informatie over de beschikbare grootten voor virtuele machine in elke regio op https://aka.ms/azure-regions

Als u van beschikbaarheid zones gebruikmaakt, probeert u een andere zone binnen de regio waarvoor beschikbare capaciteit voor de aangevraagde VM-grootte.

Als uw aanvraag voor geheugentoewijzing groot is (meer dan 500 kernen), Zie de informatie in de volgende secties om de aanvraag in kleinere implementaties.

## <a name="allocation-failures-for-older-vm-sizes-av1-dv1-dsv1-d15v2-ds15v2-etc"></a>Toewijzingsfouten voor oudere VM-grootten (Av1, Dv1, DSv1, D15v2, DS15v2, enz.)

Als we Azure-infrastructuur uitbreiden, implementeren we nieuwere generatie hardware die is ontworpen ter ondersteuning van de meest recente typen virtuele machines. Sommige van de oudere reeks virtuele machines niet uitgevoerd op onze nieuwste generatie infrastructuur. Om deze reden kunnen klanten soms toewijzingsfouten problemen voor deze oudere SKU's. U voorkomt dit probleem, raden we klanten die oudere reeks virtuele machines in overweging moet nemen verplaatsen naar de equivalente nieuwere VM's per de volgende aanbevelingen: deze VM's zijn geoptimaliseerd voor de meest recente hardware- en kunt u profiteren van betere prijzen en prestaties. 

|VM-reeks of grootte van de verouderde|Nieuwere VM-reeks of grootte van de aanbevolen|Meer informatie|
|----------------------|----------------------------|--------------------|
|Av1-serie|[Av2-serie](../articles/virtual-machines/windows/sizes-general.md#av2-series)|https://azure.microsoft.com/blog/new-av2-series-vm-sizes/
|Dv1 of DSv1-serie (D1 naar D5)|[Dv3 of DSv3-serie](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-sup1sup)|https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/
|Dv1 of DSv1-serie (D11 naar D14)|[Ev3 of ESv3-serie](../articles/virtual-machines/windows/sizes-memory.md#esv3-series-sup1sup)|
|D15v2 of DS15v2|Als u theResource Manager-implementatiemodel gebruikt om te profiteren van de grotere VM-grootten, kunt u D16v3/DS16v3 of D32v3/DS32v3 te verplaatsen. Deze zijn ontworpen om uit te voeren op de meest recente generatie hardware. Als u het implementatiemodel van Resource Manager gebruikt om ervoor te zorgen dat uw VM-instantie is geïsoleerd, zodat de hardware die zijn toegewezen aan één klant, kunt u verplaatsen naar de nieuwe geïsoleerde VM-grootten, E64i_v3 of E64is_v3, die zijn ontworpen om uit te voeren op de meest recente generatie hardware. |https://azure.microsoft.com/blog/new-isolated-vm-sizes-now-available/

## <a name="allocation-failures-for-large-deployments-more-than-500-cores"></a>Toewijzingsfouten voor grote implementaties (meer dan 500 kernen)

Verminder het aantal exemplaren van de aangevraagde VM-grootte en probeer het opnieuw implementeren. Bovendien voor grotere implementaties u mogelijk wilt evalueren [virtuele Azure-machine-schaalsets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Het aantal VM-exemplaren automatisch verhoogd of verlaagd in reactie op aanvraag of een ingesteld schema, en u hebt een grotere kans van slagen van de toewijzing omdat de implementaties kunnen worden verdeeld over meerdere clusters. 

## <a name="background-information"></a>Achtergrondinformatie
### <a name="how-allocation-works"></a>De werking van toewijzing
De servers in Azure-datacenters worden in clusters gepartitioneerd. Normaal gesproken wordt er een toewijzingsaanvraag in meerdere clusters uitgevoerd, maar het is mogelijk dat bepaalde beperkingen van de toewijzingsaanvraag het Azure-platform ertoe dwingen de aanvraag in slechts één cluster uit te voeren. In dit artikel verwijzen we naar dit zoals "vastgemaakt aan een cluster." Afbeelding 1 hieronder ziet u dat het geval van een normale toewijzing waartoe in meerdere clusters. Diagram 2 ziet u dat het geval van een toewijzing die is vastgemaakt aan Cluster 2 omdat waar de bestaande Cloud Service CS_1 of beschikbaarheid set wordt gehost.
![Diagram van toewijzing](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Waarom toewijzingsfouten gebeuren
Wanneer een aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, is er een hogere kans niet te vinden van resources vrij, omdat de groep beschikbare resource kleiner is. Bovendien als uw aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, maar het type resource dat u hebt aangevraagd wordt niet ondersteund door dit cluster, zal uw aanvraag mislukken zelfs als het cluster resources vrij heeft. Het volgende Diagram 3 ziet u dat het geval waarbij een vastgezette toewijzing is mislukt omdat het cluster alleen candidate geen gratis resources. Diagram 4 ziet u het geval waarbij een vastgezette toewijzing is mislukt omdat het cluster alleen candidate biedt geen ondersteuning voor de aangevraagde VM-grootte, zelfs als het cluster resources vrij heeft.

![Vastgemaakte geheugentoewijzing is mislukt](./media/virtual-machines-common-allocation-failure/Allocation2.png)


