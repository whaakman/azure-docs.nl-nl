---
title: Capaciteitsplanning voor Azure Stack | Microsoft Docs
description: Meer informatie over capaciteitsplanning voor Azure Stack-implementaties.
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
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 09/18/2018
ms.openlocfilehash: 3d825a0f8a23380b4d9cf453076ab4b18ee67831
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58095514"
---
# <a name="azure-stack-capacity-planning"></a>Azure Stack-capaciteitsplanning
Bij het evalueren van een Azure Stack-oplossing, zijn er hardware-configuratie-opties die een directe invloed op de totale capaciteit van de Azure Stack-Cloud hebben. Dit zijn de klassieke keuzes van CPU, geheugen dichtheid, opslagconfiguratie, en algemene oplossing schalen of het aantal servers. De eenvoudige rekenkundige van deze onderdelen om te bepalen aan bruikbare capaciteit in tegenstelling tot een traditionele virtualisatieoplossing, is niet van toepassing. De eerste reden hiervoor is dat Azure Stack is ontworpen voor het hosten van de infrastructuur of management onderdelen binnen de oplossing zelf. De tweede reden is dat een aantal van de oplossing capaciteit wordt gereserveerd ter ondersteuning van tolerantie; het bijwerken van de software van de oplossing op een manier om onderbreking van de tenantwerkbelastingen te minimaliseren.

> [!IMPORTANT]
> De informatie over de planning van de opgegeven capaciteit en de bijbehorende werkblad, zijn alleen bedoeld als een handleiding voor het maken van Azure Stack-planning en beslissingen voor de configuratie. Ze zijn niet bedoeld als vervanging voor uw eigen onderzoek en analyse. 

