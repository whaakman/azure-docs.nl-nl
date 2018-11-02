---
title: Het oplossen van toewijzingsfouten voor virtuele Azure-machine in het klassieke implementatiemodel | Microsoft Docs
description: Toewijzingsfouten bij het maken, opnieuw opstarten, of het formaat van een klassieke virtuele machine in Azure oplossen
services: azure-service-management
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 7cd7897e3a0b940bbc636b2fbc3dbbc13b7cf540
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748422"
---
# <a name="troubleshooting-steps-specific-to-allocation-failure-scenarios-in-the-classic-deployment-model"></a>Stappen voor probleemoplossing specifieke toewijzing foutscenario's in het klassieke implementatiemodel

Hieronder vindt u algemene scenario's toewijzing die ertoe leiden dat een aanvraag voor geheugentoewijzing om te worden vastgemaakt. We u Duik in elk scenario verderop in dit artikel.

- Grootte van een virtuele machine of virtuele machines of rolinstanties toevoegen aan een bestaande cloudservice
- Gedeeltelijk gestopt (toewijzing opgeheven) virtuele machines opnieuw opstarten
- Volledig gestopt (toewijzing opgeheven) virtuele machines opnieuw opstarten
- Fasering en productie-implementaties (platform als een service alleen)
- Affiniteitsgroep (VM of service nabijheid)
- Affiniteit-groep op basis van het virtuele netwerk

Wanneer u een fout bij toewijzen ontvangt, controleert u of een van de vermelde scenario's van toepassing op de fout zijn. Gebruik de toewijzingsfout dat wordt geretourneerd door de Azure-platform voor het identificeren van het betreffende scenario. Als uw aanvraag is vastgemaakt, verwijdert u enkele van de vastgemaakte bestanden beperkingen voor het openen van uw aanvraag met meer clusters, waardoor de kans op succes.
In het algemeen als de fout wordt niet vermeld dat 'het aangevraagde VM-grootte wordt niet ondersteund', u kunt altijd opnieuw uitvoeren op een later tijdstip. Dit komt doordat er voldoende bronnen zijn vrijgegeven in het cluster om uw aanvraag mogelijk te maken. Als het probleem is dat de aangevraagde VM-grootte niet wordt ondersteund, kunt u een andere VM-grootte. Anders is de enige optie als de vastgemaakte bestanden beperking wilt verwijderen.

Twee veelvoorkomende foutscenario's zijn gerelateerd aan affiniteitsgroepen. In het verleden een affiniteitsgroep is gebruikt voor dicht in de VM's en service-exemplaren of deze is gebruikt voor het maken van een virtueel netwerk. Affiniteitsgroepen zijn met de introductie van regionale virtuele netwerken, niet meer vereist voor het maken van een virtueel netwerk. Met de verlaging van de netwerklatentie in Azure-infrastructuur, is de aanbeveling voor affiniteitsgroepen voor virtuele machines of service nabijheid gewijzigd.

Het volgende Diagram toont de taxonomie van de toewijzing van (vastgemaakte)-scenario's. 

![Vastgemaakte toewijzing taxonomie](./media/virtual-machines-common-allocation-failure/Allocation3.png)

## <a name="resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Grootte van een virtuele machine of virtuele machines of rolinstanties toevoegen aan een bestaande cloudservice
**Fout**

Upgrade_VMSizeNotSupported of GeneralError

**Oorzaak van het cluster vast te maken**

Een aanvraag om de grootte van een virtuele machine of een virtuele machine of een rolinstantie toevoegen aan een bestaande cloudservice moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande cloudservice. Het maken van een nieuwe cloudservice, kunt het Azure-platform om een ander cluster dat is gratis resources of biedt ondersteuning voor de VM-grootte die u hebt aangevraagd te vinden.

**Tijdelijke oplossing**

