---
title: Assessment berekeningen in Azure migreren | Microsoft Docs
description: Biedt een overzicht van assessment berekeningen in de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: b8075f0e1149a6fc5194347fc34e2a16d5eb2ffc
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="assessment-calculations"></a>Beoordelingsberekeningen

[Azure migreren](migrate-overview.md) voor lokale werkbelastingen voor migratie naar Azure evalueert. Dit artikel bevat informatie over hoe beoordelingen zijn berekend.



## <a name="overview"></a>Overzicht

Een beoordeling voor het migreren van Azure bestaat uit drie fasen. Beoordeling begint met een geschiktheid-analyse, gevolgd door formaat op basis van prestatie-schattingen en ten slotte een maandelijkse kosten voor kardinaliteitsschatting. Een machine alleen wordt verplaatst langs naar een later stadium als de vorige bewerking is geslaagd. Bijvoorbeeld, als een machine de controle van de geschiktheid van Azure mislukt, het gemarkeerd als geschikt voor Azure, en sizing en goedkoper won't worden berekend. 


## <a name="azure-suitability-analysis"></a>Azure geschiktheid analyse

Computers die u wilt migreren naar Azure moeten voldoen aan de Azure-vereisten en beperkingen. Worden bijvoorbeeld, als de schijf van een lokale virtuele machine meer dan 4 TB is, deze kan niet gehost op Azure. De Azure nalevingscontroles worden samengevat in de volgende tabel. 

**Selectievakje** | **Details**
--- | ---
**Type opstarten** | Het type opstarten van de Gast OS-schijf moet BIOS en niet UEFI.
**Kernen** | Het aantal kernen op de machines moet gelijk zijn aan (of minder dan) het maximum aantal kernen (32) ondersteund voor een virtuele machine in Azure.<br/><br/> Als prestatiegeschiedenis beschikbaar is, Azure migreren rekening gehouden met de gebruikte kernen voor vergelijking. Als een factor comfort is opgegeven in de instellingen voor de beoordeling, wordt het aantal gebruikte kernen vermenigvuldigd met de factor comfort.<br/><br/> Als er geen prestatiegeschiedenis van, de toegewezen kernen Azure migreren gebruikt zonder het toepassen van de factor comfort.
**Geheugen** | De grootte van de machine-geheugen moet gelijk zijn aan (of minder dan) de maximale hoeveelheid geheugen (448 GB) die is toegestaan voor een virtuele machine in Azure. <br/><br/> Als prestatiegeschiedenis beschikbaar is, beschouwt Azure migreren het gebruikte geheugen voor vergelijking. Als een factor comfort is opgegeven, wordt het gebruikte geheugen vermenigvuldigd met de factor comfort.<br/><br/> Als er geen geschiedenis van die het toegewezen geheugen wordt gebruikt, zonder de factor comfort toepassen.<br/><br/> 
**Windows Server 2003-2008 R2** | ondersteuning voor 32-bits en 64-bits.<br/><br/> Azure biedt best effort ondersteuning.
**Windows Server 2008 R2 met alle SP 's** | 64-bits-ondersteuning.<br/><br/> Azure biedt volledige ondersteuning.
**Windows Server 2012 & alle SP 's** | 64-bits-ondersteuning.<br/><br/> Azure biedt volledige ondersteuning.
**Windows Server 2012 R2 & alle SP 's** | 64-bits-ondersteuning.<br/><br/> Azure biedt volledige ondersteuning.
**Windows Server 2016 & alle SP 's** | 64-bits-ondersteuning.<br/><br/> Azure biedt volledige ondersteuning.
**Client voor Windows 7 en hoger** | 64-bits-ondersteuning.<br/><br/> Azure biedt ondersteuning voor Visual Studio-abonnement.
**Linux** | 64-bits-ondersteuning.<br/><br/> Azure biedt volledige ondersteuning voor deze [besturingssystemen](../virtual-machines/linux/endorsed-distros.md).
**Schijf voor opslag** | Toegewezen grootte van een schijf moet 4 TB (4096 GB) of minder.<br/><br/> Het aantal schijven die zijn gekoppeld aan de machine moet 65 of minder, inclusief de besturingssysteemschijf. 
**Netwerken** | Een machine moet 32 of minder NIC's gekoppeld.


