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
ms.openlocfilehash: f403e060859df6d1de96a3c0d478d57df2677eee
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "31531052"
---
Hieronder vindt u algemene scenario's toewijzing die ertoe leiden dat een aanvraag voor geheugentoewijzing om te worden vastgemaakt. We je Duik in elk scenario verderop in dit artikel.

- Wijzig het formaat van een virtuele machine of virtuele machines of rolinstanties toevoegen aan een bestaande cloudservice
- Gedeeltelijk gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
- Volledig gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
- Fasering en productie-implementaties (platform als een service alleen)
- Affiniteitsgroep (virtuele machine of service nabijheid)
- Virtueel netwerk op basis van affiniteit: groep

Wanneer u een toewijzingsfout ontvangt, moet u controleren of de vermelde scenario's van toepassing zijn op uw fout. Gebruik de toewijzingsfout die wordt geretourneerd door de Azure-platform voor het identificeren van het betreffende scenario. Als uw aanvraag is vastgemaakt, verwijder enkele van de vastmaken beperkingen uw aanvraag om meer clusters, waardoor de kans van slagen van de toewijzing te openen.
In het algemeen als de fout wordt niet vermeld dat 'het aangevraagde VM-grootte niet wordt ondersteund', u kunt altijd opnieuw uitvoeren op een later tijdstip. Dit komt doordat er onvoldoende bronnen is mogelijk vrijgemaakt in het cluster aan uw aanvraag. Als het probleem is dat de aangevraagde VM-grootte niet wordt ondersteund, kunt u een andere VM-grootte. Anders is de enige optie als de vastmaken beperking wilt verwijderen.

Twee algemene scenario's met fouten gerelateerd aan affiniteitsgroepen. In het verleden een affiniteitsgroep is gebruikt om te bieden dicht in de VM's en service-exemplaren of voor het maken van een virtueel netwerk is gebruikt. Dankzij de introductie van regionale virtuele netwerken, affiniteitsgroepen niet langer zijn vereist voor het maken van een virtueel netwerk. Met de vermindering van de netwerklatentie in Azure-infrastructuur, is de aanbeveling om affiniteitsgroepen gebruikt voor virtuele machines of op afstand van de service gewijzigd.

Het volgende Diagram toont de taxonomie van de toewijzing van (vastgemaakt)-scenario's. 

![Vastgezette toewijzing taxonomie](./media/virtual-machines-common-allocation-failure/Allocation3.png)

## <a name="resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Wijzig het formaat van een virtuele machine of virtuele machines of rolinstanties toevoegen aan een bestaande cloudservice
**Fout**

Upgrade_VMSizeNotSupported of GeneralError

**Oorzaak van het cluster vastmaken**

Een aanvraag om de grootte van een virtuele machine of een virtuele machine of een rolinstantie toevoegen aan een bestaande cloudservice moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande cloudservice. Een nieuwe cloudservice maakt, wordt de Azure-platform om te zoeken naar een ander cluster die ondersteuning biedt voor de VM-grootte die u hebt aangevraagd of resources vrij heeft.

**Tijdelijke oplossing**

