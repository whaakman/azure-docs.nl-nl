---
title: Failover en failback-Hyper-V-machines gerepliceerd naar een secundair Datacenter tijdens herstel na noodgeval met Azure Site Recovery | Microsoft Docs
description: Leer hoe u Hyper-V virtuele machines een failover naar uw secundaire on-premises site en een failback naar primaire site tijdens herstel na noodgeval met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: ef360fd56c3530a49c2f36846db34b29ca1cf444
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53788015"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Failover en failback-Hyper-V-machines gerepliceerd naar de secundaire on-premises site

De [Azure Site Recovery](site-recovery-overview.md) service beheert en orkestreert replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Dit artikel wordt beschreven hoe u een failover uitvoert voor een Hyper-V-virtuele machine beheerd in een cloud System Center Virtual Machine Manager (VMM) naar een secundaire VMM-site. Nadat u een failover hebt uitgevoerd, valt u terug op uw on-premises site wanneer deze beschikbaar is. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Failover van een Hyper-V virtuele machine uit een primaire VMM-cloud naar een secundaire VMM-cloud
> * Opnieuw beveiligen van de secundaire site naar de primaire en failback
> * U kunt desgewenst starten die opnieuw van de primaire naar secundaire

## <a name="failover-and-failback"></a>Failover en failback

Failover en failback bestaat uit drie fasen:

1. **Failover naar een secundaire site**: Failover machines van de primaire site naar de secundaire server.
2. **Failback van de secundaire site**: Virtuele machines repliceren van de secundaire naar primaire en een geplande failover naar een failback uitgevoerd.
3. Na de geplande failover, optioneel beginnen met het repliceren van de primaire site naar de secundaire opnieuw.


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u hebt een [Dr-herstelanalyse](hyper-v-vmm-test-failover.md) om te controleren of alles werkt zoals verwacht.
- Zorg ervoor dat de primaire en secundaire VMM-servers zijn verbonden met Site Recovery voor het voltooien van failback.



## <a name="run-a-failover-from-primary-to-secondary"></a>Een failover van primaire naar secundaire uitvoeren

U kunt een regelmatig of geplande failover uitvoeren voor Hyper-V-machines.

- Gebruik een normale failover voor onverwachte storingen. Wanneer u deze failover uitvoert, Site Recovery maakt u een virtuele machine op de secundaire site en deze wordt gebruikt door omhoog. Verlies van gegevens kan optreden, afhankelijk van in afwachting van gegevens die nog niet is gesynchroniseerd.
- Een geplande failover kan worden gebruikt voor onderhoud, hetzij tijdens de verwachte uitval. Deze optie heeft geen gegevens verloren gaan. Wanneer een geplande failover wordt geactiveerd, worden de bron-VM's afsluiten. Niet-gesynchroniseerde gegevens worden gesynchroniseerd en de failover wordt geactiveerd. 
- 
Deze procedure wordt beschreven hoe u een reguliere failover uitvoert.


1. Klik in **Instellingen** > **Gerepliceerde items** op de VM > **Failover**.
1. Selecteer **sluit de computer voordat u begint met failover** als u wilt dat Site Recovery probeert een afsluiten van de bron-VM's voordat de failover wordt geactiveerd. Site Recovery wordt ook proberen om te synchroniseren van on-premises gegevens die nog niet nog naar de secundaire site, verzonden voordat de failover wordt geactiveerd. Houd er rekening mee dat de failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
2. U zou nu moeten kunnen zien van de virtuele machine in de secundaire VMM-cloud.
3. Nadat u hebt gecontroleerd dat de virtuele machine, **doorvoeren** de failover. Hiermee verwijdert u alle beschikbare herstelpunten.

> [!WARNING]
> **Annuleer een failover wordt uitgevoerd niet**: De VM-replicatie wordt gestopt voordat de failover is gestart. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.  


## <a name="reverse-replicate-and-failover"></a>Omgekeerde replicatie en failover

Beginnen met het repliceren van de secundaire site naar de primaire en een failback uitvoeren naar de primaire site. Wanneer virtuele machines worden uitgevoerd op de primaire site opnieuw, kunt u ze kunt repliceren naar de secundaire site.  

 
1. Klik op de virtuele machine > Klik op **omgekeerde repliceren**.
2. Nadat de taak voltooid is, klikt u op de virtuele machine > In **Failover**, Controleer of de failoverrichting (van secundaire VMM-cloud) en selecteert u de bron- en doellocaties. 
4. Start de failover. U kunt de voortgang van de failover volgen op het tabblad **Taken**.
5. Controleer of de virtuele machine beschikbaar is in de primaire VMM-cloud.
6. Als u beginnen met het repliceren van de primaire virtuele machine terug naar de secundaire site opnieuw wilt, klikt u op **omgekeerde repliceren**.

## <a name="next-steps"></a>Volgende stappen
[Bekijk de stap](hyper-v-vmm-disaster-recovery.md) voor Hyper-V-machines repliceren naar een secundaire site.
