---
title: Failover in Site Recovery | Microsoft Docs
description: "Azure Site Recovery coördineert de replicatie, failovers en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of een secundair datacenter."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/25/2017
ms.author: pratshar
ms.openlocfilehash: 160457fdad57cd947077aeb3a4ed85fd2a2849d8
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="failover-in-site-recovery"></a>Failover in Site Recovery
Dit artikel wordt beschreven hoe met failover-virtuele machines en fysieke servers beveiligd door Site Recovery.

## <a name="prerequisites"></a>Vereisten
1. Voordat u een failover doet, voert u een [testfailover](site-recovery-test-failover-to-azure.md) om ervoor te zorgen dat alles werkt zoals verwacht.
1. [Voorbereiden van het netwerk](site-recovery-network-design.md) op doellocatie voordat u een failover.  

Gebruik de volgende tabel om te weten over de beschikbare opties failover door Azure Site Recovery. Deze opties worden ook vermeld voor verschillende failover-scenario's.

| Scenario | Vereisten voor toepassingsimplementatie herstel | Werkstroom voor Hyper-V | Werkstroom voor VMware
|---|--|--|--|
|Geplande failover vanwege een toekomstige datacenter uitvaltijd| Geen gegevensverlies voor de toepassing als een geplande activiteit wordt uitgevoerd| Voor Hyper-V repliceert ASR gegevens met een kopieerfrequentie is opgegeven door de gebruiker. Geplande Failover wordt gebruikt voor het overschrijven van de frequentie en het definitieve wijzigingen worden gerepliceerd voordat een failover wordt gestart. <br/> <br/> 1.    Een onderhoudsvenster aan de hand van uw bedrijf wijzigingsbeheerproces plan. <br/><br/> 2. Waarschuw gebruikers toekomstige uitvaltijd. <br/><br/> 3. De gebruiker gerichte toepassing offline halen.<br/><br/>4. Klik start een geplande Failover via de portal ASR. De on-premises virtuele machine wordt automatisch afgesloten.<br/><br/>Verlies van toepassingsgegevens effectieve = 0 <br/><br/>Logboek van herstelpunten is ook beschikbaar in een bewaarperiode voor een gebruiker die wil gebruiken van een oudere herstelpunt. (24 uur bewaren voor Hyper-V).| Voor VMware repliceert ASR gegevens voortdurend met behulp van CDP. Failover geeft de gebruiker de optie voor failover naar de meest recente gegevens (inclusief post toepassing afsluiten)<br/><br/> 1. Een onderhoudsvenster aan de hand van het wijzigingsbeheerproces plannen <br/><br/>2. Waarschuw gebruikers geplande uitvaltijd <br/><br/>3.  De gebruiker gerichte toepassing offline halen. <br/><br/>4.  Start een geplande Failover met ASR portal het laatste herstelpunt nadat de toepassing offline is. Gebruik de optie 'Niet-geplande Failover' op de portal en selecteert u het laatste herstelpunt failover. De on-premises virtuele machine wordt automatisch afgesloten.<br/><br/>Verlies van toepassingsgegevens effectieve = 0 <br/><br/>Logboek van herstelpunten in een bewaarperiode is opgegeven voor een klant die wil gebruiken van een oudere herstelpunt. (72 uur retentieperiode voor VMware).
|Failover vanwege een niet-geplande datacenter uitvaltijd (natuurlijke of IT-noodherstel) | Minimaal gegevensverlies voor de toepassing | 1. starten van de organisatie BCP plannen <br/><br/>2. Niet-geplande Failover met ASR portal naar de meest recente of een punt in het venster bewaren (journaal) starten.| 1. Starten van de organisatie BCP plan. <br/><br/>2.  Niet-geplande Failover met ASR portal naar de meest recente of een punt in het venster bewaren (journaal) starten.


## <a name="run-a-failover"></a>Een failover uitvoeren
Deze procedure wordt beschreven hoe u een failover voor een [herstelplan](site-recovery-create-recovery-plans.md). U kunt ook uitvoeren de failover voor een enkele virtuele machine of fysieke server van de **gerepliceerde items** pagina


![Failover](./media/site-recovery-failover/Failover.png)

