---
title: Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar Azure met Site Recovery | Microsoft Docs
description: Informatie over het Hyper-V-machines in Azure een failover en failback naar de lokale site, met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 6a34187a87c6ecda461357a1c2fc8747ddf4b056
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294289"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>Failover en failback Hyper-V-machines die zijn gerepliceerd naar Azure

Deze zelfstudie wordt beschreven hoe u een Hyper-V-VM naar Azure failover. Nadat u via u failback naar uw on-premises site mislukt hebt als deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of de Hyper-V VM-eigenschappen om te controleren in overeenstemming met de Azure-vereisten
> * Een failover naar Azure uitvoeren
> * Failback vanuit Azure naar on-premises
> * Omgekeerde replicatie lokale virtuele machines, als u wilt repliceren naar Azure opnieuw starten

Deze zelfstudie is de vijfde zelfstudie in een reeks. Er wordt vanuit gegaan dat u de taken in de vorige zelfstudies al hebt voltooid.    

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md)
3. Instellen van herstel na noodgevallen voor [Hyper-V-machines](tutorial-hyper-v-to-azure.md), of voor [Hyper-V-machines in System Center VMM-clouds worden beheerd](tutorial-hyper-v-vmm-to-azure.md)
4. [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Failover en failback voorbereiden

Controleer of er zijn geen momentopnamen op de virtuele machine en de lokale virtuele machine is uitgeschakeld tijdens de failback. Het helpt ervoor te zorgen dat gegevens tijdens de replicatie. Schakel niet op de lokale virtuele machine tijdens de failback. 

Failover en failback hebt drie fasen:

1. **Failover naar Azure**: Failover Hyper-V virtuele machines van de lokale site naar Azure.
2. **Failback naar on-premises**: Failover Azure VM's naar uw on-premises site, wanneer deze beschikbaar is. Virtuele machines repliceren naar de lokale Hyper-V virtuele machines wordt gestart. Na de eerste gegevenssynchronisatie, failover Azure VM's naar de lokale site.  
3. **Omgekeerde replicatie lokale VMs**: nadat de gegevens terug is mislukt, omgekeerde replicatie van de lokale virtuele machines starten die deze repliceert naar Azure.

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Controleer de eigenschappen van de VM voordat failover en zorg ervoor dat de virtuele machine met voldoen aan [Azure-vereisten](hyper-v-azure-support-matrix.md#replicated-vms).

1. In **beveiligde Items**, klikt u op **gerepliceerde Items** >< VM-naam >.

2. In de **gerepliceerde item** deelvenster bekijken van de VM-informatie en status van de meest recente beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
     - In **berekening en netwerk**, kunt u de VM-instellingen wijzigen en netwerkinstellingen, met inbegrip van de netwerk-en subnet waarin de virtuele machine in Azure worden geplaatst na een failover, en het IP-adres dat wordt toegewezen aan deze. Beheerde schijven worden niet ondersteund voor failback van Azure naar Hyper-V.
      - In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="failover-to-azure"></a>Failover naar Azure

1. Klik in **Instellingen** > **Gerepliceerde items** op de VM > **Failover**.
2. In **Failover** selecteert u de **nieuwste** herstelpunt. 
3. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery probeert te doen virtuele bronmachines afgesloten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Nadat u het failover-klikt u op verifiëren **doorvoeren**. De beschikbare herstelpunten worden verwijderd.

> [!WARNING]
> **Een failover uitgevoerd niet annuleren**: als u in voortgang, stopt de failover annuleert, maar de virtuele machine opnieuw won't repliceren.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Azure VM failback naar on-premises en omgekeerde replicatie van de lokale virtuele machine

Failback-bewerking is in feite een failover van Azure naar de lokale site en in de omgekeerde replicatie die wordt opnieuw gestart virtuele machines van de lokale site repliceren naar Azure.

1. In **instellingen** > **gerepliceerde items**, klikt u op de virtuele machine > **geplande Failover**.
2. In **geplande Failover bevestigen**, Controleer of de failoverrichting (van Azure) en selecteert u de bron- en doellocaties.
3. Selecteer **gegevens vóór de failover synchroniseren (alleen nog deltawijzigingen gesynchroniseerd)**. Deze optie wordt VM uitvaltijd geminimaliseerd omdat het synchroniseert zonder dat de virtuele machine afgesloten.
4. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
5. Na de initiële gegevens synchronisatie wordt uitgevoerd en u klaar bent om het virtuele Azure-machines klikt u op afsluiten **taken** > geplande-failover-job-name > **volledige Failover**. Deze de Azure VM is afgesloten, draagt de meest recente wijzigingen on-premises en de lokale virtuele machine wordt gestart.
6. Meld u bij de lokale virtuele machine om te controleren is beschikbaar, zoals verwacht.
7. De lokale virtuele machine wordt nu een **doorvoeren in behandeling** status. Klik op **doorvoeren**. Deze worden verwijderd van de Azure VM's en de schijven en wordt de lokale virtuele machine voorbereid voor omgekeerde replicatie.
Inschakelen voor het starten van de lokale virtuele machine repliceren naar Azure, **omgekeerde replicatie**. Deze gebeurtenis wordt de replicatie van deltawijzigingen die hebben plaatsgevonden sinds de Azure VM is uitgeschakeld.  
