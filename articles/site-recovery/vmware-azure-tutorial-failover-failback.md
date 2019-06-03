---
title: Failover- en failback-overschakeling uitvoeren van VMware-VM’s en fysieke servers tijdens herstel na noodgeval in Azure met Site Recovery | Microsoft Docs
description: Meer informatie over hoe u virtuele VMware-machines en fysieke servers naar Azure failover, en hoe u een failover naar de on-premises site, tijdens het herstel na noodgevallen naar Azure met behulp van Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 854e09600440c28f7505a016566ea0f05e994f5f
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418326"
---
# <a name="fail-over-and-fail-back-vmware-vms"></a>Failover en failback virtuele VMware-machines

In dit artikel wordt beschreven hoe u een on-premises VMware virtuele machine (VM) failover naar [Azure Site Recovery](site-recovery-overview.md).

Dit is de vijfde zelfstudie in een serie die laat hoe zien het instellen van herstel na noodgevallen naar Azure voor on-premises machines u.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of de VMware-VM-eigenschappen in overeenstemming met de Azure-vereisten.
> * Een failover naar Azure uitvoert.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Gebruik van standaardopties waar mogelijk en alle mogelijke instellingen en paden niet weergeven. Als u meer informatie over failover in detail wilt, Zie [failover van virtuele machines en fysieke servers](site-recovery-failover.md).

## <a name="before-you-start"></a>Voordat u begint

Voltooi de vorige zelfstudies:

1. Zorg ervoor dat u hebt [Azure instellen](tutorial-prepare-azure.md) voor on-premises herstel na noodgeval VMware-VM's, Hyper-V-machines en fysieke machines naar Azure.
2. Voorbereiden van uw on-premises [VMware](vmware-azure-tutorial-prepare-on-premises.md) of [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) omgeving voor herstel na noodgevallen. Als het instellen van herstel na noodgevallen voor fysieke servers, raadpleegt u de [ondersteuningsmatrix](vmware-physical-secondary-support-matrix.md).
3. Instellen van herstel na noodgevallen voor [virtuele VMware-machines](vmware-azure-tutorial.md), [Hyper-V-machines](hyper-v-azure-tutorial.md), of [fysieke machines](physical-azure-disaster-recovery.md).
4. Voer een [Dr-herstelanalyse](tutorial-dr-drill-azure.md) om ervoor te zorgen dat alles werkt zoals verwacht.

## <a name="failover-and-failback"></a>Failover en failback

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure:** Als uw on-premises primaire site uitvalt, schakelt u machines over naar Azure. Na een failover worden virtuele Azure-machines worden gemaakt van gerepliceerde gegevens.
2. **Azure VM's opnieuw beveiligen:** In Azure, door de Azure VM's opnieuw te beveiligen zodat ze gaan repliceren naar on-premises VMware-VM's. De on-premises VM is altijd uitgeschakeld tijdens het opnieuw beveiligen om te zorgen voor consistentie van gegevens.
3. **Failover naar on-premises:** Als uw on-premises site actief is en een failover naar een failback van Azure wordt uitgevoerd, worden uitgevoerd.
4. **On-premises VM's opnieuw beveiligen:** Nadat de gegevens terug is mislukt, de on-premises VM's opnieuw beveiligen waaraan u een failback uitgevoerd, zodat ze gaan repliceren naar Azure.

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Voordat u een failover uitvoert, controleert u de VM-eigenschappen om ervoor te zorgen dat de virtuele machines te voldoen aan [Azure-vereisten](vmware-physical-azure-support-matrix.md#replicated-machines).

Controleer de eigenschappen als volgt:

1. In **beveiligde Items**, selecteer **gerepliceerde Items**, en selecteer vervolgens de virtuele machine die u wilt controleren.

2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Selecteer **eigenschappen** om meer details weer te geven.

3. In **berekening en netwerk**, u kunt deze eigenschappen zo nodig wijzigen:
    * Naam van Azure
    * Resourcegroep
    * Doelgrootte
    * [Beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md)
    * Instellingen voor beheerde schijven

4. U kunt bekijken en wijzigen van de netwerkinstellingen, met inbegrip van:

    * Het netwerk en subnet waarin de Azure-VM na failover geplaatst worden wordt.
    * De IP-adres dat wordt toegewezen aan deze.

5. In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="run-a-failover-to-azure"></a>Een failover naar Azure uitvoeren

1. In **instellingen** > **gerepliceerde items**, selecteer de virtuele machine failover, en selecteer vervolgens de gewenste **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:
   * **Laatste**: Met deze optie worden eerst alle gegevens verwerkt die naar Site Recovery zijn verzonden. Het biedt de laagste Recovery Point Objective (RPO) omdat de Azure-virtuele machine die gemaakt na een failover de gegevens die is gerepliceerd naar de Site Recovery heeft wanneer de failover werd geactiveerd.
   * **Laatst verwerkt**: Deze optie wordt de virtuele machine failover optreedt naar de meest recente herstelpunt dat is verwerkt door Site Recovery. Deze optie biedt een lage RTO (Recovery Time Objective), omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.
   * **Laatste toepassingsconsistente punt**: Deze optie wordt de virtuele machine naar de meest recente app-consistente herstelpunt verwerkt door Site Recovery.
   * **Aangepast**: Deze optie kunt u een herstelpunt opgeven.

3. Selecteer **sluit de computer voordat u begint met failover** om bron-VM's afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.

In sommige scenario's vereist de failover extra verwerking die ongeveer 8 tot en met 10 minuten duurt om te voltooien. Ziet u mogelijk meer failover-tijden voor:

* Virtuele VMware-machines met een ouder is dan 9,8 Mobility serviceversie.
* Fysieke servers.
* VMware Linux-VM's.
* Hyper-V-machines die zijn beveiligd als fysieke servers.
* VMware VM's waarop niet de DHCP-service is ingeschakeld.
* VMware-VM's die niet de volgende opstartstuurprogramma's hebben: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> Annuleer een failover wordt uitgevoerd niet. De VM-replicatie wordt gestopt voordat de failover is gestart. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.

## <a name="connect-to-failed-over-vm"></a>Verbinding maken met failover-VM

1. Als u wilt verbinding maken met virtuele Azure-machines na een failover met behulp van Remote Desktop Protocol (RDP) en Secure Shell (SSH), [controleren of aan de vereisten is voldaan](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
2. Na een failover, gaat u naar de virtuele machine en valideer met [verbinding](../virtual-machines/windows/connect-logon.md) toe.
3. Gebruik **herstelpunt wijzigen** als u wilt gebruiken een ander herstelpunt na een failover. Nadat u de failover in de volgende stap geeft, is deze optie niet langer beschikbaar.
4. Selecteer na de validatie **doorvoeren** voor het voltooien van het herstelpunt van de virtuele machine na een failover.
5. Nadat u doorvoert, worden alle andere beschikbare herstelpunten worden verwijderd. Deze stap is voltooid voor de failover.

>[!TIP]
> Als u eventuele problemen met de netwerkverbinding na een failover ondervindt, volgt u de [problemen oplossen met](site-recovery-failover-to-azure-troubleshoot.md).

## <a name="next-steps"></a>Volgende stappen

Na een failover, door de Azure VM's naar on-premises opnieuw te beveiligen. Vervolgens, nadat de virtuele machines opnieuw en repliceren naar de on-premises site zijn, failback van Azure wanneer u klaar bent.

> [!div class="nextstepaction"]
> [Azure VM's opnieuw beveiligen](vmware-azure-reprotect.md)
> [failback van Azure](vmware-azure-failback.md)