Als de fout is Upgrade_VMSizeNotSupported *, probeert u een andere VM-grootte. Als u met behulp van een andere VM-grootte kan niet worden gebruikt, maar als het is aanvaardbaar is voor het gebruik van een ander virtueel IP-adres (VIP), maakt u een nieuwe cloudservice als host van de nieuwe virtuele machine en de nieuwe service in de cloud toevoegen aan het regionale virtuele netwerk waar de bestaande virtuele machines worden uitgevoerd. Als uw bestaande cloudservice niet voor een regionaal virtueel netwerk gebruikt wordt, kunt u nog steeds een nieuw virtueel netwerk voor de nieuwe cloudservice maken en vervolgens verbinding maken met uw [bestaand virtueel netwerk naar de nieuw virtueel netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Als de fout is GeneralError *, is het waarschijnlijk dat het type resource (zoals een bepaalde VM-grootte) wordt ondersteund door het cluster, maar het cluster heeft geen gratis resources op dit moment. Net als bij het bovenstaande scenario, de gewenste compute-resource bij het maken van een nieuwe cloudservice (Houd er rekening mee dat de nieuwe service in de cloud heeft tot het gebruik van een andere VIP) toevoegen en verbinding maken met uw cloudservices met een regionaal virtueel netwerk.

## <a name="restart-partially-stopped-deallocated-vms"></a>Gedeeltelijk gestopt (toewijzing opgeheven) virtuele machines opnieuw opstarten
**Fout**

GeneralError *

**Oorzaak van het cluster vast te maken**

Gedeeltelijke ongedaan wordt gemaakt betekent dat u (toewijzing opgeheven) een of meer, maar niet alle virtuele machines in een cloudservice gestopt. Wanneer u stoppen (toewijzing ongedaan maken) een virtuele machine, de bijbehorende bronnen worden vrijgegeven. Opnieuw starten die gestopt (toewijzing opgeheven) virtuele machine is daarom een nieuwe aanvraag voor geheugentoewijzing. Opnieuw opstarten van virtuele machines in een gedeeltelijk toewijzing ongedaan wordt gemaakt van de cloudservice is gelijk aan het virtuele machines toe te voegen aan een bestaande cloudservice. De toewijzingsaanvraag bevat om te worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande cloudservice. Het maken van een andere cloudservice, kunt het Azure-platform om een ander cluster dat is gratis resource of biedt ondersteuning voor de VM-grootte die u hebt aangevraagd te vinden.

**Tijdelijke oplossing**

Als het is aanvaardbaar is voor gebruik van een andere VIP-adres, de gestopt (toewijzing opgeheven) virtuele machines verwijderen (maar houden van de gekoppelde schijven) en de virtuele machines via een andere cloudservice toevoegt. Gebruik een regionaal virtueel netwerk om uw cloudservices verbinding te maken:

* Als uw bestaande cloudservice maakt gebruik van een regionaal virtueel netwerk, moet u gewoon de nieuwe service in de cloud toevoegen aan hetzelfde virtuele netwerk.
* Als uw bestaande cloudservice niet voor een regionaal virtueel netwerk gebruikt wordt, een nieuw virtueel netwerk voor de nieuwe cloudservice maken en vervolgens [verbinding maken met uw bestaande virtuele netwerk naar de nieuw virtueel netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="restart-fully-stopped-deallocated-vms"></a>Volledig gestopt (toewijzing opgeheven) virtuele machines opnieuw opstarten
**Fout**

GeneralError *

**Oorzaak van het cluster vast te maken**

Volledige toewijzing is opgeheven betekent dat u bent gestopt (toewijzing opgeheven) alle virtuele machines van een service in de cloud. De toewijzingsaanvragen voor deze virtuele machines opnieuw opstarten moeten worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Het maken van een nieuwe cloudservice, kunt het Azure-platform om een ander cluster dat is gratis resources of biedt ondersteuning voor de VM-grootte die u hebt aangevraagd te vinden.

**Tijdelijke oplossing**

Als het is aanvaardbaar is voor gebruik van een andere VIP-adres, verwijderen van de oorspronkelijke gestopt (toewijzing opgeheven) virtuele machines (maar de gekoppelde schijven behouden) en de bijbehorende cloudservice verwijderen (de rekenresources die gekoppeld zijn al vrijgegeven wanneer u bent gestopt (toewijzing opgeheven) de VM's). Maak een nieuwe cloudservice om toe te voegen van de virtuele machines weer.

## <a name="stagingproduction-deployments-platform-as-a-service-only"></a>Fasering/productie-implementaties (platform als een service alleen)
**Fout**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vast te maken**

De staging-implementatie en de productie-implementatie van een service in de cloud worden gehost in hetzelfde cluster. Wanneer u de tweede implementatie toevoegt, wordt de bijbehorende toewijzingsaanvraag worden uitgevoerd in hetzelfde cluster die als host fungeert voor de eerste implementatie.

**Tijdelijke oplossing**

De eerste implementatie en de oorspronkelijke cloudservice verwijderen en opnieuw implementeren van de cloudservice. Deze actie kan de eerste implementatie terechtkomen in een cluster met voldoende gratis resources om aan te passen van beide implementaties of in een cluster die ondersteuning biedt voor de VM-grootten die u hebt aangevraagd.

## <a name="affinity-group-vmservice-proximity"></a>Affiniteitsgroep (nabijheid VM/service)
**Fout**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vast te maken**

Een compute-resource die is toegewezen aan een affiniteitsgroep is gekoppeld aan een cluster. Nieuwe compute-resource-aanvragen in die affiniteitsgroep bevinden worden uitgevoerd in hetzelfde cluster waar de bestaande resources worden gehost. Dit is van toepassing of de nieuwe resources worden gemaakt via een nieuwe cloudservice of een bestaande cloudservice.

**Tijdelijke oplossing**

Als een affiniteitsgroep niet nodig is, gebruikt u een affiniteitsgroep of uw compute-resources te groeperen in meerdere affiniteitsgroepen.

## <a name="affinity-group-based-virtual-network"></a>Affiniteit groep gebaseerd virtueel netwerk
**Fout**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vast te maken**

Voordat u regionale virtuele netwerken zijn geïntroduceerd, moest u een virtueel netwerk koppelen aan een affiniteitsgroep. Als gevolg hiervan, compute-resources in een affiniteitsgroep geplaatst zijn gebonden aan de dezelfde beperkingen, zoals beschreven in de ' toewijzing scenario: affiniteitsgroep (nabijheid VM/service) ' hierboven. De compute-resources zijn gekoppeld aan een cluster.

**Tijdelijke oplossing**

Als u niet in een affiniteitsgroep hoeft, maakt u een nieuw regionaal virtueel netwerk voor de nieuwe resources die u toevoegen wilt, en vervolgens [verbinding maken met uw bestaande virtuele netwerk naar de nieuw virtueel netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

U kunt ook [uw affiniteit groep gebaseerd virtueel netwerk migreren naar een regionaal virtueel netwerk](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), en vervolgens de gewenste resources opnieuw toevoegen.


