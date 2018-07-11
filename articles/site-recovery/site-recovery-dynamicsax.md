---
title: Een Dynamics AX-implementatie met meerdere lagen te repliceren met behulp van Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u voor het repliceren en Dynamics AX beveiligen met behulp van Azure Site Recovery
services: site-recovery
documentationcenter: ''
author: asgang
manager: rochakm
editor: ''
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: asgang
ms.openlocfilehash: 931cc628dccc77a026791b27a7a8159b37c585d4
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919560"
---
# <a name="replicate-a-multitier-dynamics-ax-application-by-using-azure-site-recovery"></a>Een met meerdere lagen Dynamics AX-toepassing repliceren met behulp van Azure Site Recovery

## <a name="overview"></a>Overzicht


 Dynamics AX is een van de meest populaire ERP-oplossingen die processen te standaardiseren op locaties, resources beheren en naleving vereenvoudigen door ondernemingen gebruikt. Omdat de toepassing essentieel voor een organisatie, in het geval van een noodgeval is, moet de toepassing actief en werkend worden in de minimale tijd.

Vandaag de dag biedt Dynamics AX geen out-of-the-box noodgevallen mogelijkheden voor herstel. Dynamics AX bestaat uit meerdere serveronderdelen, zoals Windows-toepassingsserver Object, Azure Active Directory, Azure SQL Database, SharePoint Server en Reporting Services. Voor het beheren van de disaster is recovery van elk van deze onderdelen handmatig niet alleen dure, maar ook foutgevoelig.

Dit artikel wordt uitgelegd hoe u een oplossing voor noodherstel voor uw Dynamics AX-toepassing kunt maken met behulp van [Azure Site Recovery](site-recovery-overview.md). Ook wordt de geplande/niet-geplande testfailovers beschreven met behulp van een herstelplan met één klik, ondersteunde configuraties en vereisten.



## <a name="prerequisites"></a>Vereisten

Herstel na noodgevallen voor Dynamics AX-toepassing implementeren met behulp van Site Recovery, moet de volgende vereisten:

• Instellen van een on-premises Dynamics AX-implementatie.

• Een Site Recovery-kluis maken in een Azure-abonnement.

• Als Azure uw herstelsite, voer het hulpprogramma Azure Virtual Machine Readiness Assessment op de virtuele machines. Moeten compatibel zijn met de Azure Virtual Machines en Site Recovery-services.

## <a name="site-recovery-support"></a>Ondersteuning voor site Recovery

Ten behoeve van het maken van dit artikel, hebben we virtuele VMware-machines gebruikt met Dynamics AX 2012 R3 op Windows Server 2012 R2 Enterprise. Omdat replicatie van site recovery neutraal van toepassing is, maar we verwachten dat de aanbevelingen die hier beschikbaar zijn voor het opslaan van de volgende scenario's.

### <a name="source-and-target"></a>Bron en doel

**Scenario** | **Naar een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysieke server** | Ja | Ja

## <a name="enable-disaster-recovery-of-the-dynamics-ax-application-by-using-site-recovery"></a>Inschakelen van herstel na noodgevallen van de Dynamics AX-toepassing met behulp van Site Recovery
### <a name="protect-your-dynamics-ax-application"></a>Bescherm uw Dynamics AX-toepassing
Schakel de replicatie van de volledige toepassing en het herstel door moet elk onderdeel van Dynamics AX worden beveiligd.

### <a name="1-set-up-active-directory-and-dns-replication"></a>1. Active Directory en DNS-replicatie instellen

Active Directory is vereist op de site voor noodherstel voor de Dynamics AX-toepassing op de functie. U wordt aangeraden de volgende twee opties op basis van de complexiteit van de klant on-premises omgeving.

**Optie 1**

