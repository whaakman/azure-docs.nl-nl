---
title: Versiegeschiedenis van Azure Site Recovery Deployment Planner
description: Verschillende versies van Site Recovery Deployment Planner voor problemen en bekende beperkingen samen met de release-datums bekend.
services: site-recovery
author: Daya-Patil
manager: carmonm
ms.topic: article
ms.service: site-recovery
ms.date: 04/24/2019
ms.author: dapatil
ms.openlocfilehash: 2edf7ce3be1402a497ceab5b826a89ee43c5c39b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927370"
---
# <a name="azure-site-recovery-deployment-planner-version-history"></a>Versiegeschiedenis van Azure Site Recovery Deployment Planner

Dit artikel bevat een geschiedenis van alle versies van Azure Site Recovery Deployment Planner samen met de oplossingen, bekende beperkingen in elk en hun datums release.

## <a name="version-24"></a>Versie 2.4

**Uitgebracht op: 17 april 2019**

**Opgelost:**

- Verbeterde operationele compatibiliteit, specifiek bij het verwerken van fouten op basis van lokalisatie.
- Extra virtuele machines met maximaal 20 Mbps van gegevens wijzigt tarief (verloop) in de controlelijst voor de compatibiliteit.
- Verbeterde foutberichten
  - Ondersteuning toegevoegd voor vCenter 6.7.
  - Ondersteuning toegevoegd voor Windows Server 2019 en Red Hat Enterprise Linux (RHEL) werkstation.



## <a name="version-23"></a>Versie 2.3

**Uitgebracht op: 3 december 2018**

**Opgelost:**

- Een probleem waardoor de Deployment Planner van het genereren van een rapport met het opgegeven doellocatie en het abonnement is opgelost.

## <a name="version-22"></a>Versie 2.2 

**Uitgebracht op: 25 april 2018**

**Opgelost:**

- GetVMList-bewerkingen:
  - Er is een probleem waardoor GetVMList mislukken als de opgegeven map bestaat niet opgelost. Deze nu de standaard-map wordt gemaakt, of maakt u de map die is opgegeven in de parameter uitvoerbestand.
  - Toegevoegd op dat meer gedetailleerde oorzaken voor het mislukken van GetVMList.
- Type-informatie van de toegevoegd VM als een kolom in het werkblad met compatibele VM's van de Deployment Planner-rapport.
- Hyper-V naar Azure-noodherstel:
  - Uitgesloten virtuele machines met gedeelde VHD's en PassThrough-schijven van de profilering. De bewerking Startprofiling bevat de lijst met uitgesloten virtuele machines in de console.
  - Extra virtuele machines met meer dan 64 schijven aan de lijst met niet-compatibele VM's.
  - De initiële replicatie (IR) en de delta (DR) compressie replicatiefactor zijn bijgewerkt.
  - Beperkte ondersteuning toegevoegd voor SMB-opslag.

## <a name="version-21"></a>Versie 2.1

**Uitgebracht op: 3 januari 2018**

**Opgelost:**

- Het Excel-rapport bijgewerkt.
- Er zijn bugs opgelost in de GetThroughput-bewerking.
- Extra optie voor het beperken van het aantal virtuele machines voor het profiel of het rapport genereren. De standaardlimiet is 1000 virtuele machines.
- VMware naar Azure-noodherstel:
  - Een probleem van Windows Server 2016-VM in de tabel niet compatibel is opgelost. 
  - Compatibiliteit van berichten is bijgewerkt voor Extensible Firmware Interface (EFI) Windows-VM's.
- Bijgewerkt van VMware naar Azure en Hyper-V naar Azure VM gegevensverloop limiet per virtuele machine. 
- Verbeterde betrouwbaarheid van bestand met VM-lijst bij het parseren.

## <a name="version-201"></a>Versie 2.0.1

**Uitgebracht op: 7 december 2017**

**Opgelost:**

- Toegevoegde aanbeveling voor het optimaliseren van bandbreedte van het netwerk.

## <a name="version-20"></a>Versie 2.0

**Uitgebracht op: 28 november 2017**

**Opgelost:**

