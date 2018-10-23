---
title: bestand opnemen
description: bestand opnemen
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 10/05/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 9246dea7fa12e5ac9378203e96352e917679525b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49312565"
---
### <a name="general-requirements"></a>Algemene vereisten

* Het VNet moet in hetzelfde abonnement en dezelfde regio voorkomen als het Batch-account dat u gebruikt om de pool te maken.

* De pool die van het VNet gebruikmaakt, kan een maximum van 4096 knooppunten bevatten.

* Het subnet dat is opgegeven voor de pool moet voldoende vrije IP-adressen hebben voor het aantal virtuele machines voor de pool, ofwel de som van de `targetDedicatedNodes`- en `targetLowPriorityNodes`-eigenschappen van de pool. Als het subnet onvoldoende vrije IP-adressen heeft, wijst de pool de rekenknooppunten gedeeltelijk toe en wordt een fout weergegeven voor het aanpassen van de grootte. 

* Uw Azure Storage-eindpunt moet worden omgezet door alle aangepaste DNS-servers die u beschikbaar maakt voor uw VNet. Vooral URL's met de indeling `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` en `<account>.blob.core.windows.net` moeten kunnen worden omgezet. 

Aanvullende vereisten voor VNet verschillen, afhankelijk van of de Batch-pool zich in de configuratie van de virtuele machine bevindt of in de configuratie van Cloud Services. Voor nieuwe implementaties van pools in een VNet wordt de configuratie van de virtuele machine aanbevolen.

### <a name="pools-in-the-virtual-machine-configuration"></a>Pools in de configuratie van virtuele machines

**Ondersteunde VNets**: alleen op Azure Resource Manager gebaseerde VNets

**Subnet-ID**: bij het opgeven van het subnet met behulp van de Batch-API's moet u de *resource-id* van het subnet gebruiken. De subnet-id het formulier heeft de vorm van:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.Network/virtualNetworks/{network}/subnets/{subnet}
  ```

**Machtigingen**: controleer of uw beveiligingsbeleid of de vergrendelingen die voor het VNet-abonnement of de resourcegroep gelden, een gebruikersmachtiging beperkt om het VNet te beheren.

**Aanvullende netwerkresources**: Batch kent automatisch extra netwerkresources toe aan de resourcegroep met het VNet. Voor elke 50 toegewezen knooppunten (of elke 20 knooppunten met een lage prioriteit) kent Batch het volgende toe: 1 netwerkbeveiligingsgroep, 1 openbaar IP-adres en 1 load balancer. De beperkingen die voor deze resources gelden, worden bepaald door de [resourcequota](../articles/azure-subscription-service-limits.md) van het abonnement. Voor grote pools moet u mogelijk een verhoging van het quotum aanvragen voor een of meer van deze resources.

#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Het subnet moet toestaan dat met binnenkomende communicatie van de Batch-service taken op de rekenknooppunten kunnen worden gepland, en toestaan dat uitgaande communicatie mogelijk is met Azure Storage of andere resources. Voor pools in de configuratie van de virtuele machine, voegt Batch netwerkbeveiligingsgroepen toe op het niveau van netwerkinterfaces die zijn gekoppeld aan virtuele machines. Met deze netwerkbeveiligingsgroepen worden automatisch binnenkomende en uitgaande regels geconfigureerd om het volgende verkeer toe te staan:

* Binnenkomend TCP-verkeer op de poorten 29876 en 29877 van IP-adressen van Batch-servicerollen. 
* Binnenkomend TCP-verkeer op poort 22 (Linux-knooppunten) of poort 3389 (Windows-knooppunten) om externe toegang te verlenen.
* Uitgaand verkeer op een willekeurige poort naar het virtuele netwerk.
* Uitgaand verkeer op een willekeurige poort naar internet.

> [!IMPORTANT]
> Wees voorzichtig als u binnenkomende of uitgaande regels toevoegt of wijzigt in netwerkbeveiligingsgroepen die door Batch zijn geconfigureerd. Als communicatie met de rekenknooppunten in het opgegeven subnet wordt geweigerd door een netwerkbeveiligingsgroep, zet de Batch-service de status van de rekenknooppunten op **Onbruikbaar**.

U hoeft netwerkbeveiligingsgroepen niet op te geven op subnetniveau omdat Batch zijn eigen netwerkbeveiligingsgroepen configureert. Als echter aan het opgegeven subnet netwerkbeveiligingsgroepen en/of een firewall zijn gekoppeld, configureert u de binnenkomende en uitgaande beveiligingsregels zoals wordt weergegeven in de volgende tabellen. Configureer binnenkomend verkeer op poort 3389 (Windows) of 22 (Linux) alleen als u externe toegang tot de groep virtuele machines moet toestaan. Het is niet vereist dat de VM’s in de pool kunnen worden gebruikt.

**Inkomende beveiligingsregels**

| IP-adressen van bron | Bronpoorten | Doel | Doelpoorten | Protocol | Bewerking |
| --- | --- | --- | --- | --- | --- |
Alle <br /><br />Hoewel dit feitelijk 'alles toestaan' vereist, past de Batch-service op elke VM die is gemaakt op basis van de virtuele-machineconfiguratie, een netwerkbeveiligingsgroep toe op het niveau van de netwerkinterface. Deze netwerkbeveiligingsgroep filtert alle IP-adressen van niet-Batch-services uit. | * | Alle | 29876-29877 | TCP | Toestaan |
| Gebruikerscomputers die voor foutopsporing worden gebruikt, zodat toegang op afstand tot de virtuele machine in de pool mogelijk is. | * | Alle |  3389 (Windows), 22 (Linux) | TCP | Toestaan |

**Uitgaande beveiligingsregels**

| Bron | Bronpoorten | Doel | Doelservicetag | Protocol | Bewerking |
| --- | --- | --- | --- | --- | --- |
| Alle | 443 | [Servicetag](../articles/virtual-network/security-overview.md#service-tags) | Opslag (in dezelfde regio als uw Batch-account en VNet)  | Alle | Toestaan |

### <a name="pools-in-the-cloud-services-configuration"></a>Pools die zijn gemaakt in de Cloud Services-configuratie

**Ondersteunde VNets**: klassieke VNets

**Subnet-ID**: bij het opgeven van het subnet met behulp van de Batch-API's moet u de *resource-id* van het subnet gebruiken. De subnet-id heeft de vorm van:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.ClassicVirtualNetwork /virtualNetworks/{network}/subnets/{subnet}
  ```

