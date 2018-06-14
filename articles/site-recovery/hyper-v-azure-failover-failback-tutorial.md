---
title: Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar Azure met Site Recovery | Microsoft Docs
description: Informatie over het Hyper-V-machines in Azure een failover en failback naar de lokale site, met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/8/2018
ms.author: raynew
ms.openlocfilehash: 7863feb29fbb04f643aa3b7e1984209f44cdbe9a
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29852894"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar Azure

Deze zelfstudie wordt beschreven hoe u een Hyper-V virtuele machine in Azure een failover. Nadat u een failover hebt uitgevoerd, valt u terug op uw on-premises site wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of de Hyper-V VM-eigenschappen om te controleren in overeenstemming met de Azure-vereisten
> * Een failover naar Azure uitvoeren
> * Azure-VM’s opnieuw beveiligen naar de on-premises site
> * Failback van Azure met on-premises
> * On-premises VM’s opnieuw beveiligen om weer te beginnen met replicatie naar Azure

Dit is de vijfde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid.

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](tutorial-prepare-on-premises-hyper-v.md)
3. Instellen van herstel na noodgevallen voor [Hyper-V-machines](tutorial-hyper-v-to-azure.md), of voor [Hyper-V-machines in System Center VMM-clouds worden beheerd](tutorial-hyper-v-vmm-to-azure.md)
4. [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Failover en failback voorbereiden

Controleer of er zijn geen momentopnamen op de virtuele machine en de lokale virtuele machine is uitgeschakeld tijdens de beveiligingspoging. Dit helpt ervoor te zorgen dat gegevens tijdens de replicatie consistent blijven. Schakel de VM niet in nadat het opnieuw beveiligen is voltooid. 

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure**: failover-overschakeling uitvoeren van computers van de on-premises site naar Azure.
2. **Beveiligt virtuele Azure-machines**: beveiligt de Azure VM's, zodat ze weer worden gerepliceerd naar de lokale Hyper-V virtuele machines starten.
3. **Failover naar on-premises**: een failover van Azure naar uw on-premises site, wordt uitvoert wanneer deze beschikbaar is.
4. **Beveiligt lokale VMs**: nadat de gegevens terug is mislukt, beveiligt u de lokale virtuele machines om ze te repliceren naar Azure.

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Verifieer de VM-eigenschappen en zorg ervoor dat de VM voldoet aan de [Azure-vereisten](hyper-v-azure-support-matrix.md#replicated-vms).

1. In **beveiligde Items**, klikt u op **gerepliceerde Items** >< VM-naam >.

2. In de **gerepliceerde item** deelvenster bekijken van de VM-informatie en status van de meest recente beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
     - In **berekening en netwerk**, kunt u de VM-instellingen wijzigen en netwerk-instellingen, inclusief het netwerksubnet waarin de Azure VM. Beheerde schijven worden niet ondersteund voor failback van Azure naar Hyper-V.
   worden opgeslagen na de failover- en het IP-adres dat wordt toegewezen aan deze.
    - In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="fail-over-to-azure"></a>Failover naar Azure

1. Klik in **Instellingen** > **Gerepliceerde items** op de VM > **Failover**.
2. In **Failover** selecteert u de **nieuwste** herstelpunt. 
3. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery probeert te doen virtuele bronmachines afgesloten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Nadat u het failover-klikt u op verifiëren **doorvoeren**. Hiermee verwijdert u alle beschikbare herstelpunten.

> [!WARNING]
> **Annuleer een failover die in voortgang is niet**: voordat de failover wordt gestart, wordt de VM-replicatie gestopt. Als u bezig failover gestopt, annuleren, maar de virtuele machine opnieuw won't repliceren.

## <a name="reprotect-azure-vms"></a>Azure VM’s opnieuw beveiligen

1. In de **AzureVMVault** > **gerepliceerde items**, met de rechtermuisknop op de virtuele machine die failover is uitgevoerd en selecteer **opnieuw beveiligen**.
2. Controleren of de richting van de beveiliging is ingesteld op **Azure met On-premises**.
3. De lokale virtuele machine is uitgeschakeld tijdens de beveiligingspoging om te zorgen voor consistentie van de gegevens. Niet inschakelen nadat de beveiligingspoging voltooid.
4. Nadat het proces beveiligingspoging Start de virtuele machine van Azure te repliceren naar de lokale site.



## <a name="fail-over-from-azure-and-reprotect-the-on-premises-vm"></a>Failover van Azure en beveiligt u de lokale virtuele machine

Failover van Azure naar de lokale site en start de virtuele machines van de lokale site repliceren naar Azure.

1. In **instellingen** > **gerepliceerde items**, klikt u op de virtuele machine > **geplande Failover**.
2. In **geplande Failover bevestigen**, Controleer of de failoverrichting (van Azure) en selecteert u de bron- en doellocaties.
3. Selecteer **gegevens vóór de failover synchroniseren (alleen nog deltawijzigingen gesynchroniseerd)**. Deze optie wordt VM uitvaltijd geminimaliseerd omdat het synchroniseert zonder dat de virtuele machine afgesloten.
4. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
5. Na de initiële gegevens synchronisatie wordt uitgevoerd en u klaar bent om het virtuele Azure-machines klikt u op afsluiten **taken** > geplande-failover-job-name > **volledige Failover**. De Azure VM is afgesloten, de overgedragen van de meest recente wijzigingen on-premises en de lokale virtuele machine wordt gestart.
6. Meld u bij de lokale virtuele machine om te controleren is beschikbaar, zoals verwacht.
7. De lokale virtuele machine wordt nu een **doorvoeren in behandeling** status. Klik op **doorvoeren**. Dit wordt verwijderd van de Azure VM's en de schijven en wordt de lokale virtuele machine voorbereid voor omgekeerde replicatie.
Inschakelen voor het starten van de lokale virtuele machine repliceren naar Azure, **omgekeerde replicatie**. Dit activeert de replicatie van deltawijzigingen die hebben plaatsgevonden sinds de Azure VM is uitgeschakeld.  