1. Selecteer **herstelplannen** > *recoveryplan_name*. Klik op **Failover**
2. Op de **Failover** Schakel in het scherm een **herstelpunt** failover. U kunt een van de volgende opties gebruiken:
    1.  **Meest recente** (standaard): deze optie om de taak starten door eerst alle gegevens die is verzonden naar Site Recovery-service te verwerken. Verwerken van de gegevens, maakt een herstelpunt voor elke virtuele machine. Dit herstelpunt wordt gebruikt door de virtuele machine tijdens de failover. Deze optie biedt de laagste RPO (beoogd herstelpunt) als de virtuele machine gemaakt nadat de failover beschikt over alle gegevens die is gerepliceerd naar de Site Recovery-service als de failover is geactiveerd.
    1.  **Meest recente verwerkte**: deze optie is overgenomen van alle virtuele machines van het herstelplan bij om de meest recente herstelpunt dat al is verwerkt door Site Recovery-service. Wanneer u een testfailover uitgevoerd van een virtuele machine uitvoert, wordt ook de tijdstempel van de meest recente verwerkte herstelpunten weergegeven. Als u failover van een herstelplan doet, kunt u gaat u naar afzonderlijke virtuele machine en bekijk **herstelpunten van de meest recente** tegel voor deze informatie. Er is geen tijd is besteed voor het verwerken van de onbewerkte gegevens, zorgt deze optie voor een laag RTO (beoogde hersteltijd) failover-optie.
    1.  **Meest recente app-consistente**: deze optie is overgenomen van alle virtuele machines van het herstelplan bij om de meest recente toepassing consistente herstelpunten die al is verwerkt door Site Recovery-service. Wanneer u een testfailover uitgevoerd van een virtuele machine uitvoert, wordt ook de tijdstempel van de meest recente toepassingsconsistente herstelpunten weergegeven. Als u failover van een herstelplan doet, kunt u gaat u naar afzonderlijke virtuele machine en bekijk **herstelpunten van de meest recente** tegel voor deze informatie.
    1.  **Meest recente multi-VM verwerkt**: deze optie is alleen beschikbaar voor herstelplannen die ten minste één virtuele machine met de consistentie tussen meerdere VM's op. Virtuele machines die deel van een failover van de groep replicatie naar de laatste algemene multi-VM consistent herstelpunt uitmaken verwijzen. Andere virtuele machines failover naar de meest recente verwerkte herstelpunt.  
    1.  **Meest recente multi-VM-app-consistente**: deze optie is alleen beschikbaar voor herstelplannen die ten minste één virtuele machine met meerdere VM consistentie ON hebben. Virtuele machines die deel van een failover van de groep replicatie naar de meest recente algemene multi-VM toepassingsconsistent herstelpunt uitmaken verwijzen. Andere virtuele machines failover naar de meest recente toepassingsconsistente herstelpunt.
    1.  **Aangepaste**: als u een testfailover uitgevoerd van een virtuele machine uitvoert, dan kunt u deze optie om failover naar een bepaald herstelpunt.

    > [!NOTE]
    > De optie voor het kiezen van een herstelpunt is alleen beschikbaar wanneer u zijn failover wordt uitgevoerd naar Azure.
    >
    >


1. Als sommige van de virtuele machines in het herstelplan zijn failover in een eerder uitgevoerde en nu de virtuele machines actief op de locatie van de bron- en doelserver zijn, kunt u **richting wijzigen** optie om te bepalen in welke richting de failover moet gebeuren.
1. Als u bent failover wordt uitgevoerd naar Azure en gegevensversleuteling is ingeschakeld voor de cloud (geldt alleen wanneer u Hyper-v virtuele machines van een VMM-Server hebt beveiligd), in **versleutelingssleutel** selecteert u het certificaat dat was uitgegeven wanneer u ingeschakeld versleuteling van gegevens tijdens de installatie op de VMM-server.
1. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor probeert virtuele bronmachines afgesloten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt.  

    > [!NOTE]
    > Als Hyper-v virtuele machines worden beschermd, wordt de optie voor het afsluiten ook probeert te synchroniseren van de on-premises gegevens die nog niet is verzonden naar de service voordat de failover.
    >
    >

1. U kunt de voortgang van de failover volgen op de **taken** pagina. Zelfs als er fouten optreden tijdens een niet-geplande failover het herstelplan wordt uitgevoerd totdat deze voltooid is.
1. Na de failover valideren op de virtuele machine aanmelden bij deze. Als u wilt gaan, een ander herstelpunt voor de virtuele machine, dan kunt u **herstelpunt wijzigen** optie.
1. Wanneer u tevreden met de mislukte bent via de virtuele machine, kunt u **doorvoeren** de failover. Commit verwijdert alle herstelpunten die beschikbaar zijn met de service en **herstelpunt wijzigen** optie niet langer beschikbaar.