**Machtigingen**: de `MicrosoftAzureBatch`-service-principal moet de toegangsbeheerrol `Classic Virtual Machine Contributor` hebben voor het betreffende VNet.

#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Het subnet moet toestaan dat met binnenkomende communicatie van de Batch-service taken op de rekenknooppunten kunnen worden gepland, en toestaan dat uitgaande communicatie mogelijk is met Azure Storage of andere resources.

U hoeft geen netwerkbeveiligingsgroep op te geven omdat Batch binnenkomende communicatie alleen configureert van Batch IP-adressen naar de poolknooppunten. Als echter aan het opgegeven subnet netwerkbeveiligingsgroepen en/of een firewall zijn gekoppeld, configureert u de binnenkomende en uitgaande beveiligingsregels zoals wordt weergegeven in de volgende tabellen. Als communicatie met de rekenknooppunten in het opgegeven subnet wordt geweigerd door een netwerkbeveiligingsgroep, zet de Batch-service de status van de rekenknooppunten op **Onbruikbaar**.

 Configureer binnenkomend verkeer op poort 3389 (Windows) of 22 (Linux) alleen als u externe toegang tot poolknooppunten wilt toestaan. Het is niet vereist dat de poolknooppunten kunnen worden gebruikt.

**Inkomende beveiligingsregels**

| IP-adressen van bron | Bronpoorten | Doel | Doelpoorten | Protocol | Bewerking |
| --- | --- | --- | --- | --- | --- |
Alle <br /><br />Hoewel dit in feite 'alles toestaan' vereist, past de Batch-service een regel voor toegangsbeheerlijsten toe op het niveau van elk knooppunt, die ervoor zorgt dat alle IP-adressen van niet-Batch-services worden uitgefilterd. | * | Alle | 10100, 20100, 30100 | TCP | Toestaan |
| Gebruikerscomputers die voor foutopsporing worden gebruikt, zodat toegang op afstand tot de virtuele machine in de pool mogelijk is. | * | Alle |  3389 (Windows), 22 (Linux) | TCP | Toestaan |

**Uitgaande beveiligingsregels**

| Bron | Bronpoorten | Doel | Doelpoorten | Protocol | Bewerking |
| --- | --- | --- | --- | --- | --- |
| Alle | * | Alle | 443  | Alle | Toestaan |