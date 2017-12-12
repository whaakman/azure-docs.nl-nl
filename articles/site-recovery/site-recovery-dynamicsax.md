---
title: Een Dynamics AX-implementatie met meerdere lagen repliceren met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe repliceren en Dynamics AX beveiligen met behulp van Azure Site Recovery
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: asgang
ms.openlocfilehash: 8ffc4a5a573b1c5951fab98fb766aed36f626fe4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="replicate-a-multitier-dynamics-ax-application-by-using-azure-site-recovery"></a>Een met meerdere lagen Dynamics AX-toepassing repliceren met behulp van Azure Site Recovery

## <a name="overview"></a>Overzicht


 Dynamics AX is een van de meest populaire ERP-oplossingen die wordt gebruikt door bedrijven Standaardiseer processen in locaties, resources beheren en naleving wordt vereenvoudigd. Omdat de toepassing essentieel voor een organisatie, in het geval van een ramp is, moet de toepassing actief en werkend minimumtijd.

Vandaag de dag biedt Dynamics AX niet alle out-of-the-box-noodherstel herstelfuncties. Dynamics AX bestaat uit meerdere serveronderdelen, zoals Windows AOS-, Azure Active Directory, Azure SQL Database, SharePoint Server en Reporting Services. Voor het beheren van de disaster is recovery van elk van deze onderdelen handmatig niet alleen dure maar ook foutgevoelig.

Dit artikel wordt uitgelegd hoe u een noodherstel voor uw Dynamics AX-toepassing kunt maken met behulp van [Azure Site Recovery](site-recovery-overview.md). Het omvat tevens geplande/niet-geplande testfailovers met behulp van een herstelplan met één muisklik, ondersteunde configuraties en vereisten.



## <a name="prerequisites"></a>Vereisten

Herstel na noodgevallen voor Dynamics AX-toepassing implementeren met behulp van Site Recovery is het volgende vereist:

• Instellen van een on-premises Dynamics AX-implementatie.

• Een Site Recovery-kluis in een Azure-abonnement maken.

• Als Azure uw site recovery, voer het hulpprogramma Azure virtuele Machine Readiness Assessment op de virtuele machines. Ze moeten compatibel zijn met de Azure virtuele Machines en de Site Recovery-services zijn.

## <a name="site-recovery-support"></a>Site Recovery-ondersteuning

Omwille van de in dit artikel maakt, hebben we virtuele VMware-machines met Dynamics AX 2012 R3 op Windows Server 2012 R2 Enterprise gebruikt. Omdat replicatie van site recovery toepassing networkdirect, verwachten we de aanbevelingen die voor het opslaan van de volgende scenario's.

### <a name="source-and-target"></a>Bron en doel

**Scenario** | **Een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysieke server** | Ja | Ja

## <a name="enable-disaster-recovery-of-the-dynamics-ax-application-by-using-site-recovery"></a>Herstel na noodgevallen van de toepassing Dynamics AX inschakelen met behulp van Site Recovery
### <a name="protect-your-dynamics-ax-application"></a>Uw toepassing Dynamics AX beveiligen
Schakel in de volledige toepassing replicatie en herstel door moet elk onderdeel van Dynamics AX worden beveiligd. 

### <a name="1-set-up-active-directory-and-dns-replication"></a>1. De replicatie van Active Directory en DNS instellen

Active Directory is vereist op de site van het herstel na noodgevallen voor de Dynamics AX-toepassing naar de functie. U wordt aangeraden de volgende twee opties op basis van de complexiteit van de klant on-premises omgeving.

**Optie 1**

