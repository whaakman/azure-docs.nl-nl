---
title: Failover tijdens het herstel na noodgeval met Azure Site Recovery | Microsoft Docs
description: Meer informatie over mislukte via virtuele machines en fysieke servers tijdens het herstel na noodgeval met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 10/28/2018
ms.author: raynew
ms.openlocfilehash: 6e16529740377b9a082fda4f3e4409b57441715e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215154"
---
# <a name="fail-over-vms-and-physical-servers"></a>Failover van virtuele machines en fysieke servers 

Dit artikel wordt beschreven hoe failover-virtuele machines en fysieke servers beveiligd door Site Recovery.

## <a name="prerequisites"></a>Vereisten
1. Voordat u een failover, voer een [testfailover](site-recovery-test-failover-to-azure.md) om ervoor te zorgen dat alles werkt zoals verwacht.
1. [Voorbereiden van het netwerk](site-recovery-network-design.md) op doellocatie voordat u een failover.  

Gebruik de volgende tabel om te weten over de opties voor failover geleverd door Azure Site Recovery. Deze opties worden ook weergegeven voor verschillende failover-scenario's.

| Scenario | Vereisten voor toepassingsimplementatie herstel | Werkstroom voor Hyper-V | Werkstroom voor VMware
|---|--|--|--|
|Geplande failover vanwege een aankomende datacenter downtime| Geen gegevens verloren gaan voor de toepassing als een geplande activiteit wordt uitgevoerd| Voor Hyper-V repliceert ASR gegevens op een kopieerfrequentie van die is opgegeven door de gebruiker. Geplande Failover wordt gebruikt voor het overschrijven van de frequentie en het definitieve wijzigingen worden gerepliceerd voordat u een failover wordt gestart. <br/> <br/> 1.    Een onderhoudsvenster aan de hand van het wijzigingsbeheerproces van uw bedrijf van plan bent. <br/><br/> 2. Waarschuw gebruikers van de geplande downtime. <br/><br/> 3. De gebruiker gerichte toepassing offline halen.<br/><br/>4. Start een geplande Failover met behulp van de ASR-portal. De on-premises virtuele machine wordt automatisch afsluiten.<br/><br/>Effectieve toepassing gegevensverlies = 0 <br/><br/>Logboek van herstelpunten is ook beschikbaar in een bewaarperiode voor een gebruiker die met behulp van een ouder herstelpunt. (24 uur bewaren voor Hyper-V).| Voor VMware repliceert ASR gegevens continu met behulp van CDP. Failover geeft de gebruiker de optie voor failover naar de meest recente gegevens (inclusief post toepassing afsluiten)<br/><br/> 1. Een onderhoudsvenster aan de hand van het wijzigingsbeheerproces plannen <br/><br/>2. meldingen verzenden naar gebruikers van de geplande downtime <br/><br/>3.    De gebruiker gerichte toepassing offline halen. <br/><br/>4.  Start een geplande Failover met behulp van ASR-portal naar het laatste punt nadat de toepassing offline is. Gebruik de optie 'Niet-geplande Failover' in de portal en selecteert u het laatste herstelpunt voor de failover. De on-premises virtuele machine wordt automatisch afsluiten.<br/><br/>Effectieve toepassing gegevensverlies = 0 <br/><br/>Logboek van herstelpunten in een bewaarperiode is opgegeven voor een klant die met behulp van een ouder herstelpunt. (72 uur retentie voor VMware).
|Failover vanwege een niet-geplande datacenter downtime (natuurlijke of IT-ramp) | Minimaal gegevensverlies voor de toepassing | 1. Start de BCP-plan van de organisatie <br/><br/>2. Start niet-geplande Failover met behulp van ASR-portal naar de meest recente of een punt van de bewaarperiode (logboek).| 1. Starten van de organisatie BCP-plan. <br/><br/>2.  Start niet-geplande Failover met behulp van ASR-portal naar de meest recente of een punt van de bewaarperiode (logboek).