## <a name="performance-based-sizing"></a>Formaat op basis van prestaties

Nadat een machine is gemarkeerd als geschikt voor Azure, wijst Azure migreren deze toe aan een VM-grootte in Azure, met behulp van de volgende criteria:

- **Controle van de opslag**: Azure migreren probeert te koppelen van elke schijf die is gekoppeld aan de machine naar een schijf in Azure:
    - De i/o per seconde (IOPS) vermenigvuldigen Azure migreren met de factor comfort. Deze ook veelvouden de doorvoer (in MBps) van elke schijf met de factor comfort. Dit biedt de effectieve IOPS en doorvoerlimieten schijf. Op basis hiervan wijst Azure migreren de schijf naar een standaard of premium-schijf in Azure.
      - Als de service een schijf met de vereiste IOPS & doorvoer vinden kan, wordt de computer als ongeschikt zijn gemarkeerd voor Azure.
      - Als het een set geschikte schijven gevonden, selecteert Azure migreren degene die ondersteuning bieden voor de opslag redundantie methode en de locatie die is opgegeven in de instellingen van de assessment.
      - Als er meerdere schijven voor in aanmerking komt, wordt deze met de laagste kosten geselecteerd.
- **Schijf opslagdoorvoer**: [meer](../azure-subscription-service-limits.md#storage-limits) over Azure beperkt per schijf en de VM.
- **Schijftype**: Azure migreren ondersteunt alleen beheerde schijven.
- **Controle van het netwerk**: Azure migreren gezocht naar een virtuele machine van Azure die ondersteuning voor het aantal NIC's op de on-premises machine biedt.
    - U doet dit door deze maakt een aggregatie van de gegevens verzonden per seconde (MBps) buiten de machine (netwerk) op alle NIC's en past de factor comfort aan het verzamelde aantal. Dit nummer als gebruikt voor het zoeken van een virtuele machine van Azure die ondersteuning voor de vereiste netwerkprestaties biedt.
    - Azure migreren neemt de netwerkinstellingen van de virtuele machine en wordt ervan uitgegaan dat het een netwerk buiten het datacenter.
    - Als geen prestatiegegevens netwerk beschikbaar is, wordt alleen het aantal NIC voor VM-formaat beschouwd.
- **Selectievakje COMPUTE**: nadat de vereisten voor appopslag en netwerk worden berekend, Azure migreren beschouwt compute-vereisten:
    - Als de prestatiegegevens beschikbaar voor de virtuele machine is, kijkt naar de gebruikte kernen en het geheugen en de factor comfort van toepassing is. Op basis van dit nummer, wordt geprobeerd een geschikte VM-grootte niet vinden in Azure.
    - Als er geen geschikte grootte wordt gevonden, wordt de machine gemarkeerd als niet geschikt voor Azure.
    - Als een geschikte grootte wordt gevonden, Azure migreren is van toepassing de berekeningen voor opslag en netwerken. Deze locatie en prijzen van laag-instellingen voor de laatste aanbeveling voor VM-grootte is van toepassing.


## <a name="monthly-cost-estimation"></a>Maandelijkse kosten schatting

Nadat sizing aanbevelingen voltooid zijn, berekend Azure migreren na de migratie berekenings- en kosten.

- **COMPUTE-kosten**: de API van de facturering voor het berekenen van de maandelijkse kosten voor de virtuele machine met behulp van de aanbevolen grootte van de virtuele machine in Azure, Azure migreren gebruikt. De berekening wordt het besturingssysteem, software assurance, locatie en valuta-instellingen rekening. Deze cumuleert de kosten van alle machines, voor het berekenen van de totale maandelijkse compute-kosten.
- **Opslagkosten**: de maandelijkse opslag kosten voor een machine wordt berekend door het samenvoegen van de maandelijkse kosten van alle schijven die zijn gekoppeld aan de machine. De totale maandelijkse opslagkosten berekend Azure migreren door de kosten voor opslag van alle machines samen te voegen. De berekening niet op dit moment aanbiedingen die zijn opgegeven in de instellingen voor evaluatie in aanmerking genomen.

Kosten worden weergegeven in de opgegeven in de instellingen voor beoordeling valuta. 


## <a name="next-steps"></a>Volgende stappen

[Instellen van een beoordeling voor lokale virtuele VMware-machines](tutorial-assessment-vmware.md)