De klant heeft een klein aantal toepassingen en één domeincontroller voor het gehele on-premises site en plannen voor de hele site tegelijk failover. Het is raadzaam dat u Site Recovery-replicatie gebruiken voor het repliceren van de domain controller machine naar een secundaire site (van toepassing voor scenario's voor zowel de site-naar-site en de site naar Azure).

**Optie 2**

De klant heeft een groot aantal toepassingen en een Active Directory-forest en planningen enkele toepassingen tegelijk failover wordt uitgevoerd. Het is raadzaam dat u een extra domeincontroller op de site van de herstel na noodgevallen instellen (een secundaire site of in Azure).

 Zie voor meer informatie [een domeincontroller beschikbaar maken op een herstel na noodgevallen site](site-recovery-active-directory.md). Voor de rest van dit document, nemen we aan dat een domeincontroller beschikbaar op de site van de herstel na noodgevallen is.

### <a name="2-set-up-sql-server-replication"></a>2. SQL Server-replicatie instellen
Zie voor technische richtlijnen op de aanbevolen optie voor het beveiligen van de SQL-laag [toepassingen met SQL Server en Azure Site Recovery repliceren](site-recovery-sql.md).

### <a name="3-enable-protection-for-the-dynamics-ax-client-and-application-object-server-vms"></a>3. Schakel de beveiliging voor de Dynamics AX-client en Server-toepassing Object VM 's
Uitvoeren van de desbetreffende Site Recovery-configuratie op basis van het feit of de virtuele machines op zijn geïmplementeerd [Hyper-V](site-recovery-hyper-v-site-to-azure.md) of [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Het is raadzaam dat u de crashconsistent frequentie tot 15 minuten configureert.
>

De volgende momentopname toont de beveiligingsstatus van Dynamics onderdeel VM's in een scenario met VMware-site naar Azure-beveiliging.

![Beveiligde items](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Netwerken configureren
**Configureer VM compute en -instellingen**

Voor de Dynamics AX-client en Server-toepassing Object VM's, netwerkinstellingen in Site Recovery zodanig configureren dat de VM-netwerken ophalen gekoppeld aan het juiste disaster recovery netwerk na een failover. Zorg ervoor dat het netwerk van het herstel na noodgevallen voor deze lagen routeerbaar zijn naar de SQL-laag.

U kunt de virtuele machine selecteren in de gerepliceerde items voor het configureren van de netwerkinstellingen zoals weergegeven in de momentopname van de volgende:

* Selecteer de juiste beschikbaarheidsset voor AOS-servers.

* Als u een statisch IP-adres gebruikt, geeft u het IP-adres dat u wilt dat de virtuele machine moet worden uitgevoerd de **IP-adres doel** in het tekstvak.

    ![Netwerkinstellingen ](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png).


### <a name="5-create-a-recovery-plan"></a>5. Een herstelplan maken

U kunt een herstelplan maken in Site Recovery voor de failover te automatiseren. Een app-laag en een weblaag toevoegen in het herstelplan. In verschillende groepen zo gerangschikt dat de front-end wordt afgesloten voordat de app-laag.

1. Selecteer de Site Recovery-kluis in uw abonnement en selecteer de **herstelplannen** tegel.

2. Selecteer **+ herstelplan**, en geef een naam op.

3. Selecteer de **bron** en **doel**. Het doel mag Azure of een secundaire site. Als u Azure kiest, moet u het implementatiemodel.

    ![Herstelplan maken](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4. Selecteer de AOS-Server en de client virtuele machines voor het herstelplan en de ✓ selecteren.

    ![Items selecteren](./media/site-recovery-dynamics-ax/selectvms.png)

    Voorbeeld van een herstel:

    ![Details van het herstel](./media/site-recovery-dynamics-ax/recoveryplan.png)

U kunt het herstelplan voor de Dynamics AX-toepassing aanpassen door de volgende stappen toe te voegen. De vorige momentopname ziet u het volledige herstelplan nadat u alle stappen toevoegen.


* **SQL Server failover stappen**: Zie voor meer informatie over specifieke herstelstappen voor het SQL server [replicatie toepassingen met SQL Server en Azure Site Recovery](site-recovery-sql.md).

* **Failover-groep 1**: failover voor de toepassing Object Server virtuele machines.
Zorg ervoor dat het geselecteerde herstelpunt zo dicht mogelijk bij de database PIT, maar niet voor deze.

* **Script**: Voeg de load balancer (alleen E-A).
Toevoegen aan een load balancer toevoegen wordt een script (via Azure Automation) nadat de virtuele machine de Server van de toepassing Object-groep. Een script kunt u deze taak. Zie voor meer informatie [het toevoegen van een load balancer voor noodherstel van de toepassing met meerdere lagen](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/).

* **Failover-groep 2**: failover voor de Dynamics AX-client virtuele machines. Failover van de weblaag virtuele machines als onderdeel van het herstelplan.


### <a name="perform-a-test-failover"></a>Een testfailover uitvoeren

Zie de handleiding 'Active Directory-noodherstel' voor meer informatie over Active Directory tijdens de testfailover. 

Zie voor meer informatie specifieke SQL server tijdens de testfailover, [toepassingen met SQL Server en Azure Site Recovery repliceren](site-recovery-sql.md).

1. Ga naar de Azure-portal en selecteer de Site Recovery-kluis.

2. Selecteer het herstelplan gemaakt voor Dynamics AX.

3. Selecteer **Testfailover**.

4. Selecteer het virtuele netwerk om de test failover-proces te starten.

5. Nadat de secundaire-omgeving is, kunt u uw controles uitvoeren.

6. Nadat de validaties voltooid zijn, selecteert u **validaties voltooien** en de testfailoveromgeving is opgeschoond.

Zie voor meer informatie over het uitvoeren van een testfailover [testen van failover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

### <a name="perform-a-failover"></a>Een failover uitvoeren

1. Ga naar de Azure-portal en selecteer de Site Recovery-kluis.

2. Selecteer het herstelplan gemaakt voor Dynamics AX.

3. Selecteer **Failover**, en selecteer **Failover**.

4. Selecteer het doelnetwerk en selecteer **✓** om de failoverproces te starten.

Zie voor meer informatie over u een failover uitvoert, [-Failover in Site Recovery](site-recovery-failover.md).

### <a name="perform-a-failback"></a>Een failback uitvoeren

Zie voor overwegingen specifieke met SQL Server tijdens de failback [toepassingen met SQL Server en Azure Site Recovery repliceren](site-recovery-sql.md).

1. Ga naar de Azure-portal en selecteer de Site Recovery-kluis.

2. Selecteer het herstelplan gemaakt voor Dynamics AX.

3. Selecteer **Failover**, en selecteer **Failover**.

4. Selecteer **richting wijzigen**.

5. Selecteer de gewenste opties: synchronisatie van gegevens en maken van VM.

6. Selecteer **✓** om de failback proces te starten.


Zie voor meer informatie over het uitvoeren van een failback [Failback VMware-machines van Azure met on-premises](site-recovery-failback-azure-to-vmware.md).

## <a name="summary"></a>Samenvatting
Met behulp van Site Recovery kunt u een herstelplan volledig geautomatiseerd noodherstel voor uw Dynamics AX-toepassing. U kunt in het geval van een onderbreking van de start van de failover binnen enkele seconden vanaf elke locatie en ophalen van de toepassing actief en werkend in minuten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beveiligen van workloads met Site Recovery enterprise, [welke workloads kan ik beveiligen?](site-recovery-workload.md).