## <a name="compute-and-storage-capacity-planning"></a>COMPUTE en storage-capaciteitsplanning
Een Azure Stack-oplossing is gebouwd als een hypergeconvergeerde cluster van compute, netwerken en opslag. Dit maakt het effectieve gebruik of delen van alle hardwarecapaciteit in het cluster, aangeduid als een schaaleenheid voor Azure Stack, op een manier waarmee de beschikbaarheid en schaalbaarheid. Alle infrastructuur voor software die wordt gehost in een set virtuele machines (VM's) en deelt dezelfde fysieke servers als de tenant-VM's. Alle virtuele machines vervolgens beheerd door de schaaleenheid Windows Server-clustertechnologieën en afzonderlijke exemplaren van de Hyper-V. Deze aanpak vereenvoudigt de aanschaf en het beheer van een oplossing voor Azure Stack en biedt voor de verplaatsing en schaalbaarheid van alle services (tenant en infrastructuur) in het geheel van de schaaleenheid.

De alleen fysieke resource die nog niet te veel is ingericht in een Azure Stack-oplossing is server-geheugen. De andere resources, CPU-kernen, netwerken bandbreedte en opslagcapaciteit, zal worden overprovisioned zodat het beste gebruik van de beschikbare bronnen. Bij het berekenen van beschikbare capaciteit voor een oplossing, wordt in het geheugen van de fysieke server de belangrijkste inzenders is. Het gebruik van andere bronnen is vervolgens meer informatie over de verhouding van overmatige inrichting die mogelijk is en wat zal zijn aanvaardbaar is voor de beoogde werkbelasting.

Ongeveer 28 VM's worden gebruikt voor het hosten van Azure Stack-infrastructuur en gebruiken in totaal ongeveer 208 GB aan geheugen en 124 virtuele kernen.  De logica voor dit aantal virtuele machines is om te voldoen aan de benodigde service scheiding om te voldoen aan beveiliging, schaalbaarheid, onderhoud en patching uit handen vereisten. Deze interne service. Hierdoor kan de toekomstige introductie van nieuwe infrastructuurservices zoals ze zijn ontwikkeld.

Ter ondersteuning van de automatische update van alle fysieke server en infrastructuur voor software-onderdelen, of patch- en update-infrastructuur en gebruikers-VM wordt plaatsingen alle geheugenbronnen van de schaaleenheid niet gebruiken. Een bedrag van het totale geheugen op alle servers van een schaaleenheid is niet-toegewezen ter ondersteuning van de vereisten voor flexibiliteit van de oplossing. Bijvoorbeeld, wanneer de fysieke server Windows Server-installatiekopie wordt bijgewerkt, worden de virtuele machines die worden gehost op de server verplaatst elders in de schaaleenheid tijdens het bijwerken van Windows Server-installatiekopieën van de server. Wanneer de update voltooid is, wordt de server opnieuw opgestart en geretourneerd voor het onderhoud van workloads. Het doel voor de patch en de update van een Azure Stack-oplossing is om te voorkomen dat de noodzaak om te stoppen gehoste virtuele machines. In het streven om te voldoen aan dat doel, een minimumwaarde van geheugencapaciteit van één server is niet-toegewezen ruimte te geven voor het verplaatsen van virtuele machines binnen de schaaleenheid. Deze plaatsing en -verplaatsing is van toepassing op zowel de infrastructuur-VM's en de VM's gemaakt uit de naam van de gebruiker of de tenant van de Azure Stack-oplossing. De resultaten van de laatste uitvoering zijn zodat de hoeveelheid geheugen die is gereserveerd voor de ondersteuning van het vereiste VM-verkeer nog veel meer dan één server capaciteit vanwege plaatsing uitdagingen zijn kan wanneer virtuele machines verschillende geheugenvereisten voor hebben. Extra overhead in de categorie van het gebruik van geheugen is die van het Windows Server-exemplaar zelf. Het basisbesturingssysteem-exemplaar voor elke server verbruiken geheugen voor het besturingssysteem en de bijbehorende virtuele pagina tabellen samen met het geheugen die door Hyper-V wordt gebruikt bij het beheren van elk van de gehoste virtuele machines.

Een uitgebreidere beschrijving van de complexiteit van capaciteitsberekeningen wordt later in deze sectie wordt beschreven. In deze Introductie vindt u in de volgende voorbeelden om te helpen bij het begrijpen van de beschikbare capaciteit van uiteenlopende grootten van de oplossing. Deze zijn schattingen en als gevolg hiervan veronderstellingen over tenant VM geheugen gebruiken die mogelijk niet de waarde true voor productie-installaties. Voor deze tabel is wordt de standaard D2 Azure VM-grootte gebruikt. Standard D2 virtuele machines in Azure worden gedefinieerd met 2 virtuele CPU's en 7 GB geheugen.

|     |Per servercapaciteit|| Schaal eenheid capaciteit|  |  |||
|-----|-----|-----|-----|-----|-----|-----|-----|
|     | Geheugen | CPU Cores | Aantal servers | Geheugen | CPU Cores | Tenant-VM's<sup>1</sup>     | Per kern<sup>2</sup>    |
|Voorbeeld 1|256 GB|28|4|1024 GB| 112 | 54 |4:3|
|Voorbeeld 2|512 GB|28|4|2024 GB|112|144|4:1|
|Voorbeeld 3|384 GB|28|12|4608 GB|336|432|3:1|
|     |     |     |     |     |     |     |     |

> <sup>1</sup> standard D2 virtuele machines.
> 
> <sup>2</sup> virtuele kern fysieke kernverhouding.

Zoals eerder vermeld, wordt de VM-capaciteit bepaald door het beschikbare geheugen. De virtuele-cores op fysieke / core-verhouding voorbeeld zijn de VM-dichtheid heeft invloed op beschikbare CPU-capaciteit, tenzij de oplossing is gebouwd met een groter aantal fysieke kernen (een andere CPU is gekozen). Hetzelfde geldt voor opslag en de opslagcapaciteit van de cache.

De bovenstaande voorbeelden van VM-dichtheid zijn alleen ter informatie. Er is meer complexiteit in de berekeningen in de ondersteuning. Contact met Microsoft of een oplossingspartner is vereist om verder te begrijpen keuzes voor capaciteitsplanning en de resulterende, beschikbare capaciteit.

De rest van deze sectie beschrijft de vereisten voor Azure Stack-implementatie voor berekeningen en opslag. Gebruik deze informatie te krijgen van een base inzicht in welke onderdelen zijn vereist en hun waarden minimale configuratie. Aanvullende informatie is ook beschikbaar om te beschrijven hoe de oplossing is geconfigureerd met betrekking tot de beschikbare capaciteit en potentiële limieten van het systeem met betrekking tot de capaciteit en prestaties mogelijkheid tenant.

> [!NOTE]
> De planning capaciteitsvereisten voor netwerken zijn minimaal omdat alleen de grootte van het openbare VIP-adres kan worden geconfigureerd. Zie voor meer informatie over het toevoegen van meer openbare IP-adressen met Azure Stack [openbare IP-adressen toevoegen](azure-stack-add-ips.md).


## <a name="next-steps"></a>Volgende stappen
[COMPUTE-capaciteitsplanning](capacity-planning-compute.md)
