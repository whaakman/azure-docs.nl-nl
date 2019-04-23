---
title: Failover- en failback-overschakeling uitvoeren van VMware-VM’s en fysieke servers tijdens herstel na noodgeval in Azure met Site Recovery | Microsoft Docs
description: Leer een failover-overschakeling uitvoeren van VMware-VM’s en fysieke servers naar Azure, en een failback naar de on-premises site, tijdens herstel na noodgeval, met Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9206e751fadab7a09c696fbe262aecdde002ae74
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797662"
---
# <a name="fail-over-and-fail-back-vmware-vms"></a>Failover en failback virtuele VMware-machines

Dit artikel wordt beschreven hoe u een on-premises VMware-VM naar Azure failover [Azure Site Recovery](site-recovery-overview.md) service. 

Dit is de vijfde zelfstudie in een serie die laat hoe zien het instellen van herstel na noodgevallen naar Azure voor on-premises machines u.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of de VMware-VM-eigenschappen in overeenstemming zijn met de Azure-vereisten
> * Een failover naar Azure uitvoeren


> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Als u wilt meer informatie over failover in detail [raadpleegt u dit artikel](site-recovery-failover.md).

## <a name="before-you-start"></a>Voordat u begint
Voltooi de vorige zelfstudies:

1. Zorg ervoor dat u hebt [Azure instellen](tutorial-prepare-azure.md) voor on-premises herstel na noodgeval VMware-VM's, Hyper-V-machines en fysieke machines naar Azure.
2. Voorbereiden van uw on-premises [VMware](vmware-azure-tutorial-prepare-on-premises.md) of [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) omgeving voor herstel na noodgevallen. Als het instellen van herstel na noodgevallen voor fysieke servers, raadpleegt u de [ondersteuningsmatrix](vmware-physical-secondary-support-matrix.md).
3. Instellen van herstel na noodgevallen voor [virtuele VMware-machines](vmware-azure-tutorial.md), [Hyper-V-machines](hyper-v-azure-tutorial.md), of [fysieke machines](physical-azure-disaster-recovery.md).
4. Voer een [Dr-herstelanalyse](tutorial-dr-drill-azure.md) om ervoor te zorgen dat alles werkt zoals verwacht.


## <a name="failover-and-failback"></a>Failover en failback

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure**: Als uw on-premises primaire site uitvalt, schakelt u machines over naar Azure. Na een failover worden virtuele Azure-machines worden gemaakt van gerepliceerde gegevens.
2. **Azure-VM’s opnieuw beveiligen**: In Azure, door de Azure-VM's opnieuw te beveiligen zodat ze gaan repliceren naar on-premises VMware-VM's. De on-premises VM is altijd uitgeschakeld tijdens het opnieuw beveiligen om te zorgen voor consistentie van gegevens.
3. **Failover naar on-premises**: Als uw on-premises site actief is en een failover naar een failback van Azure wordt uitgevoerd, worden uitgevoerd.
4. **On-premises VM's opnieuw beveiligen**: Nadat de gegevens terug is mislukt, de on-premises VM's opnieuw beveiligen waaraan u een failback uitgevoerd, zodat ze gaan repliceren naar Azure.

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Voordat u een failover uitvoert, Controleer de eigenschappen van de virtuele machine en ervoor zorgen dat virtuele machines aan [Azure-vereisten](vmware-physical-azure-support-matrix.md#replicated-machines).

Controleer de eigenschappen als volgt:

1. Klik in **Beveiligde items** op **Gerepliceerde items** > VM.

2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.

3. In **Berekening en netwerk** kunt u de Azure-naam, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md) en beheerde-schijfinstellingen wijzigen

4. U kunt de netwerkinstellingen bekijken en wijzigen, inclusief het netwerk-/subnet waarin de Azure VM zich na failover bevindt en het IP-adres dat eraan wordt toegewezen.

5. In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="run-a-failover-to-azure"></a>Een failover naar Azure uitvoeren

1. Klik in **Instellingen** > **Gerepliceerde items** op de VM > **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:
   - **Laatste**: Met deze optie worden eerst alle gegevens verwerkt die naar Site Recovery zijn verzonden. Dit biedt het laagste RPO (Recovery Point Objective), omdat de na de failover gemaakte Azure-VM alle gegevens heeft die naar Site Recovery is gerepliceerd toen de failover werd geactiveerd.
   - **Laatst verwerkt**: Deze optie wordt de virtuele machine failover optreedt naar de meest recente herstelpunt dat is verwerkt door Site Recovery. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.
   - **Laatste toepassingsconsistente punt**: Met deze optie wordt er een failover uitgevoerd van de VM naar het laatste door Site Recovery verwerkte toepassingsconsistente herstelpunt.
   - **Aangepast**: Geef een herstelpunt op.

3. Selecteer **sluit de computer voordat u begint met failover** probeert een afsluiten van de bron-VM's voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. Ziet u mogelijk meer failover-tijden voor:
- Virtuele VMware-machines met een ouder is dan 9,8 Mobility serviceversie
- Fysieke servers
- VMware Linux-VM 's
- Hyper-V-machines die zijn beveiligd als fysieke servers
- VMware-VM's waarop niet de DHCP-service is ingeschakeld
- VMware-VM's die niet de volgende opstartstuurprogramma's hebben: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> **Annuleer nooit een failover die in uitvoering is**: De VM-replicatie wordt gestopt voordat de failover is gestart. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.

## <a name="connect-to-failed-over-vm"></a>Verbinding maken met virtuele machine failover

1. Als u wilt verbinding maken met virtuele Azure-machines na een failover met behulp van RDP/SSH [controleren of deze vereisten](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
2. Na een failover, gaat u naar de virtuele machine en valideer met [verbinding](../virtual-machines/windows/connect-logon.md) toe.
3. Gebruik **herstelpunt wijzigen** als u wilt gebruiken een ander herstelpunt na een failover. Nadat u de failover in de volgende stap geeft, is deze optie niet langer beschikbaar.
4. Klik na de validatie op **doorvoeren** voor het voltooien van het herstelpunt van de virtuele machine na een failover.
5. Na doorvoeren, worden alle andere beschikbare herstelpunten verwijderd. Deze failover is voltooid.

>[!TIP]
> Als u eventuele problemen met de netwerkverbinding na een failover ondervindt, volgt u deze [problemen oplossen met](site-recovery-failover-to-azure-troubleshoot.md).

## <a name="next-steps"></a>Volgende stappen

Na een failover, door de Azure VM's naar on-premises opnieuw te beveiligen. Vervolgens, nadat de virtuele machines opnieuw en repliceren naar de on-premises site zijn, failback van Azure wanneer u klaar bent.

> [!div class="nextstepaction"]
> [Azure VM's opnieuw beveiligen](vmware-azure-reprotect.md)
> [failback van Azure](vmware-azure-failback.md) 