## <a name="run-a-failover"></a>Een failover uitvoeren
Deze procedure wordt beschreven hoe u een failover uitvoeren voor een [herstelplan](site-recovery-create-recovery-plans.md). U kunt ook uitvoeren de failover voor een enkele virtuele machine of fysieke server uit de **gerepliceerde items** pagina zoals wordt beschreven [hier](vmware-azure-tutorial-failover-failback.md#run-a-failover-to-azure).


![Failover](./media/site-recovery-failover/Failover.png)

1. Selecteer **herstelplannen** > *recoveryplan_name*. Klik op **Failover**
2. Op de **Failover** scherm, selecteer een **herstelpunt** voor de failover. U kunt een van de volgende opties gebruiken:
    1.  **Meest recente**: deze optie om de taak starten door eerst alle gegevens die is verzonden naar Site Recovery-service verwerkt. Verwerken van de gegevens, maakt een herstelpunt voor elke virtuele machine. Dit herstelpunt wordt gebruikt door de virtuele machine tijdens de failover. Deze optie biedt het laagste RPO (Recovery Point Objective) als de virtuele machine gemaakt nadat de failover heeft alle gegevens die is gerepliceerd naar de Site Recovery-service wanneer de failover werd geactiveerd.
    1.  **Laatst verwerkte**: deze optie wordt er een failover van alle virtuele machines van het herstelplan te gaan naar de meest recente herstelpunt dat al is verwerkt door Site Recovery-service. Wanneer u test-failover van een virtuele machine uitvoert, wordt ook de tijdstempel van de meest recente verwerkte herstelpunt weergegeven. Als u failover van een herstelplan te gaan doen, kunt u deze gaat u naar de afzonderlijke virtuele machine en bekijk **laatste herstelpunten** tegel om deze informatie te verkrijgen. Als er geen tijd besteed aan voor het verwerken van de niet-verwerkte gegevens, biedt deze optie een lage RTO (Recovery Time Objective) failover-optie.
    1.  **Laatste toepassingsconsistente**: deze optie wordt er een failover van alle virtuele machines van het herstelplan te gaan naar de meest recente toepassingsconsistente herstelpunt uitgevoerd die al is verwerkt door Site Recovery-service. Wanneer u test-failover van een virtuele machine uitvoert, wordt ook de tijdstempel van de meest recente toepassingsconsistente herstelpunt weergegeven. Als u failover van een herstelplan te gaan doen, kunt u deze gaat u naar de afzonderlijke virtuele machine en bekijk **laatste herstelpunten** tegel om deze informatie te verkrijgen.
    1.  **Meest recente verwerkte multi-VM**: deze optie is alleen beschikbaar voor herstelplannen met ten minste één virtuele machine met multi-VM-consistentie op. Virtuele machines die deel van een failover van de groep replicatie naar de meest recente toepassingsconsistente herstelpunt van algemene multi-VM uitmaken verwijzen. De failover van andere virtuele machines naar hun meest recente verwerkte herstelpunt.  
    1.  **Nieuwste multi-VM-app-consistente**: deze optie is alleen beschikbaar voor het plannen voor herstel die u hebt ten minste één virtuele machine met multi-VM-consistentie op. Virtuele machines die deel van een failover van de groep replicatie naar het laatste toepassingsconsistente herstelpunt van algemene multi-VM uitmaken verwijzen. De failover van andere virtuele machines naar hun meest recente toepassingsconsistente herstelpunt.
    1.  **Aangepaste**: als u test-failover van een virtuele machine uitvoert, dan kunt u deze optie om failover naar een bepaald herstelpunt.

    > [!NOTE]
    > De optie voor het kiezen van een herstelpunt is alleen beschikbaar wanneer u bent Failover-overschakeling uitvoeren naar Azure.
    >
    >


1. Als sommige van de virtuele machines in het herstelplan failover in een vorige uitvoering en nu de virtuele machines actief op zowel bron- en locatie zijn, kunt u **richting wijzigen** optie om te bepalen in welke richting de failover moet gebeuren.
1. Als u bent Failover-overschakeling uitvoeren naar Azure en gegevensversleuteling is ingeschakeld voor de cloud (geldt alleen wanneer u Hyper-v virtuele machines van een VMM-Server hebt beveiligd), in **versleutelingssleutel** selecteert u het certificaat dat is uitgegeven als u ingeschakeld versleuteling van gegevens tijdens de installatie op de VMM-server.
1. Selecteer **afsluiten machine voordat u begint met failover** als u wilt dat Site Recovery probeert virtuele bronmachines afgesloten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als afsluiten is mislukt.  

    > [!NOTE]
    > Als Hyper-v virtuele machines worden beschermd, wordt de optie voor afsluiten ook probeert te synchroniseren van de on-premises gegevens die nog niet is verzonden naar de service voordat de failover wordt geactiveerd.
    >
    >

1. U kunt de voortgang van de failover volgen op de pagina **Taken**. Zelfs als er fouten zijn opgetreden tijdens een niet-geplande failover, het herstelplan wordt uitgevoerd totdat deze bewerking voltooid is.
1. Valideer de virtuele machine door logboekregistratie in op deze na de failover. Als u wilt overschakelen naar een ander herstelpunt van de virtuele machine, dan kunt u **herstelpunt wijzigen** optie.
1. Wanneer u tevreden bent met de virtuele machine waarvoor u de failover hebt uitgevoerd, kunt u de failover **Doorvoeren**. **Doorvoeren Hiermee verwijdert u alle herstelpunten die beschikbaar zijn met de service** en **herstelpunt wijzigen** optie is niet meer beschikbaar.

## <a name="planned-failover"></a>Geplande failover
Virtuele machines/fysieke servers die zijn beveiligd met Site Recovery ook ondersteuning voor **geplande failover**. Geplande failover is een nul gegevens verloren gaan failover-optie. Wanneer een geplande failover wordt geactiveerd, eerst de virtuele bronmachines zijn afsluiten, de meest recente gegevens worden gesynchroniseerd en vervolgens een failover wordt geactiveerd.

> [!NOTE]
> Tijdens de failover van Hyper-v virtuele machines van één on-premises site naar een andere on-premises site, keert u terug naar de primaire on-premises site u hebt eerst **reverse-replicatie** de virtuele machine terug naar de primaire site en vervolgens een failover wordt geactiveerd. Als de primaire virtuele machine niet beschikbaar is, voordat vanaf naar **reverse-replicatie** u hebt voor het herstellen van de virtuele machine vanuit een back-up.   
>
>
## <a name="failover-job"></a>Failover-taak

![Failover](./media/site-recovery-failover/FailoverJob.png)

Wanneer een failover wordt geactiveerd, moet deze stappen te volgen:

1. Controle van vereisten: deze stap zorgt ervoor dat alle vereiste voorwaarden voor failover wordt voldaan
1. Failover: Deze stap verwerkt de gegevens en maakt het gereed is zodat een virtuele machine van Azure buiten deze kan worden gemaakt. Als u hebt gekozen **nieuwste** herstelpunt, deze stap maakt een herstelpunt van de gegevens die zijn verzonden naar de service.
1. Start: Deze stap maakt u een virtuele machine van Azure met behulp van de gegevens verwerkt in de vorige stap.

> [!WARNING]
> **Een wordt uitgevoerd niet annuleren failover**: voordat de failover wordt gestart, replicatie voor de virtuele machine is gestopt. Als u **annuleren** een in de taak wordt uitgevoerd, de failover gestopt, maar de virtuele machine niet wordt gestart om te repliceren. Replicatie kan niet opnieuw worden gestart.
>
>

## <a name="time-taken-for-failover-to-azure"></a>Gebruikte tijd voor failover naar Azure

Failover van virtuele machines vereist in bepaalde gevallen, een extra tussenstap die meestal ongeveer 8 tot en met 10 minuten duurt om te voltooien. In de volgende gevallen worden de benodigde tijd voor failover hoger dan normaal:

* Met behulp van de mobility-service van de versie die ouder zijn dan 9,8 virtuele VMware-machines
* Fysieke servers
* Virtuele VMware-Linux-machines
* Hyper-V virtuele machines beveiligd als fysieke servers
* Virtuele VMware-machines waar volgende stuurprogramma's niet aanwezig is als opstartstuurprogramma's zijn
    * storvsc
    * VMBus
    * storflt
    * Intelide
    * ATAPI
* VMware-machines die geen DHCP-service is ingeschakeld, ongeacht of ze van DHCP- of statische gebruikmaakt IP-adressen

In alle andere gevallen, deze tussenliggende stap is niet vereist en de gebruikte tijd voor de failover is lager.

## <a name="using-scripts-in-failover"></a>Met behulp van scripts tijdens Failover
U kunt bepaalde acties automatiseren, terwijl u een failover uitvoert. U kunt scripts gebruiken of [Azure automation-runbooks](site-recovery-runbook-automation.md) in [herstelplannen](site-recovery-create-recovery-plans.md) om dat te doen.

## <a name="post-failover-considerations"></a>Overwegingen met betrekking tot post-failover
Na een failover die kunt u de onderstaande aanbevelingen:
### <a name="retaining-drive-letter-after-failover"></a>Stationsletter behouden na een failover
Als u wilt behouden de stationsletter op virtuele machines na een failover, kunt u instellen de **SAN-beleid** voor de virtuele machine **OnlineAll**. [Meer informatie](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Als u na een failover verbinding wilt maken met virtuele Azure-machines met behulp van RDP/SSH, volgt u de procedure die [hier](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) wordt beschreven.

Volg de stappen die [hier](site-recovery-failover-to-azure-troubleshoot.md) worden beschreven om eventuele verbindingsproblemen na een failover op te lossen.


## <a name="next-steps"></a>Volgende stappen

> [!WARNING]
> Als u failover van virtuele machines zijn mislukt en de on-premises Datacenter beschikbaar is, moet u [ **opnieuw beveiligen** ](vmware-azure-reprotect.md) virtuele VMware-machines terug naar de on-premises Datacenter.

Gebruik [ **geplande failover** ](hyper-v-azure-failback.md) optie naar **Failback** Hyper-v virtuele machines naar on-premises van Azure.

Als u hebben via een Hyper-v virtuele machine naar een andere on-premises datacenter die worden beheerd door een VMM-server en het primaire Datacenter beschikbaar is, gebruikt u vervolgens **omgekeerde replicatie** optie voor het starten van de replicatie terug naar de primaire gegevens Center.