## <a name="planned-failover"></a>Geplande failover
Virtuele machines of fysieke servers die zijn beveiligd met Site Recovery ook ondersteuning **geplande failover**. Geplande failover is een nul gegevens verloren gaan failover-optie. Wanneer een geplande failover wordt geactiveerd, eerst de virtuele bronmachines afgesloten, de meest recente gegevens worden gesynchroniseerd en vervolgens een failover wordt geactiveerd.

> [!NOTE]
> Wanneer u een failover-Hyper-v virtuele machines van een on-premises site naar een andere lokale site naar keert u terug naar de primaire lokale site hebt u in eerste **omgekeerd repliceren** de virtuele machine weer naar primaire site en vervolgens Hiermee activeert u een failover. Als de primaire virtuele machine niet beschikbaar is, voordat begint met het **omgekeerd repliceren** hebt u de virtuele machine uit een back-up hersteld.   
>
>

## <a name="failover-job"></a>Failover-taak

![Failover](./media/site-recovery-failover/FailoverJob.png)

Wanneer een failover wordt geactiveerd, omvat het volgende stappen uit:

1. Controle van vereisten: deze stap zorgt ervoor dat alle voorwaarden die vereist is voor failover wordt voldaan
1. Failover: Deze stap de gegevens worden verwerkt en maakt het klaar is zodat Azure een virtuele machine buiten deze kunnen worden gemaakt. Als u hebt gekozen **nieuwste** herstelpunt, deze stap maakt een herstelpunt wordt gemaakt van de gegevens die is verzonden naar de service.
1. Start: Deze stap maakt u een virtuele machine van Azure met behulp van de gegevens die worden verwerkt in de vorige stap.

> [!WARNING]
> **Een onderhanden niet annuleren failover**: voordat failover wordt gestart, replicatie voor de virtuele machine is gestopt. Als u **annuleren** een in voortgang van de taak, de failover stopt, maar de virtuele machine niet wordt gestart om te repliceren. Replicatie kan niet opnieuw worden gestart.
>
>

## <a name="time-taken-for-failover-to-azure"></a>Gebruikte tijd voor failover naar Azure

Failover van virtuele machines vereist in bepaalde gevallen, een extra tussenstap die meestal ongeveer 8 tot en met 10 minuten duurt om te voltooien. In de volgende gevallen is is de tijd op failover hoger dan normaal:

* Met behulp van de mobility-service van de versie die ouder zijn dan 9,8 virtuele VMware-machines
* Fysieke servers 
* Virtuele VMware-Linux-machines
* Hyper-V virtuele machines die beveiligd als de fysieke servers
* Waar volgende stuurprogramma's niet aanwezig als opstartstuurprogramma zijn virtuele VMware-machines 
    * storvsc 
    * VMBus 
    * storflt 
    * Intelide 
    * ATAPI
* Virtuele VMware-machines waarvoor geen DHCP-service is ingeschakeld ongeacht of ze zijn geïnstalleerd via DHCP of statische IP-adressen

In alle andere gevallen deze tussenstap is niet vereist en de tijd voor de failover is aanzienlijk minder. 





## <a name="using-scripts-in-failover"></a>Met behulp van scripts in Failover
Mogelijk wilt bepaalde acties automatiseren, terwijl u een failover uitvoert. U kunt scripts gebruiken of [Azure automation-runbooks](site-recovery-runbook-automation.md) in [herstelplannen](site-recovery-create-recovery-plans.md) dat doet.

## <a name="other-considerations"></a>Andere overwegingen
* **Stationsletter** : voor het bewaren van de stationsletter op virtuele machines na een failover kunt u instellen de **SAN-beleid** voor de virtuele machine **OnlineAll**. [Meer informatie](https://support.microsoft.com/en-us/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).



## <a name="next-steps"></a>Volgende stappen

> [!WARNING]
> Als u failover uitgevoerd voor virtuele machines en de on-premises Datacenter beschikbaar is, moet u [ **beveiligt** ](site-recovery-how-to-reprotect.md) virtuele VMware-machines terug naar de on-premises Datacenter.

Gebruik [ **geplande failover** ](site-recovery-failback-from-azure-to-hyper-v.md) optie naar **Failback** Hyper-v virtuele machines naar on-premises van Azure.

Als is mislukt via een Hyper-v virtuele machine naar een andere on-premises datacentrum worden beheerd door een VMM-server en het primaire Datacenter beschikbaar is, gebruikt u vervolgens **omgekeerde repliceren** optie voor het starten van de replicatie terug naar de primaire gegevens Center.
