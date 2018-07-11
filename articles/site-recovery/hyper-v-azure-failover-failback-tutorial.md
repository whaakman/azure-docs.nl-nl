---
title: Failover en failback-Hyper-V-machines worden gerepliceerd naar Azure met Site Recovery | Microsoft Docs
description: Leer hoe u Hyper-V-machines naar Azure failover en failback uitvoeren naar de on-premises site, met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: f758939964045ed373703a211d4cbef00f0e42e7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919543"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>Failover en failback Hyper-V-machines worden gerepliceerd naar Azure

Deze zelfstudie wordt beschreven hoe u een Hyper-V-VM naar Azure failover. Nadat u hebt een failover, failback u naar uw on-premises site wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleer of de Hyper-V-VM-eigenschappen in overeenstemming zijn met Azure-vereisten
> * Een failover naar Azure uitvoeren
> * Failback van Azure naar on-premises
> * Omgekeerde replicatie on-premises machines, als u wilt repliceren naar Azure opnieuw starten

Deze zelfstudie is de vijfde zelfstudie in een reeks. Hierbij wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid.    

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises Hyper-V voorbereiden](tutorial-prepare-on-premises-hyper-v.md)
3. Instellen van herstel na noodgevallen voor [Hyper-V-machines](tutorial-hyper-v-to-azure.md), of voor [Hyper-V-machines worden beheerd in System Center VMM-clouds](tutorial-hyper-v-vmm-to-azure.md)
4. [Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Voorbereiden voor failover en failback

Zorg ervoor dat er geen momentopnamen zijn op de virtuele machine en de on-premises VM is altijd uitgeschakeld tijdens de failback. Het helpt zorgen voor gegevensconsistentie tijdens de replicatie. Op de on-premises virtuele machine niet inschakelt tijdens de failback. 

Failover en failback hebt drie fasen:

1. **Failover naar Azure**: failover-Hyper-V-machines uit de on-premises site naar Azure.
2. **Failback naar on-premises**: failover-Azure-VM's naar uw on-premises-site als de on-premises site beschikbaar is. Deze begint met het synchroniseren van gegevens van Azure naar on-premises en na voltooiing, brengt dit van de virtuele machines op on-premises.  
3. **Omgekeerde replicatie on-premises machines**: nadat kan niet naar on-premises, reverse de on-premises machines om te beginnen met het ze repliceren naar Azure repliceren.

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Voordat de failover controleren of de VM-eigenschappen en zorg ervoor dat de virtuele machine voldoet aan met [Azure-vereisten](hyper-v-azure-support-matrix.md#replicated-vms).

Klik in **Beveiligde items** op **Gerepliceerde items** > VM.

2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.

3. In **Berekening en netwerk** kunt u de Azure-naam, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md) en instellingen van de beheerde schijf wijzigen

4. U kunt de netwerkinstellingen bekijken en wijzigen, inclusief het netwerk-/subnet waarin de Azure VM zich na failover bevindt en het IP-adres dat eraan wordt toegewezen.

5. In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="failover-to-azure"></a>Failover naar Azure

1. In **instellingen** > **gerepliceerde items**, klikt u op de virtuele machine > **Failover**.
2. In **Failover**, selecteer de **nieuwste** herstelpunt. 
3. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery probeert te doen een afsluiten van de bron-VM's voordat u de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Nadat u de failover controleren, klikt u op **doorvoeren**. De beschikbare herstelpunten worden verwijderd.

> [!WARNING]
> **Annuleer een failover wordt uitgevoerd niet**: als u wordt uitgevoerd, failover gestopt annuleren, maar de virtuele machine wordt niet meer gerepliceerd.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Virtuele machine failback van Azure naar on-premises en omgekeerd repliceren van de on-premises VM

Failback-bewerking is in feite een failover van Azure naar de on-premises site en in omgekeerde replicatie die wordt opnieuw gestart repliceren van virtuele machines van de on-premises site naar Azure.

1. In **instellingen** > **gerepliceerde items**, klikt u op de virtuele machine > **geplande Failover**.
2. In **geplande Failover bevestigen**, Controleer of de failoverrichting (van Azure) en selecteert u de bron- en doellocaties.
3. Selecteer **gegevens voordat de failover synchroniseren (alleen wijzigingen synchroniseren)**. Deze optie minimaliseert downtime van VM's, omdat het synchroniseert zonder dat de virtuele machine wordt afgesloten.
4. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
5. Nadat de initiële synchronisatie is uitgevoerd en u kunt het Azure-VM's klikt u op afsluiten **taken** > geplande-failover-job-name > **volledige Failover**. Deze de Azure-VM wordt afgesloten, brengt de meest recente wijzigingen on-premises en de on-premises VM begint.
6. Meld u op de on-premises virtuele machine om te controleren of dat deze is beschikbaar als verwacht.
7. De on-premises VM is nu in een **in behandeling doorvoeren** staat. Klik op **doorvoeren**. Hiermee verwijdert u de Azure VM's en de schijven en bereidt u de on-premises VM voor omgekeerde replicatie.
Inschakelen om te beginnen met het repliceren van de on-premises VM naar Azure, **omgekeerde repliceren**. Deze wordt geactiveerd voor replicatie van deltawijzigingen die hebben plaatsgevonden sinds de Azure-VM is uitgeschakeld.  