De klant heeft een klein aantal toepassingen en één domeincontroller voor het gehele on-premises site en wil een failover wordt uitgevoerd de gehele site. U wordt aangeraden gebruik te maken van Site Recovery-replicatie voor het repliceren van de domain controller machine naar een secundaire site (van toepassing voor site-naar-site- en site-naar-Azure's).

**Optie 2**

De klant heeft een groot aantal toepassingen en een Active Directory-forest en plannen voor een aantal toepassingen tegelijk failover wordt uitgevoerd. Het is raadzaam dat u een extra domeincontroller op de site voor noodherstel instellen (een secundaire site of in Azure).

 Zie voor meer informatie, [een domeincontroller beschikbaar maken op een site voor noodherstel](site-recovery-active-directory.md). Voor de rest van dit document veronderstellen we dat een domeincontroller beschikbaar op de site voor noodherstel is.

### <a name="2-set-up-sql-server-replication"></a>2. SQL Server-replicatie instellen
Zie voor technische richtlijnen voor de aanbevolen optie voor het beveiligen van de SQL-laag [toepassingen met SQL Server en Azure Site Recovery repliceert](site-recovery-sql.md).

### <a name="3-enable-protection-for-the-dynamics-ax-client-and-application-object-server-vms"></a>3. Schakel de beveiliging voor de Dynamics AX-client en de toepassing Object Server-VM 's
Uitvoeren van de desbetreffende Site Recovery-configuratie op of de virtuele machines zijn geïmplementeerd op basis van [Hyper-V](site-recovery-hyper-v-site-to-azure.md) of [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Het is raadzaam dat u de crash-consistente frequentie op 15 minuten configureert.
>

De volgende momentopname toont de status van de beveiliging van virtuele machines Dynamics-component in een scenario voor VMware-site-naar-Azure-beveiliging.

![Beveiligde items](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Netwerk configureren
**Berekening van de virtuele machine configureren en instellingen**

Voor de Dynamics AX-client en de toepassing Object Server-VM's, netwerkinstellingen in Site Recovery zo configureren dat de VM-netwerken ophalen die is gekoppeld aan het juiste disaster recovery-netwerk na een failover. Zorg ervoor dat het netwerk van het herstel na noodgevallen voor deze lagen routeerbaar zijn naar de SQL-laag.

Selecteer de virtuele machine in de gerepliceerde items de netwerkinstellingen configureren zoals wordt weergegeven in de momentopname van het volgende:

* Voor servers van de toepassingsserver Object, selecteert u de juiste beschikbaarheidsset.

* Als u een statisch IP-adres gebruikt, geeft u de IP-adres dat u wilt dat de virtuele machine te nemen de **doel-IP** in het tekstvak.

    ![Netwerkinstellingen ](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png).


### <a name="5-create-a-recovery-plan"></a>5. Een herstelplan maken

U kunt een herstelplan maken in Site Recovery om de failoverproces te automatiseren. Een app-laag en een weblaag in het herstelplan te gaan toevoegen. Zodat de front-end wordt afgesloten voordat de app-laag, moet u ze rangschikken in verschillende groepen.

1. Selecteer de Site Recovery-kluis in uw abonnement en selecteer de **herstelplannen** tegel.

2. Selecteer **+ herstelplan**, en geef een naam op.

3. Selecteer de **bron** en **doel**. Het doel is Azure of een secundaire site. Als u voor Azure kiest, moet u het implementatiemodel opgeven.

    ![Herstelplan maken](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4. Selecteer de toepassingsserver Object en de client VM's voor het herstelplan te gaan en selecteer de ✓.

    ![Items selecteren](./media/site-recovery-dynamics-ax/selectvms.png)

    Voorbeeld van een herstel:

    ![Abonnementsdetails voor herstel](./media/site-recovery-dynamics-ax/recoveryplan.png)

U kunt het herstelplan te gaan voor de Dynamics AX-toepassing aanpassen door het toevoegen van de volgende stappen uit. De vorige momentopname bevat het volledige herstelplan te gaan nadat u alle stappen hebt toegevoegd.


* **SQL Server failover stappen**: Zie voor meer informatie over herstelstappen die specifiek zijn voor SQL server [replicatie-toepassingen met SQL Server en Azure Site Recovery](site-recovery-sql.md).

* **Failover-groep 1**: de toepassing Object Server virtuele machines een failover.
Zorg ervoor dat het geselecteerde herstelpunt wordt zo dicht mogelijk bij de PIT,-database, maar niet voor deze.

* **Script**: toevoegen netwerktaakverdeler (alleen E-A).
Voeg een script (via Azure Automation) nadat de virtuele machine de Server van de toepassing-Object-groep wordt weergegeven aan een load balancer toevoegen. U kunt een script gebruiken voor deze taak. Zie voor meer informatie, [toevoegen van een load balancer voor noodherstel van de toepassing met meerdere lagen](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/).

* **Failover-groep 2**: failover van de Dynamics AX-client VM's. Failover van de weblaag VM's als onderdeel van het herstelplan te gaan.


### <a name="perform-a-test-failover"></a>Een testfailover uitvoeren

Voor meer informatie over Active Directory tijdens de testfailover, Zie de handleiding 'Oplossing voor noodherstel Active Directory'.

Zie voor meer informatie over SQL server tijdens de testfailover, [toepassingen met SQL Server en Azure Site Recovery repliceert](site-recovery-sql.md).

1. Ga naar de Azure-portal en selecteert u de Site Recovery-kluis.

2. Selecteer het plan voor herstel gemaakt voor Dynamics AX.

3. Selecteer **Testfailover**.

4. Selecteer het virtuele netwerk om de test-failover-proces te starten.

5. Nadat de secundaire-omgeving is, kunt u uw validaties uitvoeren.

6. Nadat de controles voltooid zijn, selecteert u **voltooien van validaties** en de testfailoveromgeving is opgeschoond.

Zie voor meer informatie over het uitvoeren van een testfailover [Testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

### <a name="perform-a-failover"></a>Een failover uitvoeren

1. Ga naar de Azure-portal en selecteert u de Site Recovery-kluis.

2. Selecteer het plan voor herstel gemaakt voor Dynamics AX.

3. Selecteer **Failover**, en selecteer **Failover**.

4. Selecteer het doelnetwerk en selecteer **✓** om de failoverproces te starten.

Zie voor meer informatie over een failover uitvoert, [Failover in Site Recovery](site-recovery-failover.md).

### <a name="perform-a-failback"></a>Een failback uitvoeren

Zie voor overwegingen specifieke SQL Server tijdens de failback [toepassingen met SQL Server en Azure Site Recovery repliceert](site-recovery-sql.md).

1. Ga naar de Azure-portal en selecteert u de Site Recovery-kluis.

2. Selecteer het plan voor herstel gemaakt voor Dynamics AX.

3. Selecteer **Failover**, en selecteer **Failover**.

4. Selecteer **richting wijzigen**.

5. Selecteer de gewenste opties: synchronisatie van gegevens en het maken van virtuele machine.

6. Selecteer **✓** het failback-proces wilt starten.


Zie voor meer informatie over het uitvoeren van een failback [Failback virtuele VMware-machines van Azure naar on-premises](site-recovery-failback-azure-to-vmware.md).

## <a name="summary"></a>Samenvatting
U kunt een herstelplan volledig geautomatiseerd noodherstel voor uw Dynamics AX-toepassing maken met behulp van Site Recovery. U kunt in het geval van een onderbreking, start de failover binnen enkele seconden vanaf elke locatie en de toepassing actief en werkend binnen enkele minuten aan.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beveiligen van zakelijke workloads met Site Recovery, [welke workloads kan ik beveiligen?](site-recovery-workload.md).