- Ondersteuning toegevoegd voor Hyper-V naar Azure-noodherstel.
- Toegevoegde kosten berekenen.
- OS-versiecontrole toegevoegd voor VMware naar Azure noodherstel om te bepalen of de virtuele machine compatibel of niet compatibel voor de beveiliging. Het hulpprogramma maakt gebruik van de OS-versie-tekenreeks die wordt geretourneerd door de vCenter-server voor die VM. De versie van de Gast-besturingssysteem is dat de gebruiker heeft geselecteerd tijdens het maken van de virtuele machine in VMware.

**Bekende beperkingen:**

- Voor Hyper-V naar Azure-noodherstel, virtuele machine met de naam die de tekens, zoals: `,`, `"`, `[`, `]`, en ``` ` ``` worden niet ondersteund. Als geprofileerd, worden het genereren van rapporten mislukt of heeft een onjuiste resultaten.
- Voor VMware naar Azure-noodherstel, wordt niet virtuele machine met de naam met door komma's ondersteund. Als geprofileerd, rapporteren genereren mislukt of heeft een onjuiste resultaten.

## <a name="version-131"></a>Versie 1.3.1

**Uitgebracht op: 19 juli 2017** 

**Opgelost:**

- Ondersteuning toegevoegd voor grote schijven (> 1 TB) in het genereren van rapporten. Nu u Deployment Planner gebruiken kunt voor het plannen van replicatie voor virtuele machines met schijfgrootten van meer dan 1 TB (maximaal 4095 GB).
Meer informatie over [ondersteuning voor grote schijven in Azure Site Recovery](https://azure.microsoft.com/blog/azure-site-recovery-large-disks/)

## <a name="version-13"></a>Versie 1.3

**Uitgebracht op: 9 mei 2017**

**Opgelost:**

- Er is ondersteuning toegevoegd voor beheerde schijf in het genereren van rapporten. Het aantal virtuele machines dat kan worden geplaatst op een enkel opslagaccount wordt berekend op basis van de beheerde schijf is geselecteerd voor Failover/Test-Failover.

## <a name="version-12"></a>versie 1.2

**Uitgebracht op: En met 7 april 2017**

**Opgelost:**

- Toegevoegde opstarten type (BIOS of EFI) controles voor elke virtuele machine om te bepalen of de virtuele machine compatibel of niet compatibel voor de beveiliging.
- Toegevoegde OS-type-informatie voor elke virtuele machine in de compatibele virtuele machines en incompatibele VM's werkbladen.
- Er is ondersteuning toegevoegd voor de GetThroughput-bewerking voor de Amerikaanse overheid en China Microsoft Azure-regio's.
- Er zijn meer controles op vereisten toegevoegd voor vCenter en ESXi Server.
- Er is een probleem van onjuist rapport gegenereerd wanneer de landinstellingen zijn ingesteld op niet-Engelse opgelost.

## <a name="version-11"></a>Versie 1.1

**Uitgebracht op: 9 maart 2017**

**Opgelost:**

- Een probleem waardoor profileren van virtuele machines wanneer er twee of meer virtuele machines met dezelfde naam of IP-adres voor verschillende ESXi-hosts van vCenter is opgelost.
- Een probleem waardoor kopiëren en zoeken worden uitgeschakeld voor de compatibele virtuele machines en incompatibele VM's werkbladen is opgelost.

## <a name="version-10"></a>Versie 1.0

**Uitgebracht op: 23 februari 2017**

**Bekende beperkingen:**

- Ondersteunt alleen voor VMware naar Azure herstel na noodgevallen. Voor Hyper-V naar Azure herstel na noodgevallen, gebruikt u de [Capaciteitsplanner voor Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
- Biedt geen ondersteuning voor de GetThroughput-bewerking voor de Amerikaanse overheid en China Microsoft Azure-regio's.
- Het hulpprogramma cann't profiel VM's als de vCenter-server twee of meer virtuele machines met dezelfde naam of IP-adres voor verschillende ESXi-hosts.
In deze versie slaat het hulpprogramma profilering over bij namen of IP-adressen van virtuele machines die dubbel voorkomen in het bestand dat is opgegeven voor VMListFile. Een tijdelijke oplossing is de virtuele machines te profileren met behulp van een ESXi-host in plaats van de vCenter-server. Zorg ervoor dat voor elke ESXi-host één exemplaar uitvoeren.
