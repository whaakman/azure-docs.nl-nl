---
title: Opslag voor capaciteitsplanning voor Azure Stack | Microsoft Docs
description: Meer informatie over opslag voor capaciteitsplanning voor Azure Stack-implementaties.
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
ms.lastreviewed: 09/18/2018
ms.openlocfilehash: 5d9d01a482483d030569a4dcad03c9ecef7cffc0
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245147"
---
# <a name="azure-stack-storage-capacity-planning"></a>Azure Stack opslag-capaciteitsplanning
De volgende secties bieden Azure Stack opslagcapaciteit informatie over de planning om u te helpen bij het plannen voor de opslagbehoeften van de oplossingen.

## <a name="uses-and-organization-of-storage-capacity"></a>Gebruik en de organisatie van de opslagcapaciteit
Dankzij de hypergeconvergeerde configuratie van Azure Stack kunnen fysieke opslagapparaten worden gedeeld. De drie belangrijkste afdelingen van de beschikbare opslag zijn tussen de infrastructuur, de tijdelijke opslag van de virtuele machines van tenants en de opslag-blobs, tabellen en wachtrijen van de services van Azure Consistent opslag (ACS) een back-up.

## <a name="spaces-direct-cache-and-capacity-tiers"></a>Opslagruimten directe Cache en Capaciteitslagen
Er is voor de opslagcapaciteit die wordt gebruikt voor het besturingssysteem, lokale logboekregistratie, dumpbestanden voor foutopsporing en andere infrastructuur voor tijdelijke opslag behoeften. Deze capaciteit voor lokale opslag is (apparaten en -capaciteit) te scheiden van de opslagapparaten onder beheer van de configuratie van opslagruimten Direct gebracht. De rest van de opslagapparaten is in één groep van opslagcapaciteit, ongeacht het aantal servers in de schaaleenheid geplaatst. Deze apparaten zijn twee typen: Cache en capaciteit.  De Cache-apparaten zijn alleen die-Cache. Opslagruimten Direct gebruiken van deze apparaten voor write-back en opslaan in cache lezen is. De capaciteit van deze Cache-apparaten, terwijl gebruikt, worden niet doorgegeven aan opgemaakte 'zichtbaar' capaciteit van de geformatteerde virtuele schijven. De capaciteitsapparaten voor dit doel worden gebruikt en de "home locatie' van de gegevens die worden beheerd door opslagruimten te bieden.

Alle opslagcapaciteit is toegewezen en direct worden beheerd door de Azure Stack-infrastructuur. De operator hoeft te keuzes over de configuratie, toewijzing, of omgaan met opties als het gaat om de capaciteit uitbreiden. Deze ontwerpbeslissingen zijn aangebracht aan te passen aan de vereisten van de oplossing en tijdens de initiële installatie/implementatie of capaciteitsuitbreiding worden geautomatiseerd. Meer informatie over tolerantie, gereserveerde capaciteit voor opnieuw op te bouwen en andere details zijn beschouwd als onderdeel van het ontwerp. 

Operators kunnen kiezen tussen een flash-alle of een hybride opslag-configuratie:

![Capaciteitsplanning voor Azure-opslag](media/azure-stack-capacity-planning/storage.png)

In de configuratie van alle flash, wordt in de cache NVMe is met een keuze uit SATA SSD of NVMe voor capaciteit. De cache is een keuze tussen NVMe en SATA SSD in de configuratie van de hybride, terwijl de capaciteit harde schijf is.

Een korte samenvatting van de opslagruimten Direct en Azure Stack-opslagconfiguratie is als volgt:
- Opslagruimten van één opslaggroep per schaaleenheid (alle opslagapparaten in één groep zijn geconfigureerd)
- Virtuele schijven worden gemaakt als een mirror in drie kopiëren voor optimale prestaties en tolerantie
- Elke virtuele schijf wordt opgemaakt als een ReFS-bestandssysteem
- Capaciteit van de virtuele schijf wordt berekend en is toegewezen op een manier dat een capaciteit van het apparaat en de hoeveelheid capaciteit voor niet-toegewezen in de groep. Dit is het equivalent van één schijf capaciteit per server.
- Elke ReFS-bestandssysteem hebben BitLocker is ingeschakeld voor de versleuteling van data-at-rest. 

De virtuele schijven automatisch gemaakt en de bijbehorende capaciteit zijn als volgt:




|Name|Capaciteit berekening|Beschrijving|
|-----|-----|-----|
|Lokale/opstartapparaat|Minimaal 340 GB<sup>1</sup>|Afzonderlijke serveropslagruimte voor installatiekopieën van besturingssystemen en "local" infrastructuur-VM 's|
|Infrastructuur|3,5 TB|Gebruik voor alle Azure Stack-infrastructuur|
|VmTemp|Zie hieronder<sup>2</sup>|Virtuele machines van tenants hebben een tijdelijke schijf die is gekoppeld en dat gegevens worden opgeslagen in deze virtuele schijven|
|ACS|Zie hieronder <sup>3</sup>|Azure consistente opslagcapaciteit voor het onderhoud van blobs, tabellen en wachtrijen|

<sup>1</sup> minimaal voor de opslagcapaciteit van de partner van de oplossing Azure Stack is vereist.

<sup>2</sup> de grootte van de virtuele schijf die wordt gebruikt voor de Tenant virtuele Machine tijdelijke schijven berekend als een verhouding van het fysieke geheugen van de server. Zoals beschreven in de onderstaande tabellen voor de Azure IaaS VM-grootten, is de tijdelijke schijf een verhouding van het fysieke geheugen toegewezen aan de virtuele machine. De toewijzing gedaan voor de opslag van '-tijdelijke schijf' in Azure Stack wordt uitgevoerd op een manier over het vastleggen van de meeste van use cases, maar misschien niet mogelijk om te voldoen aan alle tijdelijke schijf opslagbehoeften. De verhouding gekozen is een compromis tussen de tijdelijke opslag beschikbaar maken tijdens het grootste deel van de opslagcapaciteit van de oplossing voor alleen tijdelijke schijfcapaciteit niet gebruiken. Een tijdelijke opslagschijf per server in de schaaleenheid is gemaakt. De capaciteit van de tijdelijke opslag wordt niet groter wordt dan 10% van de totale beschikbare opslagcapaciteit in de opslaggroep van de schaaleenheid. De berekening is iets als het volgende voorbeeld:

```
  DesiredTempStoragePerServer = PhysicalMemory * 0.65 * 8
  TempStoragePerSolution = DesiredTempStoragePerServer * NumberOfServers
  PercentOfTotalCapacity = TempStoragePerSolution / TotalAvailableCapacity
  If (PercentOfTotalCapacity <= 0.1)
      TempVirtualDiskSize = DesiredTempStoragePerServer
  Else
      TempVirtualDiskSize = (TotalAvailableCapacity * 0.1) / NumberOfServers
```

<sup>3</sup> de virtuele schijven gemaakt voor gebruik door ACS zijn een eenvoudige afdeling van de resterende capaciteit. Zoals opgemerkt, alle virtuele schijven worden een mirror in drie richtingen en aan capaciteit voor elke server een capaciteit van het station is niet-toegewezen. De verschillende virtuele-schijven geïnventariseerd hierboven eerst worden toegewezen en de resterende capaciteit wordt vervolgens gebruikt voor de ACS virtuele-schijven.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de Azure Stack-werkblad voor capaciteitsplanning](capacity-planning-spreadsheet.md)