Als de fout is Upgrade_VMSizeNotSupported *, kunt u een andere VM-grootte. Als met behulp van een andere VM-grootte kan niet worden gebruikt, maar als het is acceptabel gebruik van een ander virtueel IP-adres (VIP), maakt u een nieuwe cloudservice voor het hosten van de nieuwe virtuele machine en de nieuwe cloudservice toevoegen aan het regionale virtuele netwerk waarin de bestaande virtuele machines worden uitgevoerd. Als uw bestaande cloudservice niet voor een regionaal virtueel netwerk gebruikt wordt, kunt u nog steeds een nieuw virtueel netwerk voor de nieuwe cloudservice maken en maken vervolgens verbinding met uw [bestaand virtueel netwerk met de nieuwe virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Als de fout is GeneralError *, is het waarschijnlijk dat het type resource (zoals een bepaalde VM-grootte) wordt ondersteund door het cluster, maar het cluster geen gratis resources op het moment dat heeft. Vergelijkbaar met het bovenstaande scenario, de gewenste compute resource bij het maken van een nieuwe cloudservice (Let erop dat de nieuwe cloudservice heeft tot het gebruik van een andere VIP) toevoegen en gebruiken van een regionaal virtueel netwerk verbinding maken uw cloudservices verkregen.

## <a name="restart-partially-stopped-deallocated-vms"></a>Gedeeltelijk gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
**Fout**

GeneralError *

**Oorzaak van het cluster vastmaken**

Gedeeltelijke toewijzing is opgeheven betekent dat u (toewijzing ongedaan gemaakt) een of meer, maar niet alle virtuele machines in een cloudservice gestopt. Wanneer u stopt (ongedaan gemaakt) een virtuele machine, de bijbehorende bronnen worden vrijgegeven. Opnieuw starten die gestopt (toewijzing ongedaan gemaakt) VM is daarom een nieuwe aanvraag voor geheugentoewijzing. Opnieuw opstarten van virtuele machines in een gedeeltelijk toewijzing ongedaan is gemaakt van de cloudservice is gelijk aan het toevoegen van virtuele machines aan een bestaande cloudservice. De aanvraag voor geheugentoewijzing moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande cloudservice. Een andere cloudservice maakt, wordt de Azure-platform om te zoeken naar een ander cluster die ondersteuning biedt voor de VM-grootte die u hebt aangevraagd of gratis resource heeft.

**Tijdelijke oplossing**

Als het aanvaardbaar is voor gebruik van een andere VIP, verwijderen van de gestopt (toewijzing ongedaan gemaakt) virtuele machines (maar de gekoppelde schijven behouden) en de virtuele machines opnieuw door een andere cloudservice toevoegen. Gebruik een regionaal virtueel netwerk voor de verbinding met uw cloudservices:

* Als uw bestaande cloudservice een regionaal virtueel netwerk gebruikt, moet u gewoon de nieuwe cloudservice toevoegen aan hetzelfde virtuele netwerk.
* Als uw bestaande cloudservice niet voor een regionaal virtueel netwerk gebruikt wordt, een nieuw virtueel netwerk voor de nieuwe cloudservice maken en vervolgens [uw bestaande virtuele netwerk verbinden met de nieuwe virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="restart-fully-stopped-deallocated-vms"></a>Volledig gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
**Fout**

GeneralError *

**Oorzaak van het cluster vastmaken**

Volledige toewijzing is opgeheven betekent dat u gestopt (toewijzing opgeheven) alle VM's van een cloudservice. De toewijzingsaanvragen voor deze virtuele machines opnieuw moeten worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Een nieuwe cloudservice maakt, wordt de Azure-platform om te zoeken naar een ander cluster die ondersteuning biedt voor de VM-grootte die u hebt aangevraagd of resources vrij heeft.

**Tijdelijke oplossing**

Als deze aanvaardbaar is voor gebruik van een andere VIP, verwijderen van de oorspronkelijke gestopt (toewijzing ongedaan gemaakt) virtuele machines (maar houd het bijbehorende schijven) en verwijder de bijbehorende cloudservice (de bijbehorende rekenresources zijn al is vrijgegeven wanneer u gestopt (toewijzing ongedaan gemaakt) de virtuele machines). Maak een nieuwe cloudservice voor het toevoegen van de virtuele machines terug.

## <a name="stagingproduction-deployments-platform-as-a-service-only"></a>Fasering/productie-implementaties (platform als een service alleen)
**Fout**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vastmaken**

De implementatie van fasering en productie-implementatie van een cloudservice worden gehost in hetzelfde cluster. Wanneer u de tweede implementatie toevoegt, wordt de bijbehorende toewijzingsaanvraag geprobeerd in hetzelfde cluster die als host fungeert voor de eerste implementatie.

**Tijdelijke oplossing**

Verwijder de eerste implementatie en de oorspronkelijke cloudservice en implementeer de cloudservice opnieuw. Deze actie kan de eerste implementatie terechtkomen in een cluster met onvoldoende resources vrij voor beide implementaties of in een cluster die ondersteuning biedt voor de VM-grootten die u hebt aangevraagd.

## <a name="affinity-group-vmservice-proximity"></a>Affiniteitsgroep (VM-/ service nabijheid)
**Fout**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vastmaken**

Een compute resource toegewezen aan een affiniteitsgroep is gekoppeld aan één cluster. Nieuwe compute resource aanvragen in de betreffende affiniteitsgroep bevinden worden geprobeerd in hetzelfde cluster waar de bestaande bronnen worden gehost. Dit geldt of de nieuwe resources zijn gemaakt via een nieuwe cloudservice of een bestaande cloudservice.

**Tijdelijke oplossing**

Als een affiniteitsgroep niet nodig is, gebruikt u een affiniteitsgroep of uw rekenresources te groeperen in meerdere affiniteitsgroepen.

## <a name="affinity-group-based-virtual-network"></a>Virtueel netwerk op basis van affiniteit-groep
**Fout**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vastmaken**

Voordat de regionale virtuele netwerken zijn geïntroduceerd, moest u een virtueel netwerk koppelen aan een affiniteitsgroep. Als gevolg hiervan compute resources in een affiniteitsgroep geplaatst zijn gebonden aan de dezelfde beperkingen, zoals beschreven in de ' toewijzing scenario: affiniteitsgroep (VM-/ service nabijheid) ' hierboven. De rekenresources zijn gekoppeld aan één cluster.

**Tijdelijke oplossing**

Als u niet in een affiniteitsgroep hoeft, maakt u een nieuw regionaal virtueel netwerk voor de nieuwe resources die u toevoegen wilt, en vervolgens [uw bestaande virtuele netwerk verbinden met de nieuwe virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

U kunt ook [uw virtueel netwerk voor op basis van affiniteit-groep migreren naar een regionaal virtueel netwerk](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), en voeg vervolgens de gewenste bronnen opnieuw toe.
